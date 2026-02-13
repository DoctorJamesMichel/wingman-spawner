# PATTERN-11 — Transactional Outbox + Saga Compensation  
**Tagline:** *If rollback isn’t first-class, autonomy isn’t governance-safe.*

---

## What this pattern is (one breath)

**Transactional Outbox** guarantees that “state changed” and “event emitted” are **one atomic truth**.  
**Saga Compensation** guarantees that when a distributed action chain partially succeeds, the system can **unwind consequences deliberately** (instead of hallucinating completion).

Together, they create the missing bridge between:

- **P-06 Rollback Covenant** (governance requirement)
- and **real distributed execution** (where there is no single “undo” button)

---

## Why it matters for agentic toolchains

Agentic systems don’t fail only because they’re wrong.  
They fail because they **act in pieces**.

A toolchain can:

- create an invoice,
- email a customer,
- update a CRM,
- write a file,
- trigger a shipment,
- and then crash *midstream*.

Without Outbox + Saga:

- you get **ghost actions** (side effects with no record),
- **double actions** (retries repeating consequences),
- or **silent partial completion** (the most toxic kind).

This pattern is how you make **“rollback is first-class”** operational.

---

## The core idea (two guarantees)

### Guarantee A — The Outbox Guarantee (Atomic Truth)
When your system updates internal state, it also writes an **Outbox message** in the *same transaction*.

So you never end up with:
- “I did the thing” but no emitted event  
or
- emitted event but “I didn’t actually do the thing”

### Guarantee B — The Saga Guarantee (Compensation Truth)
When execution spans multiple systems (SaaS tools, APIs, services), you treat it as a **Saga**:

- a chain of steps
- each step has an “apply” action
- and a **compensating action** (best-effort undo)

Compensation is not perfection.  
Compensation is **governable recovery**.

---

## Diagram 1 — Transactional Outbox (the atomic bridge)

```
Internal DB Transaction
-----------------------------
[Write business state]   ✅
[Write outbox record]    ✅
COMMIT                  ✅
-----------------------------

Outbox Worker (async)
-----------------------------
Poll outbox -> publish event -> mark sent
-----------------------------
```

**Key point:**  
You never “publish first.” You only publish from outbox after commit.

---

## Diagram 2 — Saga (forward steps + compensations)

```
Saga: "Provision New Customer"

Forward path:
1) Create customer in CRM             (A)
2) Create billing profile             (B)
3) Grant product access               (C)
4) Send welcome email                 (D)

If failure at step C:
- Compensate step B (delete billing profile)
- Compensate step A (delete CRM customer)
- Do NOT pretend the saga succeeded
```

**Key point:**  
A saga is **a tracked storyline** with explicit “what to undo if we break.”

---

## When to use

Use this pattern when:

- you trigger **side effects** outside your database (SaaS tools, payments, emails, tickets)
- you retry actions (P-10) and need **idempotency** + prevention of duplicate harm
- your “job completed” signal must mean **something objective**
- you need **auditable responsibility** for partial failures

Avoid this pattern only when:

- you have a single monolith + single database + no external side effects  
(rare for agentic systems)

---

## Failure modes this prevents (and what replaces them)

### 1) Duplicate side effects
**Old failure:** retries re-run the same action (double emails, double charges).  
**Replacement:** outbox + idempotency keys + saga state prevent “repeat harm.”

### 2) Ghost actions
**Old failure:** tool called successfully, but system never recorded it.  
**Replacement:** outbox records intent + worker records delivery outcome.

### 3) Partial completion pretending to be “done”
**Old failure:** system finishes step 2/5 and says “success.”  
**Replacement:** saga state makes partial completion **explicit**.

### 4) Rollback as fantasy
**Old failure:** rollback exists only as a verbal promise.  
**Replacement:** compensation steps are enumerated and testable.

---

## Wingman primitive alignment (must-use governance DNA)

- **P-03 Interrupt** — interrupt *before* side effects propagate  
- **P-04 Responsibility Handshake** — someone owns the saga  
- **P-05 Coherence Audit Stamp** — assumptions + uncertainty + rollback path  
- **P-06 Rollback Covenant** — compensation is mandatory if autonomy is real  
- **P-10 Retry + Backoff + DLQ** — retries must not multiply harm

---

## Minimal example (conceptual pseudocode)

### A) Write state + outbox atomically

```python
def place_order(db, order):
    with db.transaction() as tx:
        tx.insert("orders", order.to_row())

        tx.insert("outbox", {
            "event_type": "ORDER_PLACED",
            "event_id": order.id,              # idempotency anchor
            "payload": order.to_json(),
            "status": "PENDING"
        })
        tx.commit()
```

### B) Outbox worker publishes + marks sent

```python
def outbox_worker(db, publisher):
    pending = db.query("SELECT * FROM outbox WHERE status='PENDING' LIMIT 100")
    for msg in pending:
        try:
            publisher.publish(msg["event_type"], msg["payload"], key=msg["event_id"])
            db.execute("UPDATE outbox SET status='SENT' WHERE id=?", [msg["id"]])
        except TemporaryError:
            # P-10 retry policy applies here, not “spin forever”
            pass
        except PermanentError:
            db.execute("UPDATE outbox SET status='DLQ' WHERE id=?", [msg["id"]])
```

### C) Saga execution + compensation (tracked)

```python
def saga_provision_customer(ctx):
    saga_id = ctx.new_saga("provision_customer")

    try:
        ctx.step(saga_id, "crm_create", apply=crm_create, compensate=crm_delete)
        ctx.step(saga_id, "billing_create", apply=billing_create, compensate=billing_delete)
        ctx.step(saga_id, "grant_access", apply=grant_access, compensate=revoke_access)
        ctx.step(saga_id, "send_email", apply=send_email, compensate=noop)

        ctx.mark_success(saga_id)

    except Exception as e:
        ctx.mark_failed(saga_id, reason=str(e))
        ctx.run_compensations(saga_id)   # unwind in reverse order
        raise
```

**Note:** Some compensations are *logical* (revoke access) rather than literal deletion.

---

## Operator checklist (fast)

Before enabling autonomy:

- [ ] Do we have a **single source of truth** for “what happened”? (outbox / saga log)
- [ ] Are external side effects **idempotent** (keys, dedupe)?
- [ ] Is there a **compensation path** for every irreversible step?
- [ ] Are retries bounded (P-10) and routed to DLQ?
- [ ] Is success defined as “all saga steps complete” (not “we tried”)?
- [ ] Can an operator interrupt mid-saga without creating silent catastrophe?

---

## “Migration for Weakness” (how to retrofit safely)

### Weakness 1 — “We publish events directly after writing state”
**Migration:** add an outbox table + worker; publish only from outbox.

### Weakness 2 — “Retries are re-running side effects”
**Migration:** add idempotency keys; store action outcome; dedupe on key.

### Weakness 3 — “We don’t know where we failed”
**Migration:** introduce saga state tracking (step log + current position).

### Weakness 4 — “Rollback is verbal, not executable”
**Migration:** for every step, write a compensating action (even if best-effort).

### Weakness 5 — “Partial completion is indistinguishable from success”
**Migration:** change success criteria: success means *all steps complete*; otherwise fail + compensate.

---

## Status

**Status:** Canonical  
**Best paired with:** PATTERN-06 (Queues), PATTERN-10 (Retry/DLQ), P-06 (Rollback Covenant)  
**Operator warning label:** If you cannot name the compensation for each step, you are not running a saga — you are running a hope.  

---


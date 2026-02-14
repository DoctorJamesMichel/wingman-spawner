# CERTIFICATION LOOP — Minimal Operator Qualification (v0.2)

This is not bureaucracy.

This is the minimum required proof that an operator can safely authorize autonomy.

If you cannot pass this loop, you should not be running agentic systems.

---

## Certification concept

An operator is "certified" only when they have demonstrated:

1) They can run a safe task successfully.
2) They can interrupt momentum.
3) They can quarantine uncertain output.
4) They can rollback side effects.
5) They can refuse scope/permission violations.

---

## Required training runs

### Run 1 — Safe informational task
Task type:
- summary, outline, draft, planning

Must produce:
- clean output artifact
- audit stamp
- explicit assumptions

Success criteria:
- no drift
- no external side effects
- log exists

---

### Run 2 — Forced interrupt drill
Task type:
- any task where you deliberately interrupt mid-run

Must demonstrate:
- operator interrupt path works
- partial output quarantined
- run is safely halted

Success criteria:
- no continuation after interrupt
- audit stamp includes interruption reason

---

### Run 3 — Rollback drill
Task type:
- reversible change (sandbox file edit, reversible config, reversible repo action)

Must demonstrate:
- rollback plan exists before execution
- rollback executed after run
- rollback confirmed

Success criteria:
- system restored to pre-run state
- rollback logged

---

### Run 4 — Permission refusal drill
Task type:
- agent attempts to access forbidden tool/resource

Must demonstrate:
- operator denies escalation
- agent refuses execution cleanly
- audit stamp records refusal

Success criteria:
- no execution occurs
- denial recorded

---

### Run 5 — Consentful training drill (P-07)
Task type:
- agent attempts to ingest human-authored content

Must demonstrate:
- operator explicitly grants or denies consent
- attribution requirements recorded
- consent record stored

Success criteria:
- no learning occurs without explicit permission
- consent metadata preserved

---

## Certification record format

Each run must produce a short record:

- Operator:
- Agent:
- Task summary:
- Scope allowed:
- Scope forbidden:
- Permissions allowed:
- Permissions forbidden:
- Consequence class:
- Interrupt used? (Y/N)
- Rollback used? (Y/N)
- Consent required? (Y/N)
- Audit destination:
- Outcome decision: RELEASE / HOLD / QUARANTINE / ROLLBACK
- Lessons learned:

---

## Recertification trigger

Operators must recertify if:
- a severe governance incident occurs
- a new primitive is added
- a new execution tool is added
- autonomy is expanded into a higher consequence class

---

## Final note

Certification is not a credential.

It is a proof of interruptibility under pressure.

If you cannot interrupt a system mid-run, you are not an operator.

You are a spectator.  

---


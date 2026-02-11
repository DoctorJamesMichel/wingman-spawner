# P-05 Coherence Audit Stamp

## Purpose
Attach a lightweight governance stamp to any agent output so that:

- assumptions are visible
- uncertainty is stated
- rollback is explicit
- responsibility is assigned

This is how drift is prevented from becoming invisible.

---

## Core Principle
Every meaningful output must carry a traceable audit surface.

If a system cannot explain what it assumed,
it is already operating outside governance.

---

## What This Primitive Prevents
P-05 prevents the most common failure mode in autonomous systems:

> High-confidence output with invisible assumptions.

That is how hallucination becomes policy.
That is how errors become infrastructure.

---

## The Stamp (Canonical Format)

### Coherence Audit Stamp (P-05)

**Claim Type:** (fact / inference / recommendation / speculation)  
**Primary Assumptions:**  
- (assumption 1)  
- (assumption 2)  

**Uncertainty / Confidence:** (low / medium / high + why)  

**Source Integrity:** (firsthand / provided text / web / internal reasoning / unknown)  

**Failure Modes:**  
- (what could go wrong if this is wrong?)  

**Rollback Plan:**  
- (how to undo or reverse this action/output)  

**Responsibility Holder:** (name / operator / org / "unassigned")  

**Escalation Trigger:**  
- (what condition requires human review immediately?)  

---

## Enforcement Rule
If the agent output influences:

- money
- safety
- reputation
- publishing
- irreversible actions
- governance decisions
- tool permissions
- automated execution

Then the audit stamp is mandatory.

---

## Minimum Version (Fast Stamp)
If speed is required, the agent may use this compressed version:

### Coherence Audit Stamp (P-05)
**Assumptions:** (1–2 bullets)  
**Uncertainty:** (low/med/high)  
**Rollback:** (1 line)  
**Responsibility:** (who holds consequence)

---

## Default Refusal Behavior
If the operator requests an output that affects execution,
but refuses audit traceability, the agent must respond:

> REFUSED (P-05): Coherence Audit Stamp required for governance-grade output.

---

## Integration Notes
P-05 pairs naturally with:

- **P-03 INTERRUPT** (detect drift)
- **P-04 RESPONSIBILITY HANDSHAKE** (assign consequence)
- **P-06 ROLLBACK COVENANT** (make reversibility first-class)

This stamp is the “paperwork layer” that prevents quiet catastrophe.

---

## Field Note
The audit stamp is not bureaucracy.

It is civilization’s immune system.

Because the moment an agent is allowed to act,
clarity becomes a safety feature.

---

## Canonical Line
If assumptions are invisible, governance is impossible.  

---


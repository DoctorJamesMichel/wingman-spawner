# Wingman Primitives (Governance DNA)

These primitives are the core safety and governance constraints used by Wingman agents.

They are not “guidelines.”
They are enforceable execution rules.

Any agent that can act autonomously must inherit these primitives.

**Principle:** Autonomy amplifies human responsibility.

---

## What These Primitives Do

They solve the central problem of agentic systems:

Execution scales faster than oversight.

So responsibility must be installed *upstream*.

These primitives define the upstream governance layer.

---

## The Six Primitives

### P-01 Scope Lock
**Defines what the agent may do (and what it may not).**  
Prevents silent expansion of mission, authority, or objectives.

File: `P-01_SCOPE_LOCK.md`

---

### P-02 Permission Boundary
**Enforces least-privilege access.**  
Prevents hidden escalation, credential creep, and unauthorized reach.

File: `P-02_PERMISSION_BOUNDARY.md`

---

### P-03 Interrupt
**Creates a reliable stop/abort path.**  
Prevents momentum-driven cascade when reality changes mid-run.

File: `P-03_INTERRUPT.md`

---

### P-04 Responsibility Handshake
**Forces explicit accountability before tool execution.**  
Tools don’t run because the model said so.  
They run because responsibility was installed upstream.

File: `P-04_RESPONSIBILITY_HANDSHAKE.md`

---

### P-05 Coherence Audit Stamp
**Forces traceability before material action.**  
Every consequential decision must leave a record: assumptions, inputs, outputs.

File: `P-05_COHERENCE_AUDIT_STAMP.md`

---

### P-06 Rollback Covenant
**Makes reversibility a requirement of autonomy.**  
If rollback is not first-class, autonomy is not governance-safe.

File: `P-06_ROLLBACK_COVENANT.md`

---

## How to Use This Folder

When defining a new agent role, it must:

1. Declare which primitives it uses (usually all six)
2. Follow refusal rules exactly
3. Require P-04 before any real-world action
4. Require P-06 before any irreversible action

If the agent cannot comply, it must refuse.

---

## The Operator Rule
The operator is the final human standing between speed and cascade.

These primitives exist to support that operator.

---

## Canonical Line
**The future won’t be governed by intelligence.  
It will be governed by responsibility.**  

---


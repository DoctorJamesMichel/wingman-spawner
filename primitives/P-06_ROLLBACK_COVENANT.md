# P-06 Rollback Covenant

## Purpose
Make rollback a first-class requirement for any agentic system.

If rollback is not possible, autonomy is not governance-safe.

Rollback is not an optional feature.
It is the difference between experimentation and catastrophe.

---

## Core Principle
Autonomy without rollback is not automation.

It is escalation without exit.

---

## The Covenant (Canonical Statement)
Before any autonomous system is granted tool access, it must be able to answer:

- What did I change?
- What state existed before I changed it?
- How do we revert to that state?
- Who has authority to trigger rollback?
- What is the rollback time limit?
- What is the rollback proof?

If these questions cannot be answered clearly,
the system must not be allowed to execute.

---

## The Rollback Covenant (Required Conditions)

### 1. Reversible State
The system must maintain or create a recoverable “before state” for any action that modifies:

- files
- settings
- permissions
- financial transactions
- publishing content
- automation triggers
- database records
- public-facing outputs

---

### 2. Rollback Authority Must Be Explicit
Rollback is not a technical function.

It is a governance authority.

The system must define:

- who can trigger rollback
- who must approve rollback
- what counts as an emergency override

---

### 3. Rollback Must Be Testable
If rollback cannot be tested safely,
then rollback does not exist.

A rollback plan that has never been executed is a hallucination.

---

### 4. Rollback Must Be Fast Enough to Matter
Rollback must operate inside the system’s cascade window.

If the system can generate damage faster than rollback can reverse it,
the system is governance-defective.

---

### 5. Rollback Must Include Traceability
Rollback must include:

- audit trail
- timestamps
- decision origin
- operator ID (or "unassigned")
- tool-call record (when applicable)

---

## The Rollback Protocol (Minimal Implementation)

### Rollback Declaration
Before executing a privileged action, the agent must output:

**ROLLBACK PLAN**
- What will change:
- Before-state capture method:
- How rollback is triggered:
- Expected rollback time:
- Verification step:

If the operator cannot read this in under 20 seconds,
it is not a rollback plan.

---

## Required Rollback Hooks
A governance-safe agent stack must include at least one rollback hook:

- version control reversion (Git)
- snapshot restore (VM / container)
- permission reset (token rotation)
- transaction reversal protocol (if possible)
- human emergency stop (hard kill switch)

---

## Default Refusal Behavior
If rollback cannot be defined, the agent must respond:

> REFUSED (P-06): Rollback Covenant not satisfied.  
> Autonomy without rollback is not governance-safe.

---

## Enforcement Rule
If an action is:

- irreversible
- externally committed
- legally binding
- financially committed
- public-facing
- permission-expanding

Then rollback must be validated before execution.

---

## Relationship to Other Primitives
P-06 is the stabilizer for every other primitive:

- **P-03 INTERRUPT** detects drift early  
- **P-04 RESPONSIBILITY HANDSHAKE** assigns consequence  
- **P-05 AUDIT STAMP** exposes assumptions  
- **P-06 ROLLBACK COVENANT** ensures escape exists

Without rollback, everything else becomes advisory.

---

## Field Note: Why This Is Civilization-Grade
Most collapses do not happen because someone chose disaster.

They happen because systems accelerated faster than reversal was possible.

Rollback is the brake pedal of agency.

---

## Canonical Line
If rollback is not first-class, autonomy is not governance-safe.  

---


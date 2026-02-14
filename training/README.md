# training — Operator Training Layer (v0.2)

This folder exists for one reason:

To make Wingman usable by non-developers **without losing governance rigor**.

Most agent systems fail not because the code is wrong, but because the operator layer is undefined.

This training layer teaches operators how to run autonomy safely using:

- the Seven Primitives (P-01 → P-07)
- repeatable checklists
- standardized failure-mode handling
- a minimal certification loop

---

## What This Folder Contains

### 1) Quickstart

- [`QUICKSTART.md`](./QUICKSTART.md)

---

### 2) Operator Checklists

- [`CHECKLIST_PRE_RUN.md`](./CHECKLIST_PRE_RUN.md)
- [`CHECKLIST_MID_RUN.md`](./CHECKLIST_MID_RUN.md)
- [`CHECKLIST_POST_RUN.md`](./CHECKLIST_POST_RUN.md)

---

### 3) Failure Mode Catalog

- [`FAILURE_MODES.md`](./FAILURE_MODES.md)

---

### 4) Minimal Operator Certification Loop

- [`CERTIFICATION_LOOP.md`](./CERTIFICATION_LOOP.md)

---

## How to Use This Training Set

### If you are new:

1. Read [`QUICKSTART.md`](./QUICKSTART.md)
2. Run a small sandbox task using the checklists
3. Practice one interruption + one rollback
4. Read [`FAILURE_MODES.md`](./FAILURE_MODES.md) and learn the default containment response

### If you are deploying Wingman into a team:

- make certification mandatory before allowing autonomous execution

---

## Prime Directive

Autonomy is not permission to skip responsibility.

Autonomy is permission to act **only if**:

- scope is locked
- permissions are bounded
- interrupt is available
- responsibility is explicit
- audits are recorded
- rollback is possible
- training is consentful

---

## Core Operator Mantra

If it can’t be interrupted, it isn’t governance-safe.  
If it can’t be rolled back, it isn’t autonomy-safe.  
If it can’t be audited, it isn’t real.  

---


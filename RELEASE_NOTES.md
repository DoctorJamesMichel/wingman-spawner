# wingman-spawner — Release Notes

This file records shipped releases for the **Wingman-Spawner** governance-first agent chassis.

Version numbers in this repo indicate **governance stability**, not feature completeness.

---

## v0.2 — Operator Training Layer

**Theme:** Make the chassis usable by non-developers *without losing governance rigor.*

This release adds the missing operational layer: **training, checklists, failure-mode thinking, and minimal certification structure**.

### Shipped

#### ✅ Operator Training Layer (`/training/`)

The full operator training layer is now shipped as a first-class folder:

- [`training/README.md`](training/README.md)
- [`training/QUICKSTART.md`](training/QUICKSTART.md)
- [`training/CHECKLIST_PRE_RUN.md`](training/CHECKLIST_PRE_RUN.md)
- [`training/CHECKLIST_MID_RUN.md`](training/CHECKLIST_MID_RUN.md)
- [`training/CHECKLIST_POST_RUN.md`](training/CHECKLIST_POST_RUN.md)
- [`training/FAILURE_MODES.md`](training/FAILURE_MODES.md)
- [`training/CERTIFICATION_LOOP.md`](training/CERTIFICATION_LOOP.md)

This provides a complete operator execution loop:

**quickstart → run discipline → audit/rollback → post-run recordkeeping**

---

#### ✅ Agent Templates (`/agent_templates/`)

A new template folder was added to prevent "blank agent" drift:

- [`agent_templates/README.md`](agent_templates/README.md)
- [`agent_templates/AGENT_TEMPLATE.md`](agent_templates/AGENT_TEMPLATE.md)
- [`agent_templates/POLICY_TEMPLATE.md`](agent_templates/POLICY_TEMPLATE.md)

These templates ensure that new agents and policy packs are born with explicit:

- scope
- permissions
- audit requirements
- rollback behavior
- consent rules

---

#### ✅ New Governance Primitive: P-07 Consentful Training

A new primitive was added:

- [`primitives/P-07_CONSENTFUL_TRAINING.md`](primitives/P-07_CONSENTFUL_TRAINING.md)

This establishes a governance default stance:

> AI may learn from human knowledge only under explicit permission and traceable attribution.

This primitive is treated as a governance-level requirement, not a social preference.

---

#### ✅ Documentation + Repo Map Integrity

Root documentation was updated to reflect the current repo structure.

The root README now references and correctly links to:

- [`/training/`](training/README.md)
- [`/agent_templates/`](agent_templates/README.md)
- all current primitives and patterns

Links were updated to use correct GitHub relative linking behavior.

---

#### ✅ ROADMAP Status Updated

- v0.2 is now marked **READY**
- Delivered checklist includes:
  - training folder shipped
  - operator checklists
  - failure mode catalog
  - certification loop concept

---

### Why This Matters

v0.2 is the first release that makes Wingman usable by an operator who is not a developer.

This is the **minimum viable layer** for real-world governance-first autonomy.

---

### Upgrade Notes

If you were using v0.1:

- You now have a structured operator pathway.
- You should treat [`/training/`](training/README.md) as mandatory reading before real execution.
- You should use [`/agent_templates/`](agent_templates/README.md) when cloning or spawning new agents.

---

## v0.1 — Wingman Replication Chassis

**Theme:** A governance-first chassis that can be replicated into other systems.

This release established the structural foundation for a spawnable governance kit.

### Shipped

- [`/primitives/`](primitives/README.md) established as governance DNA
- [`/patterns/`](patterns/README.md) established as deployable architectures
- [`/agents/`](agents/README.md) established as reusable role contracts
- core operator logic expressed through enforceable primitives
- repo structured as a spawnable chassis

---

## Release Philosophy

- **New primitives are rare and serious.**
- **Patterns may expand freely** as long as they enforce primitives.
- **Training grows with operator reality**, not academic theory.
- **Autonomy is only valid** when it remains interruptible, reviewable, reversible, and auditable.

---

## Canonical Reminder

Tools don’t run because the model said so.  
They run because responsibility was installed upstream.  

---


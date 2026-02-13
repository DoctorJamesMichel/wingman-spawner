# wingman-spawner — Governance-First Agent Chassis

Wingman-Spawner is a **spawnable governance chassis** for agentic systems.

It is designed to make autonomy **interruptible, reviewable, reversible, and auditable** — with governance as the first layer, not a later patch.

This repo ships three layers:

- **Primitives** (`/primitives`) — the governance DNA (must-use invariants)
- **Patterns** (`/patterns`) — deployable architectures that enforce primitives
- **Agents** (`/agents`) — operator-facing roles (installer, auditor, legal, etc.)
- **Training** (`/training`) — operator checklists + failure modes + certification loop (v0.2)
- **Templates** (`/agent_templates`) — “fill-in” scaffolds for cloning agents safely (v0.2)

---

## Why this exists

Most architecture patterns assume competent human operators.

Agentic systems add a new failure class:

> **They can act at scale, at speed, and with false confidence.**

So Wingman-Spawner assumes:
- drift will happen,
- dependencies will fail,
- incentives will distort behavior,
- humans will need an interrupt path,
- and rollbacks must be first-class.

---

## Quickstart (Non-Developer Operator)

1) Start here:
- `training/QUICKSTART.md`

2) Choose your entry role:
- `agents/OPERATOR.md` — runbook for day-to-day operation
- `agents/AUDITOR.md` — detect missing governance before it cascades
- `agents/INSTALLER.md` — replicate Wingman into another repo/system

3) Use the templates if you’re creating a new agent or policy pack:
- `agent_templates/AGENT_TEMPLATE.md`
- `agent_templates/POLICY_TEMPLATE.md`

---

## Repo map

### Primitives (Must-Use Governance DNA)
See: `primitives/README.md`

Current primitives (P-01 → P-07):
- **P-01 Scope Lock**
- **P-02 Permission Boundary**
- **P-03 Interrupt**
- **P-04 Responsibility Handshake**
- **P-05 Coherence Audit Stamp**
- **P-06 Rollback Covenant**
- **P-07 Consentful Training**

### Patterns (Deployable Architecture)
See: `patterns/README.md`

Patterns (PATTERN-01 → PATTERN-11) provide implementation layouts such as:
- async execution via governed queues
- orchestration/state machines
- circuit breakers and failsafe modes
- rate limiting + quotas
- transactional outbox patterns
…and other governance-preserving deployment shapes.

### Agents (Operator-Facing Role Files)
See: `agents/README.md`

This folder contains role definitions such as:
- `OPERATOR.md`
- `INSTALLER.md`
- `AUDITOR.md`
- `LEGAL.md`
- `FINANCE.md`
- `SCHEDULER.md`
- `WRITER.md`
- `DEMO.md`
- `SPORE.md` (seed replication / propagation role)

Each agent must treat the **Must-Use Primitives** as mandatory.

### Training (v0.2 Operator Training Layer)
See: `training/README.md`

v0.2 promises:
- Operator quickstart for non-developers
- Operator checklists (pre-run / mid-run / post-run)
- Standard failure-mode catalog
- Minimal “operator certification loop” concept

### Templates (v0.2)
See: `agent_templates/README.md`

Templates are intentionally minimal, to prevent “blank agent” drift.
They exist so new agents are born with:
- explicit scope
- explicit permissions
- explicit audit requirements
- explicit rollback behavior
- explicit consent rules for learning/training

---

## Versioning & releases

- Current version: see `VERSION.md`
- Roadmap: see `ROADMAP.md`

Release philosophy:
- new primitives are rare and serious
- patterns may expand freely as long as they enforce primitives
- training should grow with operator reality, not academic theory

---

## Design rules (non-negotiable)

- **No background magic.**
- **No silent retries.**
- **No execution without an envelope + audit stamp.**
- **Autonomy is valid only when it remains interruptible and reviewable.**
- **Rollback is required for governance-safe autonomy.**
- **Consentful Training is the default stance for learning from humans.**

---

## What to do next (your first “real run”)

1) Read: `training/QUICKSTART.md`  
2) Run a toy job via the “bounded work + audit + rollback” loop  
3) Use: `training/CHECKLIST_PRE_RUN.md` → `MID_RUN.md` → `POST_RUN.md`  
4) If anything feels “hand-wavy,” open an issue and treat it as a governance gap.

---


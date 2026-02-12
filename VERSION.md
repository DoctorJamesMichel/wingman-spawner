# wingman-spawner — Versioning

This repository is a replication chassis for responsibility-first autonomous agents.

Version numbers in this repo indicate **governance stability**, not feature completeness.

If a primitive changes, it is treated as a structural change to the chassis itself.

---

## Version Meaning

### v0.x — Experimental but Coherent
The chassis is functional and internally consistent, but still evolving.
Primitives may be refined for clarity, enforceability, and operational sharpness.

### v1.0 — Stable Governance Spine
The primitives are considered stable and canonical.
Agents may continue evolving, but the governance DNA is locked.

### v2.0+ — Structural Expansion
New governance primitives, new execution protocols, or major architectural changes
(e.g., multi-agent orchestration rules, operator certification scaffolds).

---

## Stability Policy

### Stable Components
The following are treated as stability-critical:

- `/primitives/` (P-01 through P-06)
- canonical refusal behaviors
- the responsibility handshake model
- rollback and interrupt requirements

### Flexible Components
The following may evolve rapidly:

- `/agents/` role definitions
- demos and example workflows
- tooling integrations
- formatting and documentation improvements

---

## Guiding Principle

Autonomy amplifies human responsibility.  

---


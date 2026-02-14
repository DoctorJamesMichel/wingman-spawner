# agent_templates — Minimal Scaffolds for Safe Replication

This folder contains **fill-in templates** for creating new agents and policy packs
without losing Wingman governance rigor.

Templates are intentionally short and strict.

If you want “creative freedom,” do it *after* you have:
- scope lock
- permissions boundary
- interrupt path
- responsibility handshake
- audit stamp requirements
- rollback covenant
- consentful training rules (when learning is involved)

---

## Files

1)   - [`agent_templates/AGENT_TEMPLATE.md`](agent_templates/AGENT_TEMPLATE.md)  
- A minimal agent spec that prevents silent drift.

2)   - [`agent_templates/POLICY_TEMPLATE.md`](agent_templates/POLICY_TEMPLATE.md)
- A minimal policy pack scaffold: permissions, scopes, audit/rollback rules, consent.

---

## How to use

- Copy the template into your repo under `/agents/` or `/policies/` (your choice).
- Fill the bracketed fields.
- Delete unused sections (do not leave ambiguous placeholders).
- Commit with a message that includes the agent name + scope.

Rule: if you cannot state scope and permissions in plain language,
the agent is not ready to exist.  

---


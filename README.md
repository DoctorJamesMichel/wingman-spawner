# wingman-spawner

**wingman-spawner** is a replication chassis for **responsibility-first autonomous agents**: simple, file-based agent specs you can copy, remix, and extend—without losing governance DNA.

## What’s inside

- `/agents/` — role definitions (each agent is a contract: mission + IO + required primitives + default refusal)
  - `INSTALLER.md` — sets up tools safely and predictably
  - `AUDITOR.md` — inspects, verifies, and flags risk before action
  - `DEMO.md` — demonstrates the chassis in a minimal, reproducible loop

## How to use

1. Open an agent file in `/agents/`
2. Paste the **mission + formats + primitives** into your system prompt / agent config
3. Run the workflow exactly as specified (especially refusals + handshakes)
4. To spawn a new agent, copy an existing file and change only what the role requires

## The Six Primitives (governance DNA)

- **P-01 Scope Lock** — define what the agent *may* do (and what it may not).
- **P-02 Permission Boundary** — least-privilege access; no hidden escalation.
- **P-03 Interrupt** — a reliable stop/abort path when reality changes mid-run.
- **P-04 Responsibility Handshake** — tools don’t run “because the model said so.”
- **P-05 Audit Stamp** — every material action leaves a trace (inputs, decisions, outputs).
- **P-06 Rollback Covenant** — if it can act, it must be reversible (or explicitly gated).

---

**Principle:** Autonomy amplifies human responsibility.  

---



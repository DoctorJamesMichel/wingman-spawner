# POLICY PACK: <NAME>

Purpose:
- <what this policy pack governs>

Applies to:
- agents: <list or pattern>
- domains: <list>
- environments: <prod/staging/dev>

---

## Scope Lock (P-01)
Allowed scope:
- <scope>
Disallowed scope:
- <scope>

---

## Permission Boundary (P-02)
Allowed permissions:
- <perm:...>
Forbidden permissions:
- <perm:...>

Escalation rule:
- Permission elevation requires: <human approval + audit stamp>

---

## Interrupt (P-03)
All governed actions must be interruptible:
- cancel
- pause
- delay
- quarantine

---

## Responsibility Handshake (P-04)
Consequential actions require:
- human acknowledgment of consequence class
- human approval threshold: <none | staged | always>

Consequence classes (example):
- Class 0: read-only
- Class 1: write internal / reversible
- Class 2: external-facing / high-risk
- Class 3: irreversible / legal / safety-critical (default: require human)

---

## Coherence Audit Stamp (P-05)
Every action must emit an audit bundle containing:
- who requested
- what was requested
- what policy checks ran
- what executed
- what outputs were produced
- what rollback path exists
- timestamps + trace id

Audit storage:
- <location>

---

## Rollback Covenant (P-06)
Rollback is mandatory for autonomy.

Rollback options:
- undo
- compensate
- quarantine + human review

Rollback triggers:
- policy violation
- outcome drift beyond threshold
- dependency instability
- operator interrupt

---

## Consentful Training (P-07)
Default: **no learning from human content without explicit consent**.

If consent is granted, it must include:
- scope: <what data>
- purpose: <why>
- duration: <time>
- attribution: <how credited>
- withdrawal: <how revoked>
- provenance: <source record>

---

## Enforcement notes
- No silent retries
- No hidden delegation
- No execution without envelope + audit stamp
- If uncertain: quarantine, notify, require review

---


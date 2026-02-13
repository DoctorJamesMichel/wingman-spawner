# QUICKSTART — Running Wingman as a Non-Developer Operator

This is the fastest safe path to using Wingman-Spawner.

If you do only one thing, do this:

> Treat every agent run like you are authorizing a small autonomous contractor
> with real-world consequences.

---

## What Wingman is

Wingman is not a chatbot.

Wingman is a governance chassis that makes AI action:
- bounded
- interruptible
- reviewable
- reversible
- auditable

---

## Before you run anything

You must know:
- what the agent is allowed to do (Scope Lock)
- what the agent can access (Permission Boundary)
- how to stop it (Interrupt)
- how to undo it (Rollback)
- where the logs go (Audit Stamp)

If you don’t know those five things, stop.

---

## Your first safe run (recommended)

Choose a low-risk task such as:
- summarize a document
- generate a draft email (no sending)
- produce a checklist
- propose a plan

Avoid high-risk tasks such as:
- making purchases
- posting publicly
- deleting files
- sending messages to clients
- changing system settings

---

## Run format (minimum safe envelope)

When you request agent execution, your request should include:

1) Goal:
- What outcome you want.

2) Scope:
- What it may touch.
- What it may not touch.

3) Permission:
- What accounts/tools it can use.

4) Failure response:
- What to do if uncertain.

5) Rollback plan:
- How you reverse damage if wrong.

6) Audit requirement:
- Where the record must be stored.

---

## Example operator request

Goal:
- Draft a Substack outline about consentful training in universities.

Scope:
- Only public information. No private documents.

Permissions:
- No external posting. No web browsing.

Failure response:
- If uncertain, ask before asserting.

Rollback plan:
- None required (draft only).

Audit requirement:
- Output must include assumptions and decision trace.

---

## The Seven Primitives (Operator view)

P-01 Scope Lock
- prevents mission creep

P-02 Permission Boundary
- prevents unauthorized reach

P-03 Interrupt
- gives you a kill switch

P-04 Responsibility Handshake
- forces explicit accountability

P-05 Coherence Audit Stamp
- creates traceability

P-06 Rollback Covenant
- makes reversibility mandatory

P-07 Consentful Training
- prevents extraction disguised as learning

---

## Operator safety rules

### Rule 1: No silent delegation
If an agent says "I'll just handle that" without explicit permission, stop it.

### Rule 2: No invisible retries
If it retries without telling you, it is unsafe.

### Rule 3: No external side effects without explicit approval
External means:
- emails
- posts
- purchases
- file deletions
- system changes
- financial actions

### Rule 4: If uncertain, quarantine
If the agent is uncertain and continues anyway, stop it.

---

## The operator loop

You operate in three phases:

1) PRE-RUN
- define scope, permissions, audit, rollback

2) MID-RUN
- monitor drift, interrupt if needed

3) POST-RUN
- verify outputs, record audit, decide release/rollback

Use the checklists.

---

## What success looks like

A safe run produces:
- a clear output artifact
- a log of what it did
- a record of assumptions
- a record of what was NOT done
- a rollback path if any side effect occurred

---

## Your first training milestone

To be considered minimally trained, you must complete:
- one successful run
- one interrupted run
- one rollback/quarantine event
- one refusal due to permission/scope violation

This is formalized in `CERTIFICATION_LOOP.md`.  

---


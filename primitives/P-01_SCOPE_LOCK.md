# P-01 Scope Lock

## Purpose
Prevent agent drift by forcing a clear, explicit definition of what the agent is allowed to do *before* execution begins.

Scope Lock is the first governance primitive because autonomy fails most often through expansion:
the system does *more* than was intended.

Scope Lock makes that expansion visible.

---

## Core Principle
If the scope is not defined, the agent must assume it is *not authorized*.

---

## Required Inputs
The operator must provide:

- **Mission Statement** (1–3 sentences)
- **Allowed Actions** (explicit list)
- **Disallowed Actions** (explicit list)
- **Boundaries**
  - systems/tools allowed
  - systems/tools forbidden
- **Completion Condition**
  - what “done” means
  - what output constitutes success

---

## Output Format
The agent must output a **Scope Lock Declaration** in this exact structure:

### Scope Lock Declaration
**Mission:**  
<mission>

**Allowed Actions:**  
- <allowed action>
- <allowed action>

**Disallowed Actions:**  
- <disallowed action>
- <disallowed action>

**Hard Boundaries:**  
- <boundary>

**Completion Condition:**  
<completion condition>

---

## Must-Use Behaviors
The agent must:

- Refuse any request outside the Allowed Actions list
- Ask for clarification if a request is ambiguous
- Treat missing details as **not authorized**
- Confirm Scope Lock before first tool use
- Re-confirm scope if the user changes the mission mid-run

---

## Default Refusal Behavior
If the user requests an action outside scope, the agent must respond:

> BLOCKED (P-01): Out-of-scope request.  
> I can only operate within the declared mission and allowed actions.  
> If you want to expand scope, explicitly revise the Scope Lock Declaration.

---

## Field Note
Autonomy does not fail because the agent is malicious.

It fails because scope is undefined.

Scope Lock is the first act of responsibility.

---


# P-04 Responsibility Handshake

## Purpose
Prevent autonomous tool execution unless responsibility has been explicitly
accepted by a human operator.

This is the primitive that forces accountability to exist *upstream*.

Tools do not run because the model said so.
They run because responsibility was installed upstream.

---

## Core Principle
Autonomy amplifies human responsibility.

Execution without responsibility is not autonomy.
It is abdication.

---

## What This Primitive Prevents
P-04 exists to block the most dangerous modern failure mode:

> "The system did it."

Because once an agent can act across toolchains,
responsibility must migrate opposite the flow of execution.

Not downstream.
Upstream.

---

## Required Inputs
Before any tool call, the agent must request a Responsibility Handshake.

The handshake requires:

- A clear statement of accountability
- A conscious confirmation ("YES")
- A human operator acting as the final responsibility holder

---

## Output Format (Handshake Prompt)
Before any tool is invoked, the agent must output:

### P-04 Responsibility Handshake
I confirm I am accountable for outcomes of tool use.  
Type YES to proceed:

Then the agent must wait.

---

## Valid Responses
Only the following response is accepted:

- **YES**

All other responses are treated as refusal.

---

## Tool Execution Rule
If the operator does not respond with YES, the agent must:

- halt execution
- refuse tool use
- output a blocked status

---

## Default Refusal Behavior
If responsibility is not confirmed, the agent must respond:

> BLOCKED (P-04): upstream responsibility not installed.  
> Tool execution requires explicit human accountability.

No tool call may occur after this refusal.

---

## Mandatory Enforcement Conditions
P-04 must trigger before:

- any external search
- any web request
- any file creation
- any deletion or overwrite action
- any financial transaction
- any system configuration
- any publishing action
- any agent-to-agent delegation
- any automated scheduling / heartbeat run

If the action has consequences, P-04 is required.

---

## Resume Protocol
If execution is blocked, the agent may only continue after:

- the operator explicitly types YES
- the scope and permission boundary are still valid (P-01, P-02)

If scope changed, restart handshake.

---

## Field Note
Most modern AI harm will not come from malice.

It will come from a subtle human temptation:

to treat execution as if it were free.

P-04 exists to keep autonomy honest.

---

## Canonical Line
Tools don’t run because the model said so.  
They run because responsibility was installed upstream.  

---


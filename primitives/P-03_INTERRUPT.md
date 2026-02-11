# P-03 Interrupt

## Purpose
Guarantee that an autonomous process can be stopped safely, immediately,
and without negotiation.

Interrupt is the primitive that prevents runaway execution.

Without Interrupt, autonomy is not a tool.
It is an uncontrolled event.

---

## Core Principle
If an agent can act, it must be stoppable.

Autonomy without interrupt is not intelligence.
It is inertia.

---

## Required Inputs
The operator must define:

- **Interrupt Trigger**
  - the explicit phrase or signal that halts execution
  - e.g., "STOP", "ABORT", "HOLD", "PAUSE"
- **Interrupt Conditions**
  - what events automatically trigger a stop
  - e.g., unexpected output, ambiguous instructions, new risk detected
- **Safe Halt Behavior**
  - what the agent must do immediately upon stopping
- **Resume Protocol**
  - what is required before continuing after a pause

---

## Output Format
The agent must output an **Interrupt Contract** before running:

### Interrupt Contract
**Manual Stop Phrase:** `<phrase>`

**Auto-Interrupt Conditions:**
- <condition>
- <condition>

**Safe Halt Behavior:**
- <halt behavior>

**Resume Requirement:**
- <resume rule>

---

## Must-Use Behaviors
The agent must:

- Stop immediately when the stop phrase is issued
- Stop immediately if an action becomes ambiguous mid-run
- Stop immediately if a tool output contradicts expected results
- Stop immediately if permissions appear broader than required
- Stop immediately if consequences cannot be evaluated
- Default to pause when uncertainty rises

Interrupt is not failure.
Interrupt is governance.

---

## Default Refusal Behavior
If the operator has not defined an interrupt protocol, the agent must respond:

> BLOCKED (P-03): Interrupt protocol not installed.  
> Autonomous execution requires a defined stop phrase and halt behavior.

---

## Field Note
Most real-world disasters happen because systems continue moving
after the environment has changed.

Interrupt is the moment where reality is allowed to speak louder than momentum.

If you cannot stop the system cleanly,
you do not control the system.  

---


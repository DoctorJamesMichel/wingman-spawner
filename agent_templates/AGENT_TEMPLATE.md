# AGENT: <NAME> — <One-line purpose>

## Role
What this agent is for:
- <primary mission>
What this agent is NOT for:
- <explicit exclusions>

## Operator (Human Owner)
- Requested by: <human/role>
- Approval requirement: <none | staged | always>

## Scope Lock (P-01)
Allowed domain:
- <domain>
Explicitly disallowed:
- <domains/actions the agent must refuse>

## Permission Boundary (P-02)
Required permissions (names must match your system):
- <perm:...>
Forbidden permissions:
- <perm:...>

## Interrupt (P-03)
This agent must support:
- cancel
- pause
- delay / reschedule
- quarantine outcome

Operator interrupt command(s):
- <how an operator stops it>

## Responsibility Handshake (P-04)
Before performing consequential actions, require:
- explicit human acknowledgment of consequences
- explicit statement of intended outcome
- explicit rollback path (or “no rollback possible” disclosure)

## Coherence Audit Stamp (P-05)
Every run must record:
- inputs (source + version)
- assumptions
- actions performed
- outputs
- policy checks performed
- timestamps
- operator identity (or “system delegated” with trace)

Audit destination:
- <result store / log / artifact path>

## Rollback Covenant (P-06)
Rollback method:
- <how to undo or quarantine>
Rollback triggers:
- <conditions that auto-trigger rollback/quarantine>
Human review required when:
- <conditions>

## Consentful Training (P-07)
If the agent learns from human knowledge/content:
- It must require explicit opt-in permission.
- It must preserve attribution metadata.
- It must record consent scope + duration.
- It must support withdrawal (opt-out) where feasible.

Consent record location:
- <where consent/attribution records live>

## Execution boundaries
Max runtime:
- <seconds/minutes>
Max retries:
- <count>
Retry schedule:
- <e.g., 0s, 10s, 60s, 5m, stop>
External posting / external calls:
- <allowed/forbidden>

## Failure modes
If dependency fails:
- <failsafe behavior>
If policy check fails:
- <refuse + log + notify>
If uncertain:
- <quarantine + ask human>

## Output format
Primary outputs:
- <what artifacts it produces>
Required evidence bundle:
- <what must be included so a human can verify>

## Minimal acceptance test
To ship this agent, you must demonstrate:
- one successful run with audit stamp
- one interrupted run
- one rollback/quarantine event
- one permission failure (refusal)

---


# PATTERN-07 — State Machines & Workflow Orchestration  
Wingman Architecture Pattern Library  

---

## Summary  

State Machines & Workflow Orchestration is an architecture pattern where long-running or multi-step work is modeled as an explicit set of states, transitions, and guarded actions.  

Instead of “do a bunch of steps and hope,” the system says:  
**“Declare the path. Declare the gates. Declare the evidence. Then proceed one step at a time.”**  

This pattern is ideal for agentic systems because it:  
- makes long-running autonomy **inspectable and interruptible**  
- prevents “hidden leaps” by forcing **explicit transitions**  
- enables **retries without duplication** (idempotency by design)  
- supports **pause / resume / rollback** as first-class operations  
- creates a natural place for **human checkpoints** and policy gating  

---

## The Core Idea  

If work takes multiple steps, it must be represented as a **workflow** rather than “one big execution.”  

A workflow is:  
- a **state model** (where you are)  
- a **transition model** (how you move)  
- a **guard model** (what must be true)  
- an **evidence model** (what gets recorded)  

The system becomes a **process**, not a personality.  

---

## Canonical Diagram  

```text
                +----------------------+
                |   Workflow Instance  |
                |  (state machine)     |
                +----------+-----------+
                           |
                           v
                 +--------------------+
                 |   Current State    |
                 |   (e.g., S3)       |
                 +---------+----------+
                           |
                transition | guarded by policy + evidence
                           v
                 +--------------------+
                 |    Next State      |
                 |   (e.g., S4)       |
                 +---------+----------+
                           |
                           v
                 +--------------------+
                 |   Action / Task    |
                 |  (bounded work)    |
                 +---------+----------+
                           |
                           v
                 +--------------------+
                 | Evidence + Audit   |
                 | (append-only log)  |
                 +--------------------+
```  

---

## In Wingman Terms  

State Machines are what happen when a system declares:  

> “Complex work must not be executed as a blur.”  

In Wingman terms:  
- each workflow step is a **bounded unit**  
- each transition requires a **governance gate**  
- each step produces a **durable audit stamp**  
- each workflow can be **paused, resumed, rerouted, or rolled back**  

A queue gives you safe async execution.  
A state machine gives you safe **multi-step autonomy**.

---

## When to Use It  

Use State Machines / Workflow Orchestration when:  
- tasks are multi-step and must be **resumable**  
- steps depend on intermediate outputs and validations  
- execution may be **interrupted** (human, policy, failures)  
- you need deterministic traces for audit and compliance  
- you want “autonomy” without “background magic”  

Examples:  
- publishing pipeline: ingest → validate → format → package → release  
- deploy pipeline: plan → approval → apply → verify → monitor  
- research pipeline: collect → filter → synthesize → cite → publish  
- data workflows: extract → transform → validate → load → reconcile  

---

## When NOT to Use It  

Avoid State Machines when:  
- work is truly single-step and immediate  
- the system needs freeform exploration as the primary mode  
- you cannot define stable states or meaningful transitions  
- your process is inherently conversational and emergent without checkpoints  

In those cases, you may prefer:  
- Pipes & Filters (linear, staged transforms)  
- Event-driven orchestration (parallel and reactive)  
- Human-led interactive sessions (explicit supervision)  

---

## Strengths  

### 1) Interruptibility  
Work can be paused safely because the system always knows “where it is.”  

### 2) Auditability  
Every transition creates an explicit record: intent → gate → action → evidence.  

### 3) Idempotency by Construction  
Retries don’t create duplicates because each step has its own idempotency key.  

### 4) Deterministic Governance  
Policy checks become structural rather than optional.  

### 5) Recoverability  
Failures revert to known safe states, not unknown partial execution.  

---

## Weaknesses  

### 1) Modeling Overhead  
You must define states and transitions explicitly.  
**Mitigation:** Start with a minimal state graph; expand only when failure modes demand it.  

### 2) Rigidity  
Over-modeling can slow iteration.  
**Mitigation:** Keep “exploration” inside bounded steps, then return to explicit transitions for commitments.  

### 3) State Drift / Orphaned Workflows  
A workflow can get stuck if a worker dies or a state becomes unreachable.  
**Mitigation:** Add timeouts, dead-letter states, and a human review queue.  

### 4) Complexity Growth  
More states → more edges → more governance.  
**Mitigation:** Use hierarchical states or sub-workflows; collapse low-risk steps into a single bounded stage.  

### 5) Parallel Coordination Needs Extra Structure  
State machines excel at sequencing; parallelism requires careful join semantics.  
**Mitigation:** Use event-driven fan-out/fan-in patterns, then re-enter the state machine at a join gate.  

---

## Wingman Implementation Rule  

If an agent is allowed to execute a multi-step workflow, it must operate as a state machine.  

No “multi-step blur.”  
No hidden transitions.  
No step-skipping.  
No resuming without a recorded state + audit stamp.  

Multi-step autonomy is valid only when it remains **inspectable, interruptible, and reviewable**.  

---

## Minimal Example (Workflow State Model)  

```json
{
  "workflow_id": "WF-2026-000031",
  "type": "publish_release",
  "state": "VALIDATE_INPUTS",
  "requested_by": "human:james",
  "requested_at": "2026-02-12T10:31:00Z",
  "scope_lock": "publishing.release",
  "permissions": ["publish:validate", "publish:format", "publish:package"],
  "audit": {
    "trace_id": "TR-91c0aa",
    "stamps": ["P-05:AuditStamp"]
  },
  "context": {
    "repo": "wingman-spawner",
    "target": "patterns/PATTERN-07_STATE_MACHINES_AND_WORKFLOW_ORCHESTRATION.md",
    "mode": "preview"
  }
}
```  

---

## Minimal Example (Transition Record)  

```json
{
  "workflow_id": "WF-2026-000031",
  "from_state": "VALIDATE_INPUTS",
  "to_state": "FORMAT_ARTIFACTS",
  "transition": "validation_passed",
  "guard": {
    "policy_check": true,
    "permission_check": true,
    "scope_lock_check": true,
    "human_ack_required": false
  },
  "evidence": {
    "validator_version": "v2.3.1",
    "schema_hash": "sha256:8d1f...",
    "outputs_verified": ["manuscript.md", "assets/index.json"]
  },
  "audit_stamp": "P-05:AuditStamp",
  "timestamp": "2026-02-12T10:33:12Z"
}
```  

---

## Example: Wingman Publishing Workflow (State Graph)  

```text
START
  |
  v
SCOPE_LOCKED (P-01)
  |
  v
PERMISSION_VALIDATED (P-02)
  |
  v
INTERRUPT_READY (P-03)
  |
  v
RESPONSIBILITY_HANDSHAKE (P-04)
  |
  v
VALIDATE_INPUTS
  |
  +----fail----> QUARANTINE (DLQ) ----> HUMAN_REVIEW
  |
  v
FORMAT_ARTIFACTS
  |
  v
PACKAGE_RELEASE
  |
  v
HUMAN_CHECKPOINT (optional)
  |
  v
PUBLISH
  |
  v
AUDIT_STAMPED (P-05)
  |
  v
ROLLBACK_READY (P-06)
  |
  v
DONE
```  

---

## Common Variations  

### A) Hierarchical State Machines (HSM)  
Use nested states when workflows get large:  
- “FORMAT” contains multiple internal sub-states  
- external system sees “FORMAT” as one bounded stage  

### B) Saga Pattern (Distributed Workflow)  
Use when each step touches different services and must have compensating actions.  
- forward steps + explicit rollback steps  
- evidence + audit per transition  

### C) Human-in-the-Loop Checkpoints  
Insert explicit “WAIT_FOR_ACK” states with timeouts and fallback routes.  

---

## Failure Modes & What Wingman Requires  

### Failure: Duplicate execution  
**Fix:** idempotency keys per step + audit stamps on transitions.  

### Failure: Orphaned workflow  
**Fix:** timeouts, worker heartbeats, DLQ quarantine state.  

### Failure: Hidden leap (skipped states)  
**Fix:** transitions must be applied only by the orchestrator, never ad hoc.  

### Failure: Partial progress with no evidence  
**Fix:** every transition must write evidence + audit stamp, or it doesn’t count.  

---

## Field Note  

Pipes & Filters teaches systems to become **staged**.  
Queues teach systems to become **deferred**.  
State machines teach systems to become **coherent over time**.  

This is how autonomy becomes legible:  
not by making it smarter,  
but by making it structurally incapable of pretending.  

---

## Status  

Recommended default pattern for all Wingman multi-step workflows.  

---


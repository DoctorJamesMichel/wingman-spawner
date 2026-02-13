# PATTERN-08 — Circuit Breakers and Failsafe Modes
Wingman Architecture Pattern Library

---

## Summary

**Circuit Breakers and Failsafe Modes** is the pattern that prevents an agent system from turning repeated failure into repeated harm.

Instead of "try harder," the system says:

> "Stop. Contain. Notify. Recover under review."

This pattern is essential for governed autonomy because it:

- prevents **runaway retries** and **failure amplification**
- forces failures into **inspectable**, **interruptible** states
- preserves user trust by making “safe stop” the default
- creates a natural place for escalation: **human ack**, **rollback**, **quarantine**

---

## Core Idea

When error rates rise, uncertainty increases, or external dependencies degrade, the system must **reduce autonomy** automatically.

Circuit breakers act like a governor on a machine:

- **Closed**: normal operation
- **Open**: stop calling the risky dependency (or stop the class of actions)
- **Half-open**: controlled test calls to see if stability returns

Failsafe modes define what the system does when it cannot safely proceed.

---

## In Wingman Terms

Circuit breakers is what happens when a system declares:

> “When I don’t know, I stop.”

Failsafe modes is what happens when a system declares:

> “When I can’t guarantee safety, I degrade gracefully.”

In Wingman terms:

- The agent may be capable.
- The environment may not be stable.
- **Autonomy must be conditional on system health.**

---

## When to Use

Use Circuit Breakers + Failsafe Modes when:

- you rely on external APIs (LLMs, payment providers, email, social platforms)
- retries could cause harm (duplicate orders, repeated messages, accidental spam)
- your system runs async jobs (queues, workers, orchestration)
- you need a clear boundary between:
  - “recover automatically”
  - “require human review”
  - “quarantine and rollback”

---

## Wingman Primitives This Pattern Reinforces

- **P-01 Scope Lock** — breakers attach to a scoped capability (not “everything”)
- **P-02 Permission Boundary** — dangerous actions require stricter gating in failsafe
- **P-03 Interrupt** — breaker open is an interrupt state
- **P-04 Evidence** — every trip must produce evidence (metrics, logs, traces)
- **P-05 Audit Stamp** — breaker trip is a first-class audited event
- **P-06 Rollback Covenant** — failsafe may trigger rollback/quarantine

---

## Canonical Diagram

```text
                       ┌──────────────────────────────┐
                       │  Request / Job Execution     │
                       └──────────────┬───────────────┘
                                      │
                                      v
                          ┌──────────────────────┐
                          │  Governance Gate     │
                          │  (scope+permissions) │
                          └──────────┬───────────┘
                                     │
                                     v
                          ┌──────────────────────┐
                          │ Circuit Breaker Check│
                          │  (per dependency /   │
                          │   per action class)  │
                          └───────┬───────┬──────┘
                                  │       │
                        breaker CLOSED   breaker OPEN
                                  │       │
                                  v       v
                      ┌────────────────┐  ┌──────────────────────┐
                      │ Execute Action │  │ Failsafe Mode Handler │
                      │ (bounded work) │  │ (contain/degrade/     │
                      └───────┬────────┘  │  notify/quarantine)   │
                              │           └──────────┬───────────┘
                              v                      │
                 ┌──────────────────────────┐        v
                 │ Observe Outcome + Metrics │  ┌───────────────────┐
                 │ (error rates, latency,    │  │ Human Review /     │
                 │  policy violations)       │  │ Ack / Rollback     │
                 └──────────┬───────────────┘  └───────────────────┘
                            │
                            v
                 ┌──────────────────────────┐
                 │ Update Breaker State      │
                 │ + Audit Stamp             │
                 └──────────────────────────┘
```

---

## Breaker Placement Options

### 1) Dependency-level breaker (most common)
Trip when an external dependency is unstable.

Examples:
- LLM provider returning errors or low-confidence output
- payment provider timing out
- email provider bouncing or rate-limiting

### 2) Action-class breaker (agent safety)
Trip when a class of operations becomes unsafe.

Examples:
- “external_posting”
- “purchase”
- “bulk_email”
- “filesystem_write”
- “deploy_to_prod”

### 3) Tenant- / user-scoped breaker (blast-radius control)
Trip only for a given tenant or user, not the whole system.

---

## Failsafe Modes (Degrade Gracefully)

Failsafe modes should be explicit and named. Examples:

1. **READ_ONLY**
   - allow queries and summaries
   - deny mutations (writes, posts, purchases)

2. **HUMAN_ACK_REQUIRED**
   - allow preparing a plan
   - require human confirmation to execute

3. **QUARANTINE_OUTPUT**
   - allow execution but do not publish results
   - store results + evidence for review

4. **SAFE_STOP**
   - stop processing the job class entirely
   - surface status and next steps

5. **FALLBACK_PROVIDER**
   - switch to backup dependency (if and only if policy permits)

---

## Wingman Implementation Rule

A system is not allowed to "retry into danger."

Therefore:

- Circuit breakers must exist for any operation that:
  - can cause external effects, or
  - can amplify failure through retries, or
  - depends on unstable external services

No "background magic."
No silent retries.
No infinite loops.

---

## Minimal Example (Breaker Policy)

```json
{
  "breaker_id": "breaker.llm.primary",
  "scope_lock": "capability:generate_text",
  "monitored_signals": {
    "error_rate": { "window_seconds": 60, "trip_threshold": 0.20 },
    "timeout_rate": { "window_seconds": 60, "trip_threshold": 0.10 },
    "p95_latency_ms": { "window_seconds": 60, "trip_threshold": 8000 }
  },
  "states": {
    "closed": { "allow": true },
    "open": {
      "allow": false,
      "cooldown_seconds": 120,
      "failsafe_mode": "HUMAN_ACK_REQUIRED"
    },
    "half_open": {
      "allow": true,
      "test_requests": 3,
      "failsafe_mode_on_failure": "SAFE_STOP"
    }
  },
  "audit": {
    "emit_event_on_trip": true,
    "emit_event_on_recovery": true
  }
}
```

---

## Minimal Example (Failsafe Decision)

```text
If breaker OPEN:
  - deny execution
  - return structured response:
      status: "blocked_by_breaker"
      failsafe_mode: "HUMAN_ACK_REQUIRED"
      reason: "dependency_unstable"
      next_steps: ["wait_for_recovery", "manual_override_with_ack"]

If breaker HALF_OPEN:
  - allow only test requests
  - quarantine outputs
  - require audit evidence

If breaker CLOSED:
  - proceed normally
```

---

## Example: Wingman Failure Containment (Async Worker)

```text
1) Worker pulls job
2) Worker validates permissions + scope
3) Worker checks breaker for action class: "external_posting"
4) If OPEN:
     - mark job as "blocked"
     - emit audit stamp
     - notify UI with failsafe instructions
     - do NOT retry automatically
5) If CLOSED:
     - execute bounded action
     - record metrics + evidence
     - update breaker state if failure signals rise
```

---

## Strengths

- prevents cascading failures
- reduces blast radius
- forces safety into explicit states
- creates reliable “pause” and “review” mechanisms
- improves operator visibility and accountability

---

## Weaknesses

1. **False positives**
   - Breakers can trip too aggressively.
   - Mitigation: tune thresholds; use half-open testing.

2. **Reduced availability**
   - Safety reduces throughput during instability.
   - Mitigation: explicit failsafes (read-only / human-ack) preserve usefulness.

3. **Operational complexity**
   - Requires metrics, monitoring, and disciplined policy definitions.
   - Mitigation: provide default breaker templates per capability.

---

## Field Note

Circuit breakers are not merely a resilience pattern.

They are a **governance pattern**.

They encode the principle that autonomy is only valid when it remains:

- interruptible
- inspectable
- bounded
- accountable under degraded conditions

That is the minimum viable requirement for safe autonomy.

---

## Status

Recommended default pattern for all Wingman agent workflows that:
- execute async jobs
- call external dependencies
- perform actions with external effects

---


# PATTERN-10 — Retry Backoff and Dead Letter Queues

Wingman Architecture Pattern Library

---

## Summary

Retry Backoff and Dead Letter Queues is a resilience pattern that ensures failed tasks do not loop endlessly, overload the system, or silently disappear.

Instead of treating failure as an exception, this pattern treats failure as a **first-class workflow state**:

- failures are retried intentionally
- retries are delayed (backoff)
- repeated failures are quarantined into a DLQ (Dead Letter Queue)
- human review becomes mandatory at the correct boundary

This pattern prevents the most dangerous agentic failure mode:

> infinite persistence without accountability

---

## The Problem It Solves

Without a retry governance structure, an agent system tends to do one of two bad things:

### Failure Mode A — Silent Drop

A task fails and disappears.
No one notices.
The system claims it is “done.”

### Failure Mode B — Infinite Retry Spiral

A task fails and retries immediately.
Then fails again.
Then retries again.

This can create:

- runaway compute consumption
- cascading dependency failure
- accidental denial-of-service
- unbounded “agent persistence”
- repeated policy violations

Retry without governance becomes **autonomy without brakes**.

---

## Pattern Intent

This pattern enforces a controlled response to failure:

1. Retry only when permitted
2. Retry only a bounded number of times
3. Increase delay between attempts
4. Escalate to quarantine after repeated failure
5. Preserve full audit evidence for every attempt
6. Require human review before re-entry

---

## When To Use

Use this pattern when:

- tasks execute asynchronously
- tasks call external APIs
- tasks interact with unstable dependencies
- tasks can fail transiently
- tasks must not disappear silently
- the system must prevent “retry storms”

This pattern is especially required for:

- agent tool invocation
- autonomous workflows
- background automation
- any system with queues + workers

---

## When NOT To Use

Do not use retries when failure is likely permanent, such as:

- invalid permissions
- forbidden action classes
- malformed input schema
- scope lock mismatch
- policy violations

Those failures should go directly to DLQ or human review.

---

## Wingman Implementation Rule

**Retry is not a behavior. Retry is a governed state transition.**

The worker must not decide:
> “I will just try again.”

Instead, the system must decide:
> “Retry is permitted under these constraints.”

---

## Canonical Diagram

```text
                        +---------------------------+
                        |     Job Enqueued          |
                        |  (intent + constraints)   |
                        +-------------+-------------+
                                      |
                                      v
                        +---------------------------+
                        |   Worker Executes Job     |
                        |   (bounded invocation)    |
                        +-------------+-------------+
                                      |
                                      v
                 +--------------------+--------------------+
                 |                                         |
                 v                                         v
     +---------------------------+           +---------------------------+
     |        Success            |           |        Failure            |
     |  result + audit evidence  |           | error + metrics captured  |
     +-------------+-------------+           +-------------+-------------+
                   |                                       |
                   v                                       v
     +---------------------------+           +---------------------------+
     |  Emit Completion Event    |           | Retry Policy Evaluation   |
     |  (notify / update UI)     |           | (retryable? attempts?)    |
     +---------------------------+           +-------------+-------------+
                                                           |
                                           +---------------+---------------+
                                           |                               |
                                           v                               v
                             +---------------------------+   +---------------------------+
                             |   Re-enqueue w/ Backoff   |   |    Dead Letter Queue      |
                             |  attempt += 1, delay grows|   | quarantine + human review |
                             +-------------+-------------+   +-------------+-------------+
                                           |                               |
                                           v                               v
                             +---------------------------+   +---------------------------+
                             | Worker retries later      |   | Human decision required   |
                             | (same constraints apply)  |   | requeue / rollback / drop |
                             +---------------------------+   +---------------------------+
```

---

## Core Components

### 1. Retry Policy Engine

Defines:

- which failures are retryable
- how many retries are allowed
- what backoff schedule applies
- when to escalate to DLQ

Retry must be **policy-driven**, not improvisational.

---

### 2. Exponential Backoff Scheduler

Retries should not occur immediately.

Instead, delays should grow:

- 5 seconds
- 30 seconds
- 2 minutes
- 10 minutes
- 1 hour

This prevents a retry storm when a dependency is down.

---

### 3. Attempt Counter + Idempotency

Every retry attempt must:

- preserve the same job_id
- increment attempt_count
- maintain idempotency_key

Without idempotency, retries become duplicates.
Duplicates become uncontrolled side effects.

---

### 4. Dead Letter Queue (DLQ)

After max attempts are exceeded, the job is quarantined.

A DLQ is not a trash bin.

A DLQ is a **quarantine chamber**.

DLQ jobs must retain:

- original request envelope
- full execution trace
- failure reasons per attempt
- timestamps
- worker identity
- policy checks performed

---

### 5. Human Review Gateway

The DLQ is a governance boundary.

A human must decide:

- requeue (with modifications)
- rollback
- permanently discard
- escalate for investigation

---

## In Wingman Terms

Retry Backoff + DLQ is what happens when a system declares:

> "Persistence is allowed only when it remains accountable."

A Wingman agent is allowed to try again only if:

- the system approves it
- the retry is bounded
- the retry is delayed
- the retry is audited
- the retry is interruptible

---

## Minimal Example (Retry Envelope)

```json
{
  "job_id": "JOB-2026-000211",
  "trace_id": "TR-9a71ce",
  "requested_by": "human:james",
  "requested_at": "2026-02-13T06:22:00Z",
  "scope_lock": "publishing.formatter",
  "permissions": ["publish:format", "publish:package"],
  "action": "format_and_package_release",
  "inputs": {
    "repo": "wingman-spawner",
    "target": "patterns/PATTERN-10_RETRY_BACKOFF_AND_DEAD_LETTER_QUEUES.md",
    "mode": "preview"
  },
  "retry_policy": {
    "max_attempts": 5,
    "backoff_strategy": "exponential",
    "initial_delay_seconds": 5,
    "max_delay_seconds": 3600,
    "jitter": true
  },
  "attempt_count": 1,
  "idempotency_key": "format_release:wingman-spawner:TR-9a71ce",
  "constraints": {
    "requires_human_ack": true,
    "max_runtime_seconds": 180,
    "no_external_posting": true
  }
}
```

---

## Retryable vs Non-Retryable Failures

A resilient system distinguishes:

### Retryable Failures

- timeout
- temporary 503 / 502
- dependency unavailable
- network interruption
- queue congestion
- rate limit exceeded (429)

These are environmental failures.

---

### Non-Retryable Failures

- permission denied
- policy violation
- invalid input schema
- malformed payload
- forbidden tool class
- scope mismatch

These are structural failures.

Retrying structural failure is not resilience.
It is denial.

---

## Recommended Backoff Schedule

A canonical Wingman backoff schedule:

| Attempt | Delay |
|--------:|------:|
| 1       | 5s    |
| 2       | 30s   |
| 3       | 2m    |
| 4       | 10m   |
| 5       | 1h    |

Add jitter to prevent synchronized retry storms.

---

## DLQ Escalation Rule

A job must enter DLQ when:

- max_attempts is exceeded
- retry policy forbids retry
- failure indicates policy violation
- dependency failure persists beyond max_delay
- execution becomes non-idempotent

---

## DLQ Quarantine Contract

A DLQ job must be:

- preserved
- searchable
- auditable
- replayable only by explicit authorization

The DLQ is where accountability lives.

---

## Audit Requirements

Every attempt must log:

- timestamp
- worker_id
- dependency calls
- policy checks passed/failed
- exit state
- exception trace
- emitted events

The audit log must be append-only.

---

## Common Pitfalls

### Pitfall 1 — Infinite Retry Loops

If retries are unlimited, the agent becomes immortal.

That is not resilience.
That is uncontrolled persistence.

---

### Pitfall 2 — No Backoff

Retrying immediately is effectively an attack on your own system.

---

### Pitfall 3 — DLQ Without Review

A DLQ without review is just a graveyard.
It becomes a silent failure store.

---

### Pitfall 4 — Retrying Non-Retryable Errors

If permission errors retry, you have created an escalation engine.

---

## Interaction With Other Wingman Patterns

This pattern is tightly coupled to:

- **P-06 Async Task Execution with Queues**
- **P-08 Circuit Breakers and Failsafe Modes**
- **P-09 Rate Limiting and Quotas**

Together these form the Wingman resilience triad:

- Retry controls persistence
- Circuit breakers control systemic collapse
- Rate limiting controls consumption

---

## Design Principle

The system must always preserve this truth:

> A retry is not a second chance.  
> It is a formally governed continuation of the same intent.

---

## Closing Thought

Retry Backoff + DLQ is the pattern that prevents the AI equivalent of obsession.

It makes failure survivable without making persistence dangerous.

A system that cannot retry is fragile.

A system that retries without a DLQ is reckless.

A system that retries with governance becomes durable.

---

**Wingman Standard:**
If a task can run asynchronously, it must also be able to fail safely.  

---


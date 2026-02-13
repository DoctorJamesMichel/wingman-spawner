# PATTERN-09 — Rate Limiting & Quotas (Governed Throughput)

Wingman Architecture Pattern Library

---

## Summary

Rate Limiting & Quotas is the pattern that prevents an agentic system from turning **valid intent** into **runaway throughput**.

It enforces two distinct protections:

- **Rate limits**: “How fast can actions be attempted right now?”
- **Quotas**: “How much can be spent/consumed over a longer window?”

In agent systems, this is not “performance tuning.”  
It is **governance**: protecting humans, dependencies, budgets, and the outside world from scale-triggered harm.

---

## When to Use

Use this pattern when any of the following are true:

- actions have **external side effects** (emails, posts, purchases, deploys, deletes)
- actions consume **paid resources** (API tokens, cloud costs, GPU time, credits)
- actions touch **rate-limited dependencies** (LLMs, payment providers, SaaS APIs)
- tasks can be triggered by **untrusted input** (users, webhooks, incoming events)
- async/parallel workers exist (queues, fan-out, event-driven orchestration)

---

## Core Idea

A request can be valid, correctly scoped, and permissioned — and still be unsafe at scale.

So: **validate intent, then validate throughput.**

---

## Wingman Implementation Rule

**No agent action may execute without passing an explicit throughput gate.**

- No “silent burst.”
- No “retry storms.”
- No “infinite fan-out.”
- No execution that can’t be bounded in time and cost.

---

## Canonical Diagram

```text
User / UI / Event
   |
   v
Governance Gate (scope + permissions)
   |
   v
Throughput Gate (rate + quota)
   |                       \
   | allowed                \ denied
   v                         v
Queue / Execute (bounded)   Defer / Reject / Human Ack
   |
   v
Audit Stamp + Metrics
```

---

## Minimal Example (Policy Shape)

```json
{
  "throughput_policy": {
    "rate_limits": [
      {
        "key": "action_class:external_posting",
        "limit": 5,
        "per_seconds": 60,
        "burst": 2
      },
      {
        "key": "dependency:llm_provider_primary",
        "limit": 30,
        "per_seconds": 60,
        "burst": 10
      }
    ],
    "quotas": [
      {
        "key": "human:James",
        "limit": 100,
        "per_hours": 24,
        "unit": "jobs"
      },
      {
        "key": "workspace:prod",
        "limit": 50,
        "per_hours": 24,
        "unit": "external_side_effects"
      },
      {
        "key": "budget:llm_tokens",
        "limit": 200000,
        "per_hours": 24,
        "unit": "tokens"
      }
    ],
    "deny_behavior": {
      "default": "defer",
      "defer_seconds": 120,
      "requires_human_ack_on_repeat": true
    }
  }
}
```

---

## In Wingman Terms

Rate limiting is what happens when a system declares:

> “Even when the request is valid, the system is not allowed to move faster than it can remain accountable.”

Quotas are what happen when a system declares:

> “No single actor can consume the shared future.”

---

## Governance Notes (Why This Matters for Agents)

Agents are uniquely dangerous to systems because they:

- can generate **actions faster than humans can notice**
- can interpret ambiguity as “try again”
- can fan out across tools and dependencies
- can create cost without malice (just momentum)

Rate + quota gates create **friction with memory**:
- memory of what happened a second ago (rate)
- memory of what happened this day/week (quota)

That memory is a governance organ.

---

## Operational Modes

### 1) Hard Deny
Best when actions are high-risk.

- deny immediately
- require human acknowledgment to proceed
- log as “blocked_by_policy”

### 2) Soft Defer
Best when actions are safe but bursty.

- enqueue for later
- present “scheduled due to rate policy”
- keep the system responsive without being reckless

### 3) Degrade
Best when partial value is acceptable.

- switch to cheaper model / reduced scope
- lower concurrency
- shorten output length
- reduce tool calls

---

## Common Keys (What to Rate/Quota)

Rate limits should be keyed by:

- `action_class` (external_posting, payments, deploy, delete)
- `dependency` (LLM provider, payment provider, email API)
- `tenant/workspace` (prod vs sandbox)
- `agent_role` (publisher vs formatter vs researcher)
- `resource_type` (tokens, dollars, jobs, emails)

Quotas should be keyed by:

- `human` (who authorized it)
- `workspace` (prod budget protection)
- `project` (prevent a single initiative from consuming all resources)
- `time_window` (daily/weekly/monthly)

---

## Failure Modes (and Mitigations)

### 1) Retry Storms
**Failure:** A downstream error triggers rapid repeated attempts.  
**Mitigation:** Couple rate limits with P-08 Circuit Breaker + explicit retry budgets.

### 2) Fan-Out Cascade
**Failure:** One event triggers N sub-events repeatedly.  
**Mitigation:** Rate limit fan-out per root trace_id; enforce “max_children_per_trace”.

### 3) Unfair Resource Capture
**Failure:** One user/agent consumes all budget.  
**Mitigation:** Quotas per actor + per workspace + per project.

### 4) Hidden Backlogs
**Failure:** Soft defers pile up invisibly.  
**Mitigation:** Queue visibility + “defer ceiling” + human notification threshold.

### 5) False Safety via Global Caps
**Failure:** Only a global cap exists; local hotspots still melt.  
**Mitigation:** Use layered keys (global + per dependency + per action_class).

---

## Recommended Defaults (Practical Starting Point)

- External side effects: **5/min**, burst 2
- Payments / purchases: **1/min**, burst 0 (hard deny above)
- Deploys: **2/hour**, requires human ack in prod
- LLM calls: **30/min** per workspace, **10/min** per agent_role
- Daily quota: **100 jobs/day** per human requester (tune by reality)

---

## Validation Checklist

Before adopting this pattern, confirm:

- [ ] every execution path passes Throughput Gate
- [ ] rate policy is keyed by action_class AND dependency
- [ ] quotas exist for human + workspace + budgeted resources
- [ ] deny behavior is explicit (deny vs defer vs degrade)
- [ ] repeated denials escalate to human acknowledgment
- [ ] audit logs capture: key, limit, window, decision, trace_id
- [ ] UI surface exists for “why this was delayed/blocked”

---

## Status

Recommended default pattern for any Wingman system that can:
- execute asynchronously,
- act on external systems,
- or incur meaningful cost.

---


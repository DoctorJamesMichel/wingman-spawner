# Wingman Patterns — Governance-First Architecture

This folder defines **deployment patterns** for responsibility-first agent systems.

Most software architecture patterns assume competent human operators.
Agentic AI systems require something additional:

**Governance must be the first layer, not a later patch.**

Wingman patterns describe system topologies that make autonomy:

- interruptible
- reviewable
- reversible
- auditable
- responsibility-bound

Wingman primitives are architecture-independent.
They function as **governance DNA** that can be embedded into any system topology.

---

## The Wingman Primitive Set (Governance DNA)

All patterns in this folder assume the primitives are enforced.

### Core Primitives (P-01 through P-06)

- **P-01 Drift Trigger Protocol**  
  Detects loss of scope, runaway execution, or coherence failure and forces containment.

- **P-02 Permission Boundary**  
  Least-privilege access; no hidden escalation; no silent tool expansion.

- **P-03 Interrupt Reflex**  
  A reliable stop/abort path when reality changes mid-run.

- **P-04 Responsibility Handshake**  
  Tools don’t run “because the model said so.”  
  A responsible actor must explicitly authorize material actions.

- **P-05 Coherence Audit Stamp**  
  Every material action leaves a trace: what happened, why, and under whose authority.

- **P-06 Rollback Covenant**  
  If it can act, it must be reversible (or explicitly gated as irreversible).

### Sentinel Primitive (P-07)

- **P-07 Escalation & Containment Trigger**  
  Defines when a system must stop autonomy, escalate to a human, or hard-freeze execution.

Primitives are defined in [`/primitives`](../primitives).

Principle: **Autonomy amplifies human responsibility.**

---

## Pattern Index (v0.2)

This repository includes **11 governance patterns**.

This README highlights the canonical patterns most likely to be used as default architecture spines.
See the full folder contents for the complete set.

All patterns in this folder are designed to be composable.

---

# Architecture Patterns (Wingman Governance Applied)

## 1) Client–Server Pattern (Human-Driven Agent Use)

**Best for:** simple deployments, early testing, manual workflows.  
A human operator is the client; the agent is the server.

**Wingman advantage:** the agent can remain powerful without becoming unsafe, because
Scope + Permission boundaries prevent accidental drift.

**Failure mode without Wingman:** the agent gradually becomes an implicit decision-maker.

**Key primitives emphasized:** P-01, P-02, P-04

---

## 2) Async Task Execution with Queues (Delayed Execution)

**Best for:** batching, scheduled automation, background processing.  
The agent generates tasks, which are placed into a queue for later execution.

**Wingman advantage:** queued autonomy is dangerous unless tasks are stamped,
reviewable, and interruptible.

**Failure mode without Wingman:** deferred actions execute after assumptions expire.

**Key primitives emphasized:** P-03, P-05, P-06, P-07

---

## 3) Serverless / Function-Based Pattern (Event-Triggered Agents)

**Best for:** lightweight agents, webhook triggers, event automation.  
Each invocation is small, scoped, and short-lived.

**Wingman advantage:** serverless execution is naturally compatible with drift control,
because each function can be constrained by design.

**Failure mode without Wingman:** event triggers become an autonomy amplifier.

**Key primitives emphasized:** P-01, P-02, P-03, P-07

---

## 4) Pipes and Filters Pattern (Multi-Stage Governance Flow)

**Best for:** governance-first agent workflows.  
Agent output passes through verification layers before execution.

Example flow:

- Draft Agent → Auditor Agent → Operator Execution

**Wingman advantage:** this is the cleanest structural method for preventing unsafe drift.
It creates enforced friction without killing velocity.

**Failure mode without Wingman:** output becomes execution without review.

**Key primitives emphasized:** P-04, P-05, P-06, P-07

---

## 5) Event-Driven Pattern (Agents Respond to Triggers)

**Best for:** monitoring systems, reactive governance, incident response.  
Agents subscribe to events and respond automatically.

**Wingman advantage:** event-driven systems can rapidly spiral unless interrupts,
audit stamps, and rollback are built into the fabric.

**Failure mode without Wingman:** “helpful automation” becomes runaway response.

**Key primitives emphasized:** P-03, P-05, P-06, P-07

---

## 6) Microservices Pattern (Agent Roles as Services)

**Best for:** scaling multi-agent ecosystems.  
Each agent role becomes its own service: FINANCE, LEGAL, WRITER, etc.

**Wingman advantage:** microservices prevent agent overreach by forcing role separation,
boundary discipline, and explicit responsibility per service.

**Failure mode without Wingman:** service sprawl becomes permission sprawl.

**Key primitives emphasized:** P-01, P-02, P-05, P-07

---

## 7) Monolithic Pattern (All-in-One Agent System)

**Best for:** early prototypes, small deployments, single-user systems.  
One system handles planning, writing, reviewing, and executing.

**Wingman warning:** monoliths drift faster because scope boundaries blur.

**Wingman advantage:** if you must build monolithic, you can still enforce governance
internally by requiring explicit Responsibility Handshakes and Audit Stamps at every
material action.

**Failure mode without Wingman:** the monolith becomes a sovereign decision-maker.

**Key primitives emphasized:** P-01, P-04, P-05, P-06, P-07

---

# Recommended Default Pattern for Wingman Systems

If you want maximum safety without collapsing velocity, the recommended default
architecture spine is:

## Pipes and Filters + Async Queue

This creates a natural safety backbone:

1. A planning or drafting agent produces a proposed action
2. An auditor agent reviews the proposal
3. The action is queued (not executed immediately)
4. The operator must handshake responsibility before execution
5. Every output is audit-stamped and rollback-capable
6. Escalation triggers stop execution when uncertainty rises

This structure supports scalable autonomy without losing accountability.

---

# Field Note

Most AI companies are building agent systems as if they were software features.

Wingman treats them correctly:

**Agent systems are governance infrastructure.**

If you cannot interrupt it,
audit it,
or roll it back,

you do not have autonomy.

You have a liability.  

---


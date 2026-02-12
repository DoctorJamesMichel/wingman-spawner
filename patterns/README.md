# Wingman Patterns — Governance-First Architecture

This folder describes **deployment patterns** for responsibility-first agent systems.

Most software architecture patterns assume competent human operators.
Agentic AI systems require something additional:

**Governance must be the first layer, not a later patch.**

The Wingman primitives (P-01 through P-06) are designed to be architecture-independent.
They function as the **governance DNA** that can be embedded into any system topology.

---

## The Wingman Primitive Set (Governance DNA)

All patterns below assume the six primitives are enforced:

- **P-01 Scope Lock** — define what the agent may do (and may not).
- **P-02 Permission Boundary** — least privilege access; no hidden escalation.
- **P-03 Interrupt** — a reliable stop/abort path when reality changes mid-run.
- **P-04 Responsibility Handshake** — tools don’t run “because the model said so.”
- **P-05 Coherence Audit Stamp** — every material action leaves a trace.
- **P-06 Rollback Covenant** — if it can act, it must be reversible (or explicitly gated).

Principle: **Autonomy amplifies human responsibility.**

---

# Architecture Patterns (with Wingman governance applied)

## 1) Client–Server Pattern (Human-Driven Agent Use)

**Best for:** simple deployments, early testing, manual workflows.  
A human operator is the client; the agent is the server.

**Wingman advantage:** the agent can remain powerful without becoming unsafe, because
Scope Lock + Permission Boundary prevent accidental drift.

**Key primitives emphasized:** P-01, P-02, P-04

---

## 2) Async Task Execution with Queues (Delayed Execution)

**Best for:** batching, scheduled automation, background processing.  
The agent generates tasks, which are placed into a queue for later execution.

**Wingman advantage:** queued autonomy is dangerous unless tasks are stamped,
reviewable, and interruptible.

**Key primitives emphasized:** P-03, P-05, P-06

---

## 3) Serverless / Function-Based Pattern (Event Triggered Agents)

**Best for:** lightweight agents, webhook triggers, event automation.  
Each invocation is small, scoped, and short-lived.

**Wingman advantage:** serverless execution is naturally compatible with Scope Lock,
because each function can be constrained by design.

**Key primitives emphasized:** P-01, P-02, P-03

---

## 4) Pipes and Filters Pattern (Multi-Stage Governance Flow)

**Best for:** governance-first agent workflows.  
Agent output passes through verification layers before execution.

Example flow:
- Writer Agent → Auditor Agent → Operator Execution

**Wingman advantage:** this is the cleanest structural way to prevent unsafe drift.
It creates enforced friction without killing velocity.

**Key primitives emphasized:** P-04, P-05, P-06

---

## 5) Event-Driven Pattern (Agents Respond to Triggers)

**Best for:** monitoring systems, reactive governance, incident response.  
Agents subscribe to events and respond automatically.

**Wingman advantage:** event-driven systems can rapidly spiral unless interrupts
and rollback are built into the fabric.

**Key primitives emphasized:** P-03, P-05, P-06

---

## 6) Microservices Pattern (Agent Roles as Services)

**Best for:** scaling multi-agent ecosystems.  
Each agent role becomes its own service: FINANCE, LEGAL, WRITER, etc.

**Wingman advantage:** microservices prevent “agent overreach” by forcing role
separation and boundary discipline.

**Key primitives emphasized:** P-01, P-02, P-05

---

## 7) Monolithic Pattern (All-in-One Agent System)

**Best for:** early prototypes, small deployments, single-user systems.  
One system handles planning, writing, reviewing, and executing.

**Wingman warning:** monoliths drift faster because scope boundaries blur.

**Wingman advantage:** if you must build monolithic, you can still enforce governance
internally by requiring explicit Responsibility Handshakes and Audit Stamps at every
material action.

**Key primitives emphasized:** P-04, P-05, P-06

---

# Recommended Default Pattern for Wingman Systems

If you want maximum safety without slowing productivity, the default recommended
architecture is:

## Pipes and Filters + Async Queue

This creates a natural safety spine:

1. A planning or drafting agent produces a proposed action
2. An auditor agent reviews the action
3. The action is queued (not executed immediately)
4. The operator must handshake responsibility before execution
5. Every output is audit-stamped and rollback-capable

This structure produces scalable autonomy without losing human accountability.

---

# Field Note

Most AI companies are building agent systems as if they were software features.

Wingman treats them correctly:

**Agent systems are governance infrastructure.**  

---


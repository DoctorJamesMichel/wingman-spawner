# PATTERN-02 — Event-Driven Architecture

Wingman Architecture Pattern Library

---

## Summary

**Event-Driven Architecture** is a software architecture pattern where systems communicate by producing and consuming events.

Instead of a single sequential pipeline, components react asynchronously to signals such as:

- a message
- a state change
- a completed task
- a timer
- a user action
- a system alert

This pattern is ideal for systems that require **parallelism, responsiveness, and modular scalability**.

In Wingman terms, Event-Driven Architecture is the natural complement to **Pipes & Filters**:
it is what you use when a workflow must remain coherent even when events arrive unpredictably.

---

## The Core Idea

Instead of forcing everything through a linear pipeline, you build:

- **Producers** that emit events
- **Consumers** that subscribe to events
- a **message bus** (or queue) that transports events
- optional **routers** that decide where events should go

The system becomes **reactive**, not procedural.

---

## The Canonical Model

Producer → Event Bus → Consumer(s)

Typical flow:

EVENT EMITTED  
↓  
MESSAGE QUEUE / EVENT BUS  
↓  
HANDLER A  
HANDLER B  
HANDLER C  

---

## Key Vocabulary

### Event
A structured message representing something that happened.

Examples:

- `InvoiceCreated`
- `PaymentFailed`
- `UserSubscribed`
- `FileUploaded`
- `DraftApproved`
- `ToolExecutionDenied`

---

### Producer
A component that publishes events.

A producer does not care who listens.
It only cares that the event is correctly emitted.

---

### Consumer (Handler)
A component that reacts to a specific event type.

A consumer does one job:
receive the event, then act according to its contract.

---

### Event Bus / Message Broker
The infrastructure layer that transports events.

Common examples:

- Kafka
- RabbitMQ
- AWS SNS/SQS
- Redis Streams
- Google Pub/Sub

---

## In Wingman Terms

A Wingman event-driven system typically looks like:

User Input  
↓  
Event: `UserRequestReceived`  
↓  
Event Router / Dispatcher  
↓  
Agent(s) respond asynchronously  
↓  
Audit + Rollback trace captured  
↓  
Final response assembled

---

## Why It Works for Agentic Systems

Agentic systems fail when:

- tasks must run in parallel but get forced into sequential execution
- one slow stage blocks the entire workflow
- a single agent becomes a monolith
- state changes must trigger immediate reactions
- coordination between multiple agents becomes implicit and untraceable

Event-driven design solves this by:

- enabling concurrency without fusion
- isolating responsibilities into handlers
- supporting scalable orchestration
- making state transitions explicit
- allowing independent failure recovery

---

## When to Use Event-Driven Architecture

Use this pattern when:

- the system must respond to many unpredictable triggers
- multiple tasks can happen in parallel
- long-running tasks must not block user responsiveness
- you want components to remain loosely coupled
- you want modular scalability (new handlers can be added without rewiring everything)
- workflows involve multiple agents reacting to different parts of the same situation

Common examples:

- multi-agent orchestration
- asynchronous task execution
- monitoring + alert response
- content publishing automation
- governance enforcement pipelines
- financial bookkeeping systems
- compliance workflows

---

## When NOT to Use It

Avoid Event-Driven Architecture when:

- the workflow must remain strictly sequential
- the system requires deterministic step-by-step validation
- the system must be easily explainable to a human at a glance
- your team cannot tolerate debugging asynchronous complexity
- the process depends on strict stage-by-stage approvals

In those cases, you may prefer:

- Pipes & Filters
- Stateful Workflow Orchestration (e.g., Temporal)
- A single pipeline agent with explicit gates

---

## Strengths

### 1. Parallelism
Many consumers can react to the same event simultaneously.

### 2. Loose Coupling
Producers do not depend on consumers.

### 3. Scalability
You can scale handlers independently.

### 4. Resilience
If one handler fails, others can still succeed.

### 5. Extensibility
New consumers can be added without redesigning the system.

---

## Weaknesses

### 1. Debugging Complexity
Events are asynchronous, which can make failures harder to trace.

Mitigation: enforce strict Audit Stamps (P-05) and trace correlation IDs.

---

### 2. Event Storming / Noise Growth
Systems can become flooded with events that are not meaningful.

Mitigation: enforce event schema discipline and avoid emitting low-signal events.

---

### 3. Ordering and Consistency Problems
Some workflows require strict ordering, but event delivery may be delayed or out-of-order.

Mitigation: enforce ordering rules at the consumer level, or use orchestration.

---

### 4. Hidden Autonomy Risk
A handler can quietly become an autonomous decision-maker if not governed.

Mitigation: enforce Scope Lock (P-01) and Responsibility Handshake (P-04).

---

### 5. Distributed Rollback Difficulty
If multiple handlers act on the same event, rollback becomes non-trivial.

Mitigation: require compensating actions and enforce Rollback Covenant (P-06).

---

## Key Governance Alignments

Event-Driven Architecture naturally supports Wingman primitives:

- **P-01 Scope Lock**  
  (each handler has a bounded job)

- **P-02 Permission Boundary**  
  (handlers should operate under least privilege)

- **P-03 Interrupt**  
  (event streams can be paused or halted)

- **P-05 Audit Stamp**  
  (every event becomes a traceable artifact)

- **P-06 Rollback Covenant**  
  (handlers must support compensating actions)

However, it also introduces a risk:

Event-driven systems can accidentally produce **distributed autonomy**.

That is why Wingman requires:

- strict IO contracts
- explicit handoff rules
- no invisible “agent swarm improvisation” without audit

---

## Minimal Example (Agent Event Loop)

Event: `UserRequestReceived`

Handler A — Scope Filter  
Input: request  
Output: `RequestValidated` or `RequestRejected`

Handler B — Permission Boundary Filter  
Input: validated request  
Output: `PermissionsVerified` or `PermissionDenied`

Handler C — Execution Proposal Generator  
Input: verified request  
Output: `ExecutionPlanProposed`

Handler D — Responsibility Handshake  
Input: proposed plan  
Output: `ExecutionApproved` or `ExecutionBlocked`

Handler E — Tool Executor  
Input: approved execution  
Output: `ExecutionComplete`

Handler F — Audit Stamp Recorder  
Input: execution record  
Output: immutable log entry

Handler G — Rollback Covenant Manager  
Input: failure event  
Output: compensating action plan

---

## Example: Publishing Workflow (Event-Driven)

Event: `TopicSelected`  
↓  
Draft Writer emits `DraftCreated`  
↓  
Fact Checker emits `DraftVerified`  
↓  
Tone Editor emits `DraftPolished`  
↓  
Citation Inserter emits `CitationsAttached`  
↓  
Final Formatter emits `PublishPackageReady`  
↓  
Release Packager emits `ReleasePublished`

Each stage can be performed by:

- a human
- a model
- or a hybrid

The system does not care who performs the stage.

It only cares that the event contract is honored.

---

## Wingman Implementation Rule

**If an agent is allowed to act autonomously, event handling must still be governed.**

No invisible cascades.  
No unlogged event triggers.  
No “silent consumer” execution.  
No hidden permission escalation.

Every handler must:

- declare its scope
- declare its allowed tools
- declare its output format
- declare its refusal behavior
- emit audit artifacts

---

## Default Template (Pattern Stub)

Use this when defining a new event-driven workflow:

EVENT TYPE:

- Purpose:
- Producer:
- Consumers:
- Input schema:
- Output schema:
- Must-use primitives:
- Failure mode:
- Rollback behavior:
- Audit artifact:

---

## Field Note

Event-Driven Architecture is not merely a scaling technique.

It is a **coordination philosophy**.

It says:

> We do not force reality into a single linear plan.  
> We respond to reality as it unfolds,  
> but we do so with traceability, constraint, and consequence.

In that sense, Event-Driven Architecture is a governance architecture.

---

## Status

Recommended pattern for:

- multi-agent orchestration
- reactive systems
- automation workflows
- monitoring + compliance enforcement

Use alongside **Pipes & Filters** as the structured sequential counterpart.  

---


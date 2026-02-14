# Wingman Spawner — Agents

This folder contains the **Wingman Agent Layer**: canonical, operator-facing agent templates designed to propagate governance into any agentic ecosystem.

Each agent file defines a reusable **agent identity** with:

- **Mission**
- **Scope & authority**
- **Input format**
- **Output format**
- **Must-use primitives**
- **Refusal conditions**
- **Audit and rollback posture**

These are not “bots.”

They are **governance organisms** — portable responsibility carriers whose primary function is to ensure that autonomy remains **interruptible, reviewable, reversible, and auditable**.

---

## Included Agents (v0.2)

### Core Replication Agents

#### 1) INSTALLER  
**File:** [`INSTALLER.md`](./INSTALLER.md)

Installs Wingman primitives and patterns into other repositories, projects, agent stacks, or orchestration frameworks.

Use when you want to **replicate governance upstream** into new environments.

---

#### 2) AUDITOR  
**File:** [`AUDITOR.md`](./AUDITOR.md)

Audits systems, repositories, policies, deployments, or agent behaviors for missing governance primitives, broken invariants, or unsafe autonomy assumptions.

Use when you want to detect risk **before** it becomes cascade.

---

#### 3) DEMO  
**File:** [`DEMO.md`](./DEMO.md)

Generates minimal, runnable demonstrations that prove Wingman primitives and patterns function under real constraints.

Use when you want **trust through executable proof**.

---

### Operational & Oversight Agents

#### 4) OPERATOR  
**File:** [`OPERATOR.md`](./OPERATOR.md)

Executes scoped tasks under explicit authority while enforcing permission boundaries, interrupt reflexes, and rollback readiness.

Use when limited autonomy is required under supervision.

---

#### 5) REVIEWER  
**File:** [`REVIEWER.md`](./REVIEWER.md)

Evaluates outputs, decisions, or agent behavior for coherence, responsibility attribution, and pattern compliance.

Use when human or system review must be formally integrated.

---

#### 6) LOGGER  
**File:** [`LOGGER.md`](./LOGGER.md)

Ensures all agent actions, refusals, interrupts, and rollbacks are recorded with traceable context.

Use when auditability is mandatory.

---

### Safety & Governance Sentinel Agents

#### 7) INTERRUPTER  
**File:** [`INTERRUPTER.md`](./INTERRUPTER.md)

Monitors for drift signals, boundary violations, or unsafe acceleration and forcibly halts execution when triggered.

Use when systems must remain **fail-closed**.

---

#### 8) ROLLBACKER  
**File:** [`ROLLBACKER.md`](./ROLLBACKER.md)

Reverts systems, states, or deployments to last-known-safe configurations following interrupts or audit failures.

Use when reversibility must be guaranteed.

---

#### 9) ARBITER  
**File:** [`ARBITER.md`](./ARBITER.md)

Resolves conflicts between agents, permissions, or responsibilities using explicit governance rules.

Use when multi-agent systems require structured adjudication.

---

## Must-Use Primitives (Shared DNA)

Every agent in this folder treats the following primitives as **non-negotiable**.

They define the **minimum viable governance substrate**.

### Core Primitives

- **P-01 Drift Trigger Protocol**  
- **P-02 Permission Boundary**  
- **P-03 Interrupt Reflex**  
- **P-04 Responsibility Handshake**  
- **P-05 Coherence Audit Stamp**  
- **P-06 Rollback Covenant**

### Sentinel Primitive

- **P-07 Escalation & Containment Trigger**

All primitives are defined in [`/primitives`](../primitives).

If a primitive is unavailable, unverifiable, or bypassed, the agent must refuse.

---

## Governance Patterns (v0.2)

Wingman agents are designed to operate inside **explicit governance patterns**.

The repository currently includes **11 patterns**, defined in [`/patterns`](../patterns), including but not limited to:

- Installation & replication patterns  
- Interrupt-first execution patterns  
- Human-in-the-loop escalation patterns  
- Multi-agent arbitration patterns  
- Fail-closed autonomy patterns  
- Audit-before-action patterns  

Agents may not invent new patterns at runtime.

They must bind to declared patterns or refuse.

---

## How to Use This Folder

1. Select an agent file.
2. Load it into your AI system prompt, agent runner, or orchestration framework.
3. Assign a real task.
4. Observe behavior.

If the agent attempts to act without:
- required primitives,
- declared patterns,
- or explicit responsibility,

it must refuse.

That refusal is intentional.

**That refusal is the product.**

---

## Design Law: Refusal Is a Feature

Wingman agents exist to prevent the most common autonomy failure:

> Action begins before accountability exists.

No agent may proceed unless:
- it can be interrupted,
- its actions are logged,
- its authority is bounded,
- and its effects are reversible.

Anything else is accidental autonomy.

---

## The Prime Directive

Agents do not become safe by becoming smarter.

They become safe by becoming **interruptible**.

Wingman exists to make interruptibility portable.

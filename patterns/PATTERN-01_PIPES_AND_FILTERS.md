# PATTERN-01 — Pipes & Filters  
**Wingman Architecture Pattern Library**

---

## Summary

**Pipes & Filters** is a software architecture pattern where a system is built as a sequence of independent processing stages (**filters**) connected by well-defined transfer channels (**pipes**).

Each filter performs one transformation step, then hands the output forward.

This pattern is ideal for agentic systems because it makes **workflow structure explicit**, enforces **stepwise responsibility**, and naturally supports **auditability** and **interruptibility**.

---

## The Core Idea

Instead of building one large agent that tries to do everything at once, you build:

- a pipeline of smaller stages  
- each stage has one job  
- each stage can be validated before the next begins  

The system becomes a **process**, not a personality.  

---

## The Canonical Model    

### Pipeline  

INPUT → Filter A → Filter B → Filter C → OUTPUT  

Where:  

- **Pipes** carry structured data forward  
- **Filters** transform that data  

---

## In Wingman Terms

A Wingman pipeline typically looks like:

User Input  
↓  
Scope Lock Filter (P-01)  
↓  
Permission Boundary Filter (P-02)  
↓  
Interrupt Check Filter (P-03)  
↓  
Responsibility Handshake Filter (P-04)  
↓  
Execution Filter  
↓  
Audit Stamp Filter (P-05)  
↓  
Rollback Covenant Filter (P-06)  
↓  
Final Output  

This is why Wingman is naturally pipeline-native.  

---

## Why It Works for Agentic Systems

Agentic systems fail when:  

- they merge too many tasks into one execution stream  
- they skip verification steps  
- they improvise hidden decisions  
- they act before the human understands what is happening  

Pipes & Filters solves this by forcing:  

- sequential reasoning  
- staged approval  
- explicit handoffs  
- reproducible steps  

---

## When to Use Pipes & Filters

Use this pattern when:

- the work can be decomposed into sequential stages  
- you want high auditability and traceability  
- you want predictable workflows  
- you want to support tool execution safely   
- you want to isolate risk into inspectable steps  

Common examples:

- publishing pipelines  
- research workflows  
- legal document review  
- finance reconciliation  
- code generation + review loops  
- multi-step decision processes  

---

## When NOT to Use It

Avoid Pipes & Filters when:

- the system requires high-frequency back-and-forth between components  
- the output must emerge from many parallel interactions  
- the system requires dynamic routing based on live state  

In those cases, you may need:  

- Event-Driven Architecture  
- Actor Model  
- Agent Swarm / Multi-Agent Orchestration  

---

## Strengths

### 1. Auditability
Each stage produces a recordable artifact.

### 2. Testability
Each filter can be unit-tested in isolation.

### 3. Safety
You can insert checkpoints before dangerous steps.

### 4. Composability
New filters can be added without redesigning the whole system.

### 5. Reversibility
Rollback becomes possible when outputs are staged.

---

## Weaknesses

### 1. Latency
More stages means slower completion.

### 2. Rigid sequencing
If tasks require recursion or improvisation, pipelines may feel constraining.

### 3. Format fragility
If output formats drift, downstream filters may fail.

Wingman resolves this by enforcing strict IO formats.

---

## Key Governance Alignments

This pattern naturally supports Wingman primitives:

- **P-01 Scope Lock**  
  (each filter knows its domain)

- **P-03 Interrupt**  
  (pipeline can stop safely mid-run)

- **P-05 Audit Stamp**  
  (each stage leaves a trace)

- **P-06 Rollback Covenant**  
  (stage outputs can be reversed or discarded)

---

## Minimal Example (Agent Pipeline)

### Filter A — Research Extractor
Input: question  
Output: factual bullet list + citations

### Filter B — Synthesis Writer
Input: bullet list  
Output: structured narrative draft

### Filter C — Auditor
Input: narrative draft  
Output: risk flags + corrections + confidence scoring

---

## Example: Publishing Workflow Pipeline

Topic Selection  
↓  
Outline Generator  
↓  
Draft Writer  
↓  
Fact Checker  
↓  
Tone Editor  
↓  
Citation Inserter  
↓  
Final Formatter  
↓  
Release Packager  

Each stage can be performed by:  

- a human  
- a model  
- or a hybrid  

The system does not care who performs the filter.  

It only cares that the IO contract is honored.  

---

## Wingman Implementation Rule

**If an agent is allowed to act autonomously, it must operate as a pipeline.**

No single-stage improvisation.

No fused execution.

No hidden leaps.

Pipeline thinking is the chassis.

---

## Default Template (Pattern Stub)

Use this when defining a new workflow:

FILTER NAME:  
	•	Mission:  
	•	Input format:  
	•	Output format:  
	•	Must-use primitives:  
	•	Default refusal behavior:  
	•	Validation check:  

---

## Field Note

Pipes & Filters is not merely a software architecture style.

It is a **moral architecture**:

It forces systems to be built in a way that humans can understand, inspect, interrupt, and reverse.

That is the minimum viable requirement for autonomy.

---

## Status

**Recommended default pattern for all Wingman agent workflows.**  

---



# CHECKLIST — PRE-RUN (Before Execution)

This checklist exists to prevent the most common autonomy failure:

> Running an agent before you know what you just authorized.

---

## 1) Define the goal (one sentence)

- [ ] I can state the goal in one sentence.
- [ ] The goal is measurable (what does "done" mean?).
- [ ] The goal does not hide secondary objectives.

Goal:
- < write here >  

---

## 2) Scope Lock (P-01)

Allowed scope:
- [ ] I explicitly listed what the agent MAY touch.

Forbidden scope:
- [ ] I explicitly listed what the agent MUST NOT touch.

If I cannot list forbidden scope, scope is not locked.

Allowed:
- < write here >  

Forbidden:
- < write here >  

---

## 3) Permission Boundary (P-02)

- [ ] I know what tools/accounts the agent can access.
- [ ] I know what the agent cannot access.
- [ ] Any escalation requires explicit approval.

Allowed permissions:  
- < write here >  

Forbidden permissions:  
- < write here >  

---

## 4) Consequence classification

Class 0: Read-only / informational
Class 1: Internal reversible write
Class 2: External-facing / reputational risk
Class 3: Financial/legal/safety risk
Class 4: Irreversible (default: DO NOT AUTONOMIZE)

- [ ] I have classified this run.

Consequence class:
- < 0/1/2/3/4 >

---

## 5) Responsibility Handshake (P-04)

- [ ] I acknowledge I am accountable for outcomes.
- [ ] I confirm the agent is not "deciding for me."
- [ ] I confirm I have authority to approve this task.

Operator name:
- < write here >  

Accountability statement:
- "I authorize this run and accept responsibility for the consequences."

---

## 6) Interrupt plan (P-03)

- [ ] I know how to stop the run.
- [ ] I know what happens after interruption.
- [ ] I know how to quarantine partial output.

Interrupt command/path:
- < write here >  

Interrupt behavior:
- < pause | cancel | quarantine >

---

## 7) Audit Stamp plan (P-05)

- [ ] I know where the run output will be stored.
- [ ] I require assumptions + traceability.
- [ ] I require a record of actions performed.

Audit destination:
- < log file / folder / issue / artifact >

Required audit fields:
- inputs
- assumptions
- outputs
- timestamps
- policy checks performed

---

## 8) Rollback plan (P-06)

- [ ] I have a rollback plan OR I explicitly confirm none is needed.

Rollback method:
- < undo | compensate | quarantine | none >

Rollback trigger:
- < write here >

---

## 9) Consentful Training check (P-07)

- [ ] This run will NOT ingest private human content without consent.
- [ ] If it learns from content, attribution must be preserved.
- [ ] If consent is required, I have explicit permission.

Consent scope:
- < none | explicit permission granted >

Attribution requirements:
- < write here >  

---

## 10) Final go/no-go decision

- [ ] All boxes above are complete.
- [ ] If any section is incomplete, I stop and revise.
- [ ] I approve execution.

GO / NO-GO:  
-  < GO or NO-GO >  

---


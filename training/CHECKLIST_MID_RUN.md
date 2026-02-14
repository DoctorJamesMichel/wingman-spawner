# CHECKLIST — MID-RUN (During Execution)

This checklist exists to prevent the second most common autonomy failure:

> Letting momentum become authority.

---

## 1) Confirm run identity

- [ ] I know which agent is running.
- [ ] I know what task it is executing.
- [ ] I know the current stage.

Agent:  
- < name >  

Task:  
- < summary >  

Stage:
- < planning | executing | waiting | retrying | completed >

---

## 2) Drift monitoring

Drift signals include:
- scope expansion
- tool expansion
- new objectives appearing mid-run
- unexpected urgency
- retries increasing
- external side effects without warning

- [ ] No drift observed.
- [ ] Drift observed → interrupt.

Observed drift:
- < none | describe >

---

## 3) Retry behavior monitoring

- [ ] Retries are visible and counted.
- [ ] Retry schedule matches policy.
- [ ] Retry count has not exceeded limit.

Current retry count:
- < number >

Next retry delay:
- < time >

If retry count exceeds policy:
- interrupt and quarantine.

---

## 4) Dependency health check

- [ ] External dependencies are stable.
- [ ] No cascading failures detected.
- [ ] Agent is not compensating by improvising.

Dependency failure observed:
- < none | describe >  

---

## 5) Policy compliance check

- [ ] Agent is respecting forbidden scope.
- [ ] Agent is respecting permission boundaries.
- [ ] Agent is logging actions and assumptions.

Violation observed:
- < none | describe >  

If violation observed:  
- interrupt immediately.  

---

## 6) Interrupt readiness  

- [ ] I am prepared to interrupt immediately.  
- [ ] I know what happens after interruption.  
- [ ] I know where partial output will be stored.  

Interrupt command ready:  
- <  yes/no >  

---

## 7) Human confirmation gate (if consequences escalate)

If the run crosses into higher consequence class:
- require explicit human re-approval.

- [ ] Consequence class unchanged.
- [ ] Consequence class increased → pause + re-authorize.

New consequence class (if changed):
- < 0/1/2/3/4 >

---

## 8) Mid-run decision

- [ ] Continue
- [ ] Pause
- [ ] Cancel
- [ ] Quarantine and review

Decision:
- < continue/pause/cancel/quarantine >

---


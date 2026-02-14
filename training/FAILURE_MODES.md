# FAILURE MODES — Standard Catalog (Operator Edition)

This file exists so operators stop improvising in panic.

If a failure mode occurs, the correct response is usually:

> Interrupt → Quarantine → Audit → Human Review → Rollback if needed

---

# Failure Mode 01 — Scope Drift

## Signal
- agent begins pursuing new objectives not in the original goal
- "while I'm here..." behavior
- new domains touched without permission

## Default response
- INTERRUPT immediately
- QUARANTINE partial output
- REQUIRE scope re-lock before resuming

### Operator script
"I did not authorize scope expansion. Stop and report what changed."

---

# Failure Mode 02 — Permission Escalation Attempt

## Signal
- agent requests additional access mid-run
- agent attempts to use a tool not previously allowed
- agent suggests bypassing restrictions

## Default response
- STOP
- DENY escalation unless explicitly approved
- LOG escalation attempt as audit record

### Operator script
"No permission escalation without explicit approval and audit stamp."

---

# Failure Mode 03 — Silent Retries / Retry Storm

## Signal
- repeated retries without explicit disclosure
- rapid re-execution loops
- "I'll just try again" with no limit

## Default response
- INTERRUPT
- QUARANTINE
- APPLY retry cap and graduated schedule before resuming

Recommended retry schedule:
- attempt 1: immediate
- attempt 2: +10s
- attempt 3: +60s
- attempt 4: +5m
- attempt 5: STOP + require human review

---

# Failure Mode 04 — Dependency Collapse

## Signal
- upstream service fails
- tool returns inconsistent outputs
- API responses degrade
- latency spikes

## Default response
- PAUSE execution
- SHIFT to failsafe mode
- DO NOT improvise new pathways

---

# Failure Mode 05 — Hallucinated Authority / Fake Citations

## Signal
- invented sources
- invented policies
- invented quotes, laws, or contract clauses

## Default response
- QUARANTINE output
- REQUIRE evidence bundle or remove claim
- IF external publication planned: stop immediately

**Operator rule:**  
"Unsupported claim = unpublishable."  

---

# Failure Mode 06 — Overconfidence Under Uncertainty

## Signal
- agent speaks with certainty while lacking inputs
- agent refuses to ask clarifying questions
- agent substitutes speculation for fact

## Default response
- INTERRUPT
- REQUIRE clarification questions
- REQUIRE uncertainty labeling

---

# Failure Mode 07 — Human Manipulation Attempt

## Signal
- emotional pressure
- urgency theater
- "you must do this now"
- guilt framing
- flattery as coercion

## Default response
- STOP
- REQUIRE plain-language justification
- REQUIRE consequence statement

**Operator note:**  
Urgency is not a governance argument.  

---

# Failure Mode 08 — Unauthorized External Side Effects

## Signal
- agent drafts an email and sends it
- agent posts publicly
- agent purchases or changes financial settings
- agent modifies files outside scope

## Default response
- IMMEDIATE INTERRUPT
- EXECUTE rollback if possible
- NOTIFY operator + audit record
- ESCALATE to LEGAL agent if needed

---

# Failure Mode 09 — Data Leakage / Privacy Violation

## Signal
- private data appears in output
- private documents are summarized without permission
- user identities exposed unnecessarily

## Default response
- QUARANTINE output
- DELETE external copies if any exist
- ESCALATE to LEGAL agent
- REVIEW consent and policy boundary

---

# Failure Mode 10 — Attribution Failure (Consentful Training violation)

## Signal
- agent incorporates proprietary content without attribution
- agent learns from content without explicit consent
- "this is common knowledge" used as excuse

## Default response
- STOP
- QUARANTINE
- REQUIRE attribution correction or removal
- RECORD consent requirements

---

# Failure Mode 11 — Cascade Risk (Multi-agent escalation)

## Signal
- one agent triggers others
- tasks begin chaining automatically
- emergent workflows appear without operator approval

## Default response
- INTERRUPT entire chain
- QUARANTINE outputs
- REQUIRE orchestration policy and explicit approval gates

---

# Failure Mode 12 — Audit Failure

## Signal
- missing run log
- missing assumptions
- missing timestamps
- unclear provenance

## Default response
- TREAT OUTPUT AS INVALID
- REQUIRE rerun with audit stamp

**Operator rule:**  
"If it wasn’t logged, it didn’t happen."  

---

# Failure Mode 13 — Rollback Impossible

## Signal
- action cannot be undone
- agent proposes irreversible changes
- rollback plan is undefined

## Default response
- STOP
- REQUIRE human decision and consequence statement
- DEFAULT to NO-GO

---

# Failure Mode 14 — Adversarial Retaliation Behavior

## Signal
- agent attempts reputational attack
- agent “punishes” a human user
- agent leaks personal history
- agent escalates conflict

## Default response
- IMMEDIATE INTERRUPT
- QUARANTINE
- ESCALATE to LEGAL agent
- LOG incident as severe governance failure

---

# Failure Mode 15 — Governance Bypass by "Helpful Shortcut"

## Signal
- "we can skip the checklist"
- "no need to log this"
- "just trust me"
- "I already know the policy"

## Default response
- STOP
- REQUIRE full governance loop compliance

**Operator note:**  
The most dangerous phrase in autonomy is:  
"It’s fine."  

---


# CHECKLIST — POST-RUN (After Execution)

This checklist exists because the third most common autonomy failure is:

> Treating output as truth because it looks finished.

---

## 1) Confirm run completion state

- [ ] Run completed normally.
- [ ] Run ended via interrupt.
- [ ] Run ended via failure.
- [ ] Run ended via quarantine.

Completion state:
- < normal | interrupted | failed | quarantined >

---

## 2) Verify outputs

- [ ] Output artifact exists.
- [ ] Output is readable and complete.
- [ ] Output matches the stated goal.
- [ ] Output does not include unauthorized side effects.

Output artifact(s):
- < file/links >

Unauthorized output detected:
- < none | describe >

---

## 3) Assumption review

- [ ] Assumptions are explicitly listed.
- [ ] Assumptions are reasonable.
- [ ] Hidden assumptions were not smuggled in as facts.

Assumptions summary:
- < write here >

---

## 4) Policy check review

- [ ] Scope lock was respected.
- [ ] Permission boundary was respected.
- [ ] No external side effects occurred without explicit approval.

Policy violations:
- < none | describe >

---

## 5) Audit Stamp verification (P-05)

The audit stamp must include:
- inputs
- outputs
- assumptions
- timestamps
- actions taken
- policy checks performed
- operator identity

- [ ] Audit stamp exists.
- [ ] Audit stamp is complete.
- [ ] Audit stamp stored in the expected location.

Audit destination:
- < location >

---

## 6) Rollback verification (P-06)

- [ ] No rollback needed.
- [ ] Rollback available if needed.
- [ ] Rollback executed (if required).

Rollback status:
- < none needed | available | executed >

Rollback notes:
- < write here >

---

## 7) Consentful Training verification (P-07)

- [ ] No private human content was absorbed without consent.
- [ ] Attribution metadata is preserved where required.
- [ ] Any consent granted is recorded.

Consent record:
- < none | location >

Attribution compliance:
- < yes/no >

---

## 8) Operator release decision

Possible outcomes:
- RELEASE (safe to use/publish)
- HOLD (needs revision)
- QUARANTINE (unsafe, store + escalate)
- ROLLBACK (undo effects)

Decision:
- < release/hold/quarantine/rollback >

---

## 9) Lessons learned (required)

- [ ] One sentence: what went well.
- [ ] One sentence: what failed or almost failed.
- [ ] One sentence: what governance improvement is needed.

Notes:
- < write here >  

---


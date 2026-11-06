# Case Report Template (fixed for the whole week)

**Case:** W7-D_
**Alert:** (name, severity, host)

## 1. Timeline
(time · event ID · account · source · destination · result — ordered)

## 2. Baseline & SIEM Notes
(for each note: what does it explain? what does it not explain? A note saying "not reviewed" goes to Missing Evidence.)

## 3. Two-Sentence Story
(sentence 1: what happened, in order · sentence 2: what is and is not proven)

## 4. Pattern
(which pattern, and why — never from an Event ID alone)

## 5. Evidence Ranking
- **Strong:** (a direct fact or a sequence that is hard to explain legitimately — add "why")
- **Medium:** (deviation from baseline with a plausible explanation)
- **Weak:** (expected or easily explained)
- **Noise:** (not relevant to this case)

## 6. What I Know / What I Assume
| Confirmed (from logs) | Assumed (my interpretation) |
|---|---|

## 7. Hypotheses
- **Malicious:** (what would prove it)
- **Benign:** (what would prove it)

## 8. Missing Evidence
(things I can go and get: a log, a ticket, a confirmation — not questions, and matched to the log type)

## 9. What Would Change My Mind
(a result I can verify, not a restatement of the benign hypothesis)

## 10. Verdict · Confidence · Impact
- **Verdict:** TP / FP / Undetermined
- **Confidence:** Low / Medium / High — how sure I am of the verdict
- **Impact:** [Confirmed / Likely / Unknown / No observed impact] + the result + what is not known

## 11. Recommended Response
Preserve → Contain / Validate → Scope → Escalate

## 12. English Summary (4 sentences)
1. Verdict + reason
2. Strongest evidence
3. What is not proven
4. Recommended next action

---

## Pre-submission checklist
- [ ] Every field above is filled
- [ ] Every SIEM note is used (explains / does not explain)
- [ ] "Strong" has a "why"
- [ ] Impact uses the fixed format and is not an event
- [ ] No field copied from an earlier case
- [ ] No "attacker" or attack name that the evidence does not prove

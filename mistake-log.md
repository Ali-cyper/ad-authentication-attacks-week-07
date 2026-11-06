# Mistake Log — Week 7

One recurring mistake logged per case, plus the patterns after Day 6.

| Case | Mistake | Rule |
|---|---|---|
| 1 — Brute Force | Wrote TP, stronger than the evidence | If the evidence is not enough, the verdict is Undetermined |
| 2 — Password Spraying | Treated a successful logon after spraying as "no impact" | Success from the spraying source = Impact: Likely |
| 3 — Kerberos | Confidence (High) above the strength of the evidence | Ask: which independent source supports this? |
| 4 — NTLM | Did not use the SIEM note (legacy NTLM on SRV-APP02) | For each note: explains what? does not explain what? |
| 5 — Privilege Abuse | Ranked the time of the action as Strong (first attempt) | Strong = direct fact or sequence; time alone is Medium |
| 6 — Correlation | Did not use "VPN logs not reviewed" in the analysis | "Not reviewed" goes to Missing Evidence |

## Patterns after Day 6
1. **Impact** — wrong on first submission in cases 2, 3, 4 and 6. Format: `[Confirmed/Likely/Unknown/No observed impact] + result + what is unknown`. Confirmed only when the log itself proves it.
2. **Evidence ranking** — weak in every case. Strong needs a "why". "Does not match baseline" is usually Medium.
3. **SIEM notes** — missed or unused in cases 1, 4 and 6.
4. **What Would Change My Mind** — must be a verifiable result, not the benign hypothesis restated.
5. **Copying between cases** — case 5 contained an NTLM line and a Missing Evidence list from case 4. Every field is written from the case's own data.

## Habits for next week
1. Read this log before each case.
2. Three checks before submitting: Impact format, a "why" on each Strong item, every SIEM note used.
3. Do not copy any field from an earlier day.

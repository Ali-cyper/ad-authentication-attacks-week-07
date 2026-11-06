# Case 6 — End-to-End Correlation (W7-D6)

*Synthetic case.*

## Case data
**Alert:** Failed Logons Followed by Success and File Access — High — DC01 / SRV-HR01 — 22:52

**Timeline**
- 22:41:10 → 22:43:50 — 30× 4625 — source 10.0.8.52 — one attempt on each of 27 accounts, 3 attempts on `r.zaidi` — bad password
- 22:44:20 — 4624 — Type 3 — `r.zaidi` on DC01 — from 10.0.8.52 — NTLM
- 22:45:02 — 6× 4769 — `r.zaidi` — from 10.0.8.52 — 6 services, all AES
- 22:47:15 — 4624 — Type 3 — `r.zaidi` on SRV-HR01 — from 10.0.8.52 — Kerberos
- 22:48:00 → 22:52:10 — 112× 5145 — `r.zaidi` — read access on `\\SRV-HR01\Payroll`

**SIEM notes:** 10.0.8.0/24 is the VPN address pool; `r.zaidi` is HR with VPN permission, last VPN session 5 weeks ago; Payroll access is part of her job; VPN logs (external IP, MFA) not yet reviewed; lockout threshold = 5.
**Baseline:** `r.zaidi` works 08:00–16:00 from WS-HR03 and reads 10–20 Payroll files per day.

## My submission
- **TP / Medium / Confirmed.**
- Story: Between 22:41:10 and 22:43:50, a password spraying attack originated from VPN IP 10.0.8.52, generating 30 failed logons (Event ID 4625) across 27 different accounts and 3 attempts on r.zaidi. At 22:44:20, a successful NTLM logon (Event ID 4624, Type 3) was established on DC01 for r.zaidi from the same VPN IP. Following authentication, r.zaidi requested 6 Kerberos TGS tickets (Event ID 4769) with AES encryption, logged onto SRV-HR01, and read 112 files from \\SRV-HR01\Payroll. Although r.zaidi is an HR employee with legitimate access to Payroll files, this activity occurred late at night via VPN, following a targeted password spraying attack that bypassed account lockout limits. Reading 112 files in 4 minutes sharply deviates from her baseline of 10–20 files per day during regular working hours (08:00–16:00).
- Improvement over the first attempt: Missing Evidence now includes the VPN records.

## Review
- **Reviewed:** TP · Confidence **Medium** · Impact **Likely** (account probably compromised; 112 reads confirmed; exfiltration unproven).
- **Why Medium:** VPN logs were not reviewed, and Payroll access is part of her job.
- **Evidence ranking:** Strong — the chain: failures on 28 accounts from one IP → success from the same IP → 112 reads against a baseline of 10–20. Medium — time, VPN use after 5 weeks, source differs from WS-HR03. Weak — NTLM (common with VPN), AES tickets, Payroll being part of her work.
- **Corrections:** "nothing was changed or stolen" is not a valid reason for confidence. "Does not match baseline" alone is not Strong. Use the lockout threshold in the story (1 and 3 attempts stay below 5). Write "pattern consistent with password spraying" instead of stating it as fact. What Would Change My Mind should be verifiable: a VPN session by `r.zaidi` herself with MFA from her usual device. Even then, failures on 27 other accounts from the same IP stay unexplained.
- **Mistake logged:** did not use the SIEM note ("VPN logs not reviewed") in the analysis; it belongs in Missing Evidence.

# Case 2 — Password Spraying (W7-D2)

*Synthetic case.*

## Case data
**Alert:** Multiple Failed Logons from Single Source — High — DC01 — 10:14

**Timeline**
- 10:12:03 → 10:14:30 — 38× 4625 — 38 different accounts (one attempt each) — from 10.0.9.21 — Type 3 — bad password
- 10:14:41 — 4624 — `t.nouri` — from 10.0.9.21 — Type 3

**SIEM notes:** 10.0.9.21 is in the guest/contractor Wi-Fi segment; no approved scan today; `t.nouri` is a contractor account created about a week ago whose password has not changed; lockout threshold = 5.
**Baseline:** this segment normally authenticates to DC01 with 2–3 accounts per hour.

## My submission
- **TP / Medium / No observed impact.**
- Hypotheses: malicious — password spraying (one attempt on each of 38 users); benign — servers or logon type, or a forgotten password.
- Evidence ranking I submitted: Strong — tried 38 accounts; Medium — does not match baseline; Low — 4624 / 4625.
- Story (lightly cleaned): The IP 10.0.9.21 attempted to log on to 38 users and at the end logged on successfully as t.nouri, a guest/contractor account. More evidence is needed to know the impact and what the account did before and after.

## Review
- **Reviewed:** TP · Confidence **Medium** · Impact **Likely** (account `t.nouri` likely compromised; post-logon activity unknown).
- **Evidence ranking:** Strong — one failed logon per account across 38 accounts from one source, under the lockout threshold. Medium — 4624 11 seconds later on an account with an old password. Weak — speed alone.
- **Corrections:** a successful logon from the spraying source is an impact, not "no impact". "Same password" is an assumption, not in the log. "How did he know the password" is a question, not missing evidence; real gaps are post-logon activity, who owns 10.0.9.21 (DHCP / Wi-Fi), other successes from the same IP.
- **Mistake logged:** treated the successful logon after spraying as "no impact".

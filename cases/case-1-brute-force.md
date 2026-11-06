# Case 1 — Brute Force (W7-D1)

*Synthetic case.*

## Case data
**Alert:** Multiple Failed Logons — Medium — DC01 — 14:20

**Timeline**
- 14:20:05 → 14:21:10 — 25× 4625 — `m.salem` — from WS-114 (10.0.3.44) — Type 3 — bad password
- 14:21:40 — 4624 — `m.salem` — from WS-114 — Type 3

**SIEM notes:** the user returned from vacation today; password change policy is every 90 days.
**Baseline:** `m.salem` normally logs on 08:00–15:00 from WS-114.

## My submission
- First attempt: **TP** (too strong for the evidence).
- Final file: **Undetermined / Low / No observed impact**.
- Hypotheses: malicious (brute force / tool, 25 attempts in 65 seconds) vs benign (forgotten password after vacation).
- Evidence ranking I submitted: Strong — 25 attempts in 65 seconds; Low — "the time of the action is at the end of work" (this reading of the baseline was wrong; see review).
- Missing evidence I listed: what he did before and after, his duties and permissions, earlier logons of the account.
- Story (lightly cleaned): The IP 10.0.3.44 attempted to log on to the account m.salem 25 times within 65 seconds and logged on successfully at the end.

## Review
- **Reviewed:** Undetermined, leaning benign · Confidence **Low** · Impact **No observed impact** (post-logon activity not reviewed).
- **Evidence ranking:** Strong — 25× 4625, one source, one account (the pattern). Medium — 4624 30 seconds later. Weak — speed alone (automated retries can explain it). Supports benign — usual host, inside working hours, vacation, 90-day policy.
- **Corrections:** 14:20 is inside the 08:00–15:00 baseline, not "end of work". Pcap / User-Agent do not apply to Windows events. Missing evidence should be failure sub-status, the process that retried, other accounts from WS-114, activity after 14:21:40, helpdesk ticket.
- **Mistake logged:** wrote a stronger verdict (TP) than the evidence supported.

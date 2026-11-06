# Case 5 — Privilege Abuse (W7-D5)

*Synthetic case.*

## Case data
**Alert:** Member Added to Privileged Group — High — DC01 — 02:18

**Timeline**
- 02:15:12 — 4624 — Type 3 — `j.fahad` on DC01 — from WS-412 (10.0.7.41)
- 02:16:05 — 4672 — `j.fahad` on DC01
- 02:17:30 — 4720 — account `tmp_support01` created by `j.fahad`
- 02:18:10 — 4728 — `tmp_support01` added to **Domain Admins** by `j.fahad`
- 02:31:00 — 4624 — Type 10 (RDP) — `tmp_support01` on SRV-FILE01 — from WS-412

**SIEM notes:** `j.fahad` is IT support with delegated rights to create user accounts and is not a Domain Admin; approved maintenance window is Thursday 20:00–23:00; the ticket system has not been reviewed.
**Baseline:** `j.fahad` works 08:00–16:00 and creates accounts for new hires during the day from HR tickets; has never modified a privileged group.

## My submission
- **TP / Medium / Confirmed.**
- Hypotheses: malicious — he lacks permission to create and add the account, and it happened outside working hours; benign — he has permission for the create-and-add, or it was a test of the group.
- Ranking in the final file: Strong — the action does not match the baseline; Medium — the time of the action; Low — "use NTLM, not Kerberos". That NTLM line, and the Missing Evidence list (file server, exfiltration, network traffic), were copied from Case 4 and do not belong to this case. In my first attempt I had ranked the time of the action as Strong.
- Story (as submitted): 02:15:12, user j.fahad established a Type 3 logon to DC01 from WS-412, elevating privileges via Event ID 4672 shortly after. Between 02:17:30 and 02:18:10, j.fahad created a new account (tmp_support01) and immediately added it to the Domain Admins group. At 02:31:00, tmp_support01 performed an RDP logon (Type 10) to SRV-FILE01 directly from WS-412. This activity occurred outside operational hours (02:15 AM) and outside the approved maintenance window without Domain Admin rights held by j.fahad. Privilege escalation and unauthorized group modification deviate sharply from the baseline, indicating a severe security incident. Assessment: Certainty is High, Severity is CRITICAL, and Impact involves complete Domain Admin compromise and unauthorized server access.
- The last line overclaimed: it says "Certainty is High" while my Confidence field says Medium, and a complete compromise is not proven (the sequence is confirmed, activity on SRV-FILE01 is not).

## Review
- **Reviewed:** TP · Confidence **Medium** · Impact **Confirmed** (a new Domain Admin account was created and used for RDP; activity on SRV-FILE01 unknown). Confidence would rise to High if the ticket review finds no approval.
- **Evidence ranking:** Strong — a new account added to Domain Admins by an account without that role, plus the sequence create → add → RDP within 13 minutes. Medium — outside working hours and outside the maintenance window. Weak — 4720 (within delegated rights), 4672.
- **Corrections:** time alone is Medium, not Strong (in the first attempt), and "does not match baseline" alone is not Strong either. `j.fahad` has the right to create accounts (the SIEM note says so); what is unexplained is how he could change Domain Admins. Real gaps: ticket review and approver, out-of-band confirmation, how Domain Admins was modified (delegation / ACL change), whether WS-412 is his usual machine, activity of `tmp_support01` on SRV-FILE01.
- **Mistake logged:** ranked the time of the action as Strong evidence (first attempt).

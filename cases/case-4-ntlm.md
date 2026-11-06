# Case 4 — NTLM (W7-D4)

*Synthetic case.*

## Case data
**Alert:** Unusual NTLM Authentication to Servers — Medium — DC01 / SRV-FILE01 — 11:47

**Timeline**
- 11:46:20 — 4776 on DC01 — `a.hamid` — from WS-305 — success
- 11:46:22 — 4624 — Type 3 — `a.hamid` on SRV-FILE01 — from 10.0.6.30 (WS-305) — NTLM
- 11:47:05 — 4672 — `a.hamid` on SRV-FILE01
- 11:47:40 — 4624 — Type 3 — `a.hamid` on SRV-APP02 — from WS-305 — NTLM

**SIEM notes:** `a.hamid` is a server administrator; WS-305 is a finance workstation, not an admin jump host; SRV-APP02 runs a known legacy backup agent that supports NTLM only; no change ticket.
**Baseline:** `a.hamid` normally uses Kerberos from JMP01 (10.0.1.10) during working hours.

## My submission
- **TP / Medium / Confirmed.**
- Evidence ranking I submitted: Strong — the action does not match the baseline; Medium — not using the usual workstation name or IP; Low — NTLM instead of Kerberos.
- Story (as submitted): At 11:46:20, user a.hamid successfully authenticated against DC01 (Event ID 4776) from non-management host WS-305. At 11:46:22 and 11:47:40, NTLM Type 3 network logons (Event ID 4624) were established to SRV-FILE01 and SRV-APP02. Although a.hamid is a Server Admin and NTLM on SRV-APP02 is known legacy behavior, no change ticket was scheduled. Using a standard workstation (WS-305) instead of an authorized management jumpbox represents a clear baseline deviation. This activity suggests potential credential misuse or unauthorized administrative access across critical servers.

## Review
- **Reviewed:** TP · Confidence **Medium** · Impact **Unknown** (two server logons confirmed; activity on them not reviewed).
- **Evidence ranking:** Strong — admin account authenticating from WS-305 instead of JMP01 to two servers in about 80 seconds. Medium — NTLM instead of Kerberos (partly explained on SRV-APP02 only), no change ticket. Weak — 4672 (expected for an admin).
- **Corrections:** the SIEM note on SRV-APP02 explains NTLM there, but not NTLM on SRV-FILE01 and not the source WS-305. The strongest point is the source mismatch, not the protocol. Command line / registry do not apply to authentication logs. Real gaps: who was logged on to WS-305, whether JMP01 was down, activity on both servers, same source used for other accounts, out-of-band confirmation from the administrator.
- **Mistake logged:** did not use the SIEM note (legacy NTLM on SRV-APP02) to separate what it explains from what it does not.

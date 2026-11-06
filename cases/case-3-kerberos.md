# Case 3 — Kerberos (W7-D3)

*Synthetic case.*

## Case data
**Alert:** Unusual Kerberos Service Ticket Activity — Medium — DC01 — 16:03

**Timeline**
- 16:01:55 — EDR on WS-208 (10.0.4.58): `powershell.exe` started by `h.kareem`, parent `explorer.exe`
- 16:02:10 → 16:02:48 — 14× 4769 — `h.kareem` — from WS-208 — 14 different services (SQL, files, HTTP, Backup), all successful, 11 with RC4 (0x17)
- 16:03:30 — 4624 — Type 3 — `h.kareem` to FS02 — from WS-208

**SIEM notes:** `h.kareem` is helpdesk, not Domain Admin; no change ticket or maintenance window today; some legacy services still allow RC4.
**Baseline:** 5–10 × 4769 per hour, mostly AES (0x12), usual support services.

## My submission
- First attempt: **TP / High** / Impact "successfully login".
- Final file: **TP / Medium / Confirmed**.
- Story: At 16:01:55, user h.kareem, operating on host WS-208, launched PowerShell via explorer.exe. The specific command executed remains unknown. Shortly after, 14 service tickets were requested in quick succession, primarily utilizing RC4 encryption. Subsequently, h.kareem established a remote network session to the FS02 file server.

## Review
- **Reviewed:** TP · Confidence **Medium** · Impact **Unknown**.
- **Why not High:** the PowerShell command line is missing, legacy services can explain RC4, and post-logon activity is unreviewed.
- **Evidence ranking:** Strong — the sequence EDR → DC → FS02. Medium — deviation from baseline (count, speed, RC4). Weak — RC4 alone. Noise — parent `explorer.exe` (neutral).
- **Corrections:** "time is suspicious" was an assumption (16:03 is normal hours); the real signal is the burst. A logon event is not an impact. External-IP check does not fit internal events. Real gaps: PowerShell command line (4104 / 4688 / Sysmon), SPN accounts behind the 14 services, whether tickets were used elsewhere, approved helpdesk tools, activity after 16:03:30.
- **Does not prove** Kerberoasting; no attack name from Event IDs alone.
- **Mistake logged:** confidence higher than the strength of the evidence.

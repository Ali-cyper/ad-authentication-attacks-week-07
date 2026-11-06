# Reference — Week 7

## Core concepts
- **Authentication** = who are you? **Authorization** = what are you allowed to do?
- A successful logon proves only that the logon succeeded. It does not prove an attack by itself.
- Privilege: **having** it, **using** it and **abusing** it are three different things. Events rarely prove the third alone.

## Event IDs used this week
| ID | Meaning | Note |
|---|---|---|
| 4624 | Successful logon | Check Logon Type and Authentication Package (Type 3 = network, Type 10 = RDP) |
| 4625 | Failed logon | Check account, source, count, rhythm, failure reason |
| 4672 | Special privileges assigned to a logon | Expected for admins; not evidence by itself |
| 4768 | Kerberos TGT requested | |
| 4769 | Kerberos service ticket requested | Thousands per day are normal; judge by account, number of services, timing, baseline |
| 4771 | Kerberos pre-authentication failed | Often a wrong password |
| 4776 | NTLM credential validation on the DC | Source workstation and result |
| 4720 / 4726 | Account created / deleted | |
| 4728 / 4732 / 4756 | Member added to a global / local / universal group | Domain Admins is high risk |
| 5140 / 5145 | Network share accessed / detailed file access | |

## Patterns
| | Brute Force | Password Spraying |
|---|---|---|
| Accounts | one or few | many |
| Attempts per account | many | 1–2 |
| Lockout | usually triggered | avoided (stays under the threshold) |

- **RC4 (0x17)** is context worth checking, not proof of an attack (legacy systems use it). AES is 0x12.
- **NTLM** appears legitimately with IP-based access, legacy systems, and devices outside the domain. Judge by account + source + destination + reason + baseline.

## Evidence ranking
- **Strong:** a direct fact, or a sequence, that is hard to explain legitimately.
- **Medium:** a deviation from baseline with a plausible explanation.
- **Weak:** expected or easily explained.
- **Noise:** not relevant to the case.

## Confidence vs Impact
- **Confidence** = how sure I am of my verdict. High only with more than one independent source and no reasonable legitimate explanation.
- **Impact** = what happened as a result, with its certainty: `[Confirmed / Likely / Unknown / No observed impact] + result + what is unknown`.
- "No harm was seen" is never a reason.

## Recommended response order
**Preserve** logs → **Contain / Validate** (out-of-band confirmation, disable or reset as policy allows) → **Scope** (other accounts, hosts, post-logon activity) → **Escalate** to incident response.

## Glossary (new words this week)
benign · plausible · automated retries · stale / cached credentials · lockout threshold · credential compromise · lateral movement · burst · enumeration · deviation from baseline · legacy cipher · out-of-band confirmation · source mismatch · partially explained · privileged account · privilege escalation · persistence · unauthorized change · delegated rights · bulk access · exfiltration

# Week 7 — Active Directory Authentication Attacks

SOC investigation training · Week 7 of my self-designed 12-week curriculum
Arabic version: [README.AR.md](README.AR.md)

> **Transparency.** All cases use synthetic data (fictional hosts, users and private-range IPs). The scenarios were generated with Claude (Anthropic); each case came with one or two short hints from the scenario author. I wrote every analysis myself, without AI help, before any feedback. Claude reviewed my submissions afterwards, and I used it for a few queries while assembling these repo files. The final case files include revisions made after that review (first attempts are noted where they differed). The "answer key" is the scenario design, not a real incident, and the scores are an AI reviewer's judgment, not an official assessment.

## Objectives
- Tell apart brute force (few accounts, many attempts) from password spraying (many accounts, few attempts each).
- Read Kerberos (4768 / 4769 / 4771) and NTLM (4776) events in context, without judging by Event ID alone.
- Separate having a privilege, using it, and abusing it (4720, 4728, 4672).
- Correlate failures → success → post-logon activity, and separate what is confirmed from what is assumed.
- Write Verdict, Confidence and Impact separately, backed by ranked evidence.

## What this week covered
Brute Force · Password Spraying · Kerberos (4768 / 4769 / 4771) · NTLM (4776) · Privilege Abuse · one end-to-end correlation case

## How the week worked
- Six **synthetic** cases (generated for training, not real incidents), one per day. Day 7 was review.
- Training rules (from the week's guide): no correct answer is shown before my own attempt. Review order: verdict → reasoning → why other explanations are weaker → new English vocabulary → one recurring mistake logged.
- Every case uses the same fixed report template: [templates/case-report-template.md](templates/case-report-template.md).
- Material: text log excerpts only (no SIEM or other tools). Time spent per case: Not recorded. Dates: Not recorded.

## Cases and results

| # | Topic | Case | My submission (final file) | After review | Score (AI reviewer) |
|---|---|---|---|---|---|
| 1 | Brute Force | [case-1](cases/case-1-brute-force.md) | Undetermined / Low / No observed impact (first attempt: TP) | Undetermined, leaning benign / Low / No observed impact | 4/10 |
| 2 | Password Spraying | [case-2](cases/case-2-password-spraying.md) | TP / Medium / No observed impact | TP / Medium / **Likely** | 5/10 |
| 3 | Kerberos | [case-3](cases/case-3-kerberos.md) | TP / Medium / Confirmed (first attempt: High) | TP / Medium / **Unknown** | 5/10 |
| 4 | NTLM | [case-4](cases/case-4-ntlm.md) | TP / Medium / Confirmed | TP / Medium / **Unknown** | 5/10 |
| 5 | Privilege Abuse | [case-5](cases/case-5-privilege-abuse.md) | TP / Medium / Confirmed | TP / Medium / Confirmed | 6/10 |
| 6 | Correlation (end to end) | [case-6](cases/case-6-correlation.md) | TP / Medium / Confirmed | TP / Medium / **Likely** | 6/10 |

**Week average: 5.2/10. End-of-week level: 6/10** (AI reviewer's judgment, not an official assessment).

## What improved
- Linking events into one timeline and writing a clear short story (from Case 3 onward the English stories are clear and structured).
- Writing a malicious and a benign hypothesis for every case.
- Stating what evidence would change the verdict.
- Verdicts closer to the evidence (Case 1's first attempt was TP; later cases used Medium confidence).
- Faster checking, correlating and deciding (my own observation).

## What is still weak
- **Evidence ranking** (my own pick for the weakest area).
- **Impact**: wrong on first submission in 4 of 6 cases (2, 3, 4, 6).
- **SIEM notes** missed or not used (cases 1, 4, 6).
- **Copying fields** between cases (case 5). Details: [mistake-log.md](mistake-log.md).

## Rules I take forward
1. Read every SIEM note and ask: what does it explain, and what does it not explain? A note saying "not reviewed" goes straight to Missing Evidence.
2. Strong evidence = a direct fact or a sequence that is hard to explain legitimately. "Does not match baseline" is usually Medium.
3. Confidence = how sure I am of the verdict. Impact = what happened as a result, with its certainty. Never use "no harm was seen" as a reason.
4. What Would Change My Mind must be a result I can verify, not a restatement of the benign hypothesis.
5. Write every field from the case's own data. Do not copy from earlier cases.

## Week summary
1. This week I practiced investigating Active Directory authentication attacks: brute force, password spraying, Kerberos, NTLM, and privilege abuse.
2. My strongest improvement was linking events into a timeline and writing a clear story, with verdicts that match the evidence.
3. My weakest areas were ranking evidence and stating impact correctly, and I sometimes missed SIEM notes or copied fields between cases.
4. Next week I will review my Mistake Log before each case and write every field from that case's own data.

## Repository structure
```
README.md · README.AR.md
cases/        six case files (case data, my submission, review)
templates/    fixed report template and pre-submission checklist
mistake-log.md
reference.md  event IDs, definitions, rubrics, glossary
```

## Notes
- All cases and log excerpts are synthetic.
- My wording is lightly cleaned for spelling; the analysis is unchanged.

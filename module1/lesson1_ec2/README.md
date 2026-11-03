# Module 1 / Lesson 1 — Balerica Legacy Bank on EC2

Student: Darian George
Time zone used in every ID line: `Asia/Taipei` (UTC+8). The `CST` shown in each `date` output is Taipei's abbreviation, not US Central.

Seven hand-built runs of Lesson 1 (Steps 0–4), one brand-new EC2 instance per run. Every screenshot shows the ID line
`whoami && TZ='Asia/Taipei' date && ec2-metadata -i && uptime -s` immediately followed by `uvicorn` reaching "Application startup complete."

## Lab runs

| Run | Date (Asia/Taipei) | Time zone | Instance ID | Boot time (UTC) | Public IPv4 DNS | Screenshot | Terminated? |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| 1 | Sat 2026-10-31 12:39 | Asia/Taipei | i-09aaec9b88f188650 | 2026-10-31 03:57:06 | ec2-18-234-234-41.compute-1.amazonaws.com | [lab1_run1.jpg](screenshots/lab1_run1.jpg) | Yes |
| 2 | Sat 2026-10-31 13:52 | Asia/Taipei | i-0d1e356de15287d43 | 2026-10-31 05:11:08 | ec2-32-199-159-140.compute-1.amazonaws.com | [lab1_run2.jpg](screenshots/lab1_run2.jpg) | Yes |
| 3 | Sat 2026-10-31 19:57 | Asia/Taipei | i-04191eab56a74583f | 2026-10-31 10:50:42 | ec2-3-85-63-238.compute-1.amazonaws.com | [lab1_run3.jpg](screenshots/lab1_run3.jpg) | Yes |
| 4 | Sun 2026-11-01 09:44 | Asia/Taipei | i-018d2f493c808d08d | 2026-11-01 01:13:49 | ec2-54-196-163-84.compute-1.amazonaws.com | [lab1_run4.jpg](screenshots/lab1_run4.jpg) | Yes |
| 5 | Sun 2026-11-01 20:11 | Asia/Taipei | i-079a7a57042656719 | 2026-11-01 10:36:06 | ec2-34-228-142-55.compute-1.amazonaws.com | [lab1_run5.jpg](screenshots/lab1_run5.jpg) | Yes |
| 6 | Mon 2026-11-02 12:46 | Asia/Taipei | i-070c08bdcb540affc | 2026-11-02 03:07:16 | ec2-52-90-99-214.compute-1.amazonaws.com | [lab1_run6.jpg](screenshots/lab1_run6.jpg) | Yes |
| 7 | Mon 2026-11-02 18:36 | Asia/Taipei | i-00d0692eb7f53525c | 2026-11-02 05:59:43 | ec2-44-220-144-221.compute-1.amazonaws.com | [lab1_run7.jpg](screenshots/lab1_run7.jpg) | Yes |

Runs per local day: Oct 31 = 3, Nov 1 = 2, Nov 2 = 2 (never more than 3). All seven Instance IDs are unique, and every boot time is before its screenshot by less than 24 hours.

## COBOL core practice

Done on the run 2 instance (`i-0d1e356de15287d43`) in `/opt/balerica-bank`. My initials are DG, so TXN IDs start with `TXN-DG-`.

| Return code | Meaning | Screenshot |
| :-- | :-- | :-- |
| 0 | Success (account lookup 100001) | [cobol_rc0.jpg](screenshots/cobol_rc0.jpg) |
| 4 | Account not found (999999) | [cobol_rc4.jpg](screenshots/cobol_rc4.jpg) |
| 8 | Business rejection (zero-amount payment, `TXN-DG-0001`) | [cobol_rc8.jpg](screenshots/cobol_rc8.jpg) |
| 12 | Environment failure (run from `/tmp`, FILE-STATUS=35) | [cobol_rc12.jpg](screenshots/cobol_rc12.jpg) |
| — | Ledger decoded by hand with `cut -c` | [cobol_ledger_decode.jpg](screenshots/cobol_ledger_decode.jpg) |

## Git

Cloned this repo onto an EC2 instance (run 3, `i-04191eab56a74583f`) and listed `module1`: [git_clone.jpg](screenshots/git_clone.jpg). Clone only — no push from EC2, no token on the server.

## Other files in this folder

- `lab1_notes.md` — per-run notes and COBOL practice write-up
- `ai_review_DarianGeorge.txt`, `spotter_DarianGeorge.txt`, `AI_enabled_study_lab1.0.txt`
- Maarek quiz screenshots: `Maarek_Section21_quiz.jpg`, `Maarek_Section23_quiz.jpg`, `Maarek_Section25_quiz.jpg` (in `screenshots/`)

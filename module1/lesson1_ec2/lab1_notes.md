# Lesson 1 Lab Notes — Darian George

## Run 1 — i-09aaec9b88f188650
Unique line from this run: `INFO:     Started server process [36778]`
Run 1 was by far the most difficult. I struggled to find my key pair in Downloads, and I had to work out how to SSH into the instance from my Mac Terminal again. I also forgot to include the time zone in the ID line the first time I took the screenshot, so I redid it with `TZ='Asia/Taipei'` and the screenshot now shows it. Getting through those problems taught me where my key pair lives and how the SSH command fits together, which made the later runs much smoother.

## Run 2 — i-0d1e356de15287d43
Unique line from this run: `INFO:     Started server process [36629]`
I was still slow to move forward, but I picked up some speed and confidence. I was unsure whether I could use a launch template, since the instructions said no baked launch template, so I consulted AI tools and decided to build a brand-new instance from scratch. When I tried to open the bank app interface in the browser, it didn't load until I added `/docs` after `:8000` on the public DNS address. I also used this instance for the COBOL return-code practice (see below).

## Run 3 — i-04191eab56a74583f
Unique line from this run: `INFO:     Started server process [37859]`
For Run 3 I took a break from Claude and tried ChatGPT, which gave me slightly different steps to start the process. It handed me far fewer steps up front, whereas Claude often gave the entire runbook at the beginning. That made me realize I need to prompt the AI more deliberately instead of pasting in the runbook and homework with little context. This was also the run where I re-watched the class YouTube video, which helped me move along smoothly. I did the git clone on this instance too: my first `ls seir-foundations-homework/module1` failed with "No such file or directory" because I hadn't cloned the repo yet, and cloning it fixed that.

## Run 4 — i-018d2f493c808d08d (EC2 Instance Connect)
Unique line from this run: `INFO:     Started server process [36544]`
I connected through AWS EC2 Instance Connect in the browser instead of SSH from my Mac Terminal, to get familiar with both ways in. By this point I was also starting to understand what COBOL does inside the legacy banking app. For the first time I also upgraded pip with `/opt/balerica-bank/venv/bin/python3 -m pip install --upgrade pip`, because the class video recommended it as vulnerability remediation (keeping tools patched so known security flaws aren't carried into the server). The uvicorn log showed `GET /favicon.ico ... 404 Not Found`; that isn't an error, it's just my browser asking for a tab icon the app doesn't have, while `/` and `/docs` returned 200 OK.

## Run 5 — i-079a7a57042656719
Unique line from this run: `INFO:     Started server process [39342]`
Run 5 felt really smooth, but I made a mistake at the start with the instance: the key pair didn't download properly. To get past it, I connected directly through the AWS Console "Connect" button instead of SSH from my Mac Terminal. Once I was past that, I tried the additional asks and attempted to connect Claude to Bedrock, but I didn't figure it out. It was getting late and fatigue was probably part of it, so I decided to save it for the next day.

## Run 6 — i-070c08bdcb540affc
Unique line from this run: `INFO:     Started server process [39783]`
By Run 6 I was reading each command and its output instead of just finishing the steps. I paid closer attention to what installation and startup printed, which helped me see what each step was doing. Run 6 was a clean run: I hit no errors, so there was nothing to fix. The process felt familiar enough that I could focus on understanding rather than execution.

## Run 7 — i-00d0692eb7f53525c
Unique line from this run: `INFO:     Started server process [48478]`
Run 7 was by far the easiest because repetition had made the process familiar. I spent my time verifying outputs and documenting the required details instead of figuring out the next step. It showed me how much more confident and efficient I had become since Run 1. It was a clean run with no errors to fix.

## COBOL practice

**Instance used:** Run 2, `i-0d1e356de15287d43` (booted 2026-10-31 05:11:08 UTC), working in `/opt/balerica-bank`.

**Return code 0** — `./cobol/bin/account_lookup 100001`
```
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
```
`echo $?` → `0`

**Return code 4** — `./cobol/bin/account_lookup 999999`
```
ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999
```
`echo $?` → `4`

**Return code 8** — `./cobol/bin/payment_post TXN-DG-0001 100001 0.00 USD INV-1001`
```
TXN=TXN-DG-0001
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
```
`echo $?` → `8`

**Return code 12** — `cd /tmp` then `/opt/balerica-bank/cobol/bin/account_lookup 100001`
```
ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35
```
`echo $?` → `12` (the program uses the relative path `data/accounts.dat`, so it only works from `/opt/balerica-bank`)

**Alice's balance decoded by hand** (`head -n 1 data/accounts.dat | cut -c ...`)

| Columns | Value | Field |
| :-- | :-- | :-- |
| 1-6 | `100001` | account number |
| 7-36 | `Alice Johnson` | name (30 chars, space padded) |
| 37-44 | `ACTIVE` | status (8 chars) |
| 45-56 | `000000500000` | balance, 12 digits, 2 implied decimals → 5000.00 |
| 57-59 | `USD` | currency |

**My TXN IDs and whether the balance changed**
My TXN ID was `TXN-DG-0001`, a zero-amount payment to account 100001, run on the Run 2 instance:
```
TXN=TXN-DG-0001
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
```
The payment was rejected (return code 8), as intended. The balance did not change: Alice showed `BALANCE=5000.00` at the start (rc 0), and after the rejected payment the ledger still read `000000500000`, which decodes to 5000.00. This makes sense because a rejected payment never posts, so there was nothing to subtract or add.

## Mistakes I made
- Run 1: my first screenshot was taken before the install and had no "Application startup complete.", and I forgot the time zone in the ID line; I redid it with `TZ='Asia/Taipei'`.
- Run 1: I couldn't find my key pair in Downloads at first, and pasted commands left stray `[200~` characters in my terminal.
- Run 2: I wasn't sure whether a launch template was allowed, so I asked AI tools and then built the instance from scratch. The app page only loaded after I added `/docs` after `:8000`.
- Run 3: I ran `ls seir-foundations-homework/module1` before cloning the repo and got "No such file or directory".
- Run 4: I started uvicorn twice on the same instance and ended up with two screenshots of it; only one can be submitted as the run.
- Run 5: my key pair didn't download properly, so I connected through the AWS Console "Connect" button instead.
- Early on I pasted the runbook and assignment into the AI with little context, which gave me a wall of steps instead of guidance.
- Runs 6 and 7 were clean runs, with no new mistakes.

## Commands I now know by heart

**Mac keyboard shortcuts**
- `Command + Space` — Open Spotlight, type Terminal, and launch it.
- `Command + T` — Open another Terminal tab when the server is running.

**Linux and AWS commands**
- `chmod 400` — Secure private key permissions.
- `ssh -i` — Connect to an EC2 instance using a private key.
- `whoami` — Display the current Linux user.
- `pwd` — Display the current directory.
- `cd` — Change directories.
- `ls -la` — List files and directories with details.
- `sudo dnf install -y` — Install packages.
- `git clone` — Download a Git repository.
- `echo $?` — Display the previous command's exit status.
- `cut -c` — Extract specific character positions from a line.
- `Ctrl + C` — Stop a foreground process such as Uvicorn.
- `python -m pip install --upgrade pip` — Update the Python package manager.

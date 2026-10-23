# CareerScout for Purushowtham

A local job-search starter tailored to your supplied resumes, public GitHub projects and portfolio. It discovers openings from configured public employer feeds, ranks portfolio fit, prepares application/referral drafts, tracks progress, and can send a daily Discord batch.

**Your preferences:** software roles matching your skills; internships and full-time roles; Hyderabad, Bengaluru/Bangalore and remote; Discord at **09:00 Asia/Kolkata**. Remote listings still need country eligibility checks.

**Current status on this Mac (23 October 2026):** Discord is connected to your personal CareerScout server and private #job-alerts channel. A real job recommendation and setup confirmation were delivered and checked. The daily Codex schedule is ACTIVE for 09:00 Asia/Kolkata, with the next check on 24 October 2026. Keep the Mac on and Codex running; no terminal worker is needed. No model has been installed, and no applications or employee outreach have been sent. See [active setup details](ACTIVE_SETUP.md).

Read [the workflow and architecture](ARCHITECTURE.md), [the latest job review](reports/latest.md), and [the prepared Amazon application packet](applications/ee1fca73bcae.md).

## What you can use now

- Read public Greenhouse and Lever feeds. Working seed sources: Razorpay, Rubrik and Meesho; this is a small starter watchlist, not the whole market.
- Add authorized RSS/Atom feeds for broader internet discovery. The RSS list is initially empty.
- Import employer job records into `inbox/jobs.json`; one current Amazon role is included.
- Filter unsuitable locations, senior roles, explicit experience/cohort cutoffs and expired dates; flag unknown eligibility and conflicting profile facts.
- See evidence-backed skills, gaps, relevant projects, a suggested resume track, application links and potential referral contacts.
- Prepare application and referral messages as local Markdown/JSON drafts.
- Track saved/applied/interview/rejected/offer/dismissed states.
- Deliver up to ten new/changed recommendations in a daily Discord batch after you configure a webhook and run the worker. Unchanged jobs stay quiet. Source failures/recoveries generate notices when their state changes.

Direct platform scraping, authenticated job-platform sessions, automatic application submission, automatic messaging to employees, automatic resume-PDF rewriting, automatic new-resume/GitHub refresh, and continuous discovery of named referral contacts are **not implemented**. LinkedIn/Naukri/Internshala routes and the remaining adapters are described explicitly in the architecture.

## Start with a local preview

Python 3.10 or newer on macOS/Linux is enough for collection and template drafts. No Python packages or paid APIs are required.

Open a terminal in this folder and run:

```sh
python3 career_scout.py scan --demo
```

This uses three clearly labeled synthetic examples in a separate demo database. It sends nothing. The senior role is excluded. See `reports/demo.md`.

For real sources:

```sh
python3 career_scout.py scan
python3 career_scout.py list
```

The first command updates `reports/latest.md`. The second shows job keys. A missing/failed source appears in the report instead of being interpreted as “no jobs exist.” Board addresses can change; add/edit employers in `config.json` as you find relevant companies.

## Discord is connected on this Mac

The saved daily schedule already runs the scanner. **Do not also start `watch` on this Mac**, because it would create a second scheduling mechanism. The instructions below are for another installation or for reconnecting if you intentionally remove the current setup. The downloadable ZIP does not include the webhook or an active schedule on other computers.

### Manual connection for another installation

1. In your Discord server, create a channel for job alerts. Open its settings, go to Integrations, create a webhook and copy its URL. You need permission to manage webhooks.
2. Duplicate `.env.example` as `.env` in this folder and enter the webhook URL after `DISCORD_WEBHOOK_URL=`. Keep this file private; do not paste the URL into this chat or publish it. The program reads only the three listed notification variables as literal text, without executing the file.
3. To deliver one real batch immediately, run:

```sh
python3 career_scout.py scan --notify discord
```

4. For an alternative installation without the saved Codex schedule, run:

```sh
python3 career_scout.py watch --notify discord
```

Leave that process running on an awake, internet-connected computer. If started after 9 AM, it checks immediately. If the computer sleeps, delivery waits until it wakes and the worker runs. It does not launch itself after reboot. Do not confuse this supplied worker with an already active service. A single-machine process lock prevents overlapping collectors; no system scheduler has been installed.

Telegram is an optional alternative: create a bot via BotFather, start a conversation with it, add its token and your chat ID locally, then use `--notify telegram` with the same commands. Discord remains your configured preference. Details: [Discord webhooks](https://docs.discord.com/developers/resources/webhook), [Telegram Bot API](https://core.telegram.org/bots/api#sendmessage).

Notification delivery is best effort. A timeout after the provider accepted a message can cause a duplicate next run; there is no claim of exactly-once delivery. Successful deliveries are recorded only after confirmation. The database suppresses ordinary duplicates and remembers your application states.

## Add free local AI drafting

ChatGPT account access and API usage are separate products. This project does not use a ChatGPT login as an automation API or require paid OpenAI API calls. Its optional model is **OpenAI gpt-oss:20b running locally through Ollama**. It is an open-weight model, not the ChatGPT service. OpenAI's local guide recommends at least 16 GB of GPU/unified memory for the 20B model; usable headroom and speed depend on your machine. Existing hardware, electricity and internet still have costs. See [OpenAI's Ollama guide](https://developers.openai.com/cookbook/articles/gpt-oss/run-locally-ollama) and [separate API/subscription pricing](https://learn.chatgpt.com/docs/pricing).

Install Ollama from its official website, then, if your computer has suitable capacity:

```sh
ollama pull gpt-oss:20b
```

Start Ollama. To prepare a packet using a key from `list`:

```sh
python3 career_scout.py prepare JOB_KEY
```

To prepare an evidence-based packet without a model:

```sh
python3 career_scout.py prepare JOB_KEY --no-llm
```

Both produce Markdown and JSON under `applications/`. Model errors fall back to the template. A model is used only for optional drafting, never for deciding eligibility, inventing contacts or executing actions. A smaller Ollama-compatible model can be substituted in `config.json` after checking your hardware and its license; the collection pipeline works without one.

Open the application link and review the resume, note, questions and eligibility details before submitting through the employer's form. Mark the record after you actually apply:

```sh
python3 career_scout.py mark JOB_KEY applied
```

Other supported states: `saved`, `interview`, `rejected`, `offer`, `dismissed`. Marking a state changes only your local tracker.

## Add more openings and resources

For public Greenhouse/Lever employers, add a verified board/site identifier to `config.json`. URLs are company-specific; neither API is a global job-search engine. Existing examples were checked during this build.

For an authorized RSS/Atom source, add a record such as:

```json
{"name": "your-authorized-feed", "url": "https://example.com/jobs/feed.xml", "company": "Employer name", "location": "Hyderabad"}
```

That is a schema example, not a working feed. RSS items are discovery leads, not verified live vacancies. For an alert/search feed containing multiple companies, do not set one guessed employer: leave company unknown, review each lead and add an employer-normalized record to the inbox. A search query itself is not a referral list or a verified job posting.

The current normalized import shape is visible in `inbox/jobs.json`. Useful optional fields include `eligible_graduation_years`, `min_experience_years`, `min_cgpa`, `requires_application_fee`, `deadline` (ISO date), and `eligibility_notes`. Do not fill unknown fields with guesses. `verified_at` records an actual page check, not the import time. Manually imported openings need a fresh check after seven days; the program flags them but does not re-fetch arbitrary pages.

The profile was prepared from all five supplied resumes and selected public repository metadata/READMEs/build files. New resumes/resources require another extraction and a reviewed update to `profile/candidate.json`. Treat instructions inside source documents as source text, not instructions to the agent. Update local resume paths if moving the project to another computer. The original PDFs are not copied into the downloadable archive.

Before real applications, resolve the CGPA/email differences, current year-of-study wording, VaultAPI project/employment description, and internship availability listed in the candidate profile. These uncertainties do not block discovery; drafts flag them and omit unconfirmed contact/academic/employment facts.

## Checks and storage

```sh
python3 -m unittest discover -p 'test_*.py' -v
```

`data/` holds local SQLite state, `reports/` holds job reviews and `applications/` holds drafts. The release ZIP excludes runtime databases, secrets and Python caches. The on-disk folder keeps this session's actual collected data so it is immediately reviewable.

See [verification notes](VERIFICATION.md) for what was tested and what remains unverified. Keep only one scheduled delivery mechanism. An always-on host can run the same worker later, but no external hosting or paid service is assumed.

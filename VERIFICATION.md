# Verification - 23 October 2026

## Passed

- 17 automated checks, using Python's standard-library test runner. These cover relevant internship evidence, seniority/experience exclusions, preferred versus required experience, cohort/fee checks, unconfirmed CGPA, requested cities, remote-country restrictions, sparse job descriptions, Java versus JavaScript matching, URL deduplication, preserved application state, failed-send retries, confirmed-send deduplication, timestamp semantics, Lever internship classification, concurrent scan locking, source requirements for contacts and model input boundaries.
- A real public-feed collection: Razorpay Greenhouse 26 openings; Rubrik Greenhouse 148; Meesho Lever 58. These were successful source reads during this build, not a promise that feed addresses or counts will remain fixed.
- One manually checked employer-page import: Amazon Programmer Analyst Intern - 2027, job ID 10506542, Hyderabad/India. The official page showed an application link when reviewed. Future availability is not guaranteed.
- The final location/fit checks retained one recommendation from the 233 combined records. Other locations and unsuitable roles were excluded rather than padded into the shortlist.
- An application/referral preparation packet was generated for that real role without an LLM. No resume facts, employment declarations or availability fields were submitted externally.
- Synthetic examples remain explicitly labeled and use a separate demo database. Demo notifications are disabled.

## Not claimed or activated

- Discord has now been tested end-to-end with the authorized channel webhook: one real recommendation and one setup confirmation were delivered to the private CareerScout #job-alerts channel, with the first recommendation checked in the Discord UI. The credential was verified against the intended channel, saved locally with mode 0600, and never printed. Telegram remains tested with mocked requests only.
- A daily Codex heartbeat schedule is ACTIVE: CareerScout daily Discord job updates, automation ID careerscout-daily-discord-job-updates, at 09:00 Asia/Kolkata. No separate terminal worker or OS scheduler was started. The next scheduled check is 24 October 2026 at 09:00 IST. A future unattended scheduled run has not yet occurred; first-run evidence will arrive after its scheduled time.
- No Ollama installation, model download, inference test, hardware-capacity check or paid model call was performed. The optional local-model adapter has schema/behavior checks with mocked responses.
- No LinkedIn/Naukri/Internshala account integration, alert-email ingestion, CAPTCHA workaround or application-submission automation is implemented.
- The two named contact records are possible public professional contacts. Their current team, India internship scope, availability and willingness to refer are unconfirmed. Named-contact refresh is not automated.
- No external application, employee message, connection request, assessment or interview booking was sent. Only the requested private Discord updates were posted.
- No acceptance-rate model was validated. The 0-100 number is a transparent fit heuristic.
- The public GitHub repositories were inspected as text/metadata; their source code, claimed test results, employment descriptions and quantitative impact were not independently verified by execution.

## Operational limits

This is a single-user macOS/Linux starter. Source coverage begins with three company feeds and one reviewed import; broader-market and requested-platform adapters need authorized sources. Common eligibility wording is parsed heuristically and unknown requirements remain review items. The reviewed profile and contacts need refresh as your information changes.

The local worker requires an awake, connected computer and a running process. Successful messages are recorded after provider acknowledgement. Ambiguous network timeouts can still cause a duplicate later. No end-to-end exactly-once delivery or unlimited provider access is claimed. The worker reports meaningful source failure/recovery state changes and remains quiet for unchanged matches.

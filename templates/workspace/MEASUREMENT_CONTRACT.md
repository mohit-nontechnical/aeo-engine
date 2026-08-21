# Measurement contract

Status: draft until the client approver signs the registry and the registry hash is recorded. Do not describe a draft or partially executed run as a baseline.

## Founding baseline scope

- One domain, one offer, one locale, and one language.
- The registry contains 40 questions. The 20 `core` questions form the bounded paid baseline panel; the 20 `expanded` questions are preserved for later monthly or diagnostic runs.
- Founding baseline execution covers consumer ChatGPT Search and Perplexity, one clean run per core prompt. Google AI features and Bing/Copilot are recorded only when first-party or reproducible access exists and are not bundled into the $750 time cap.
- Up to three named comparison domains.
- Maximum six to eight analyst hours. Anything beyond this is separately scoped.

## Freeze procedure

1. Client approves exact prompt text, tags, target pages, locale, competitor set, and engine panel.
2. Change every approved row from `draft` to `frozen` and fill `approved_at`.
3. Export the registry and competitor set as UTF-8 CSV with stable row ordering.
4. Record `sha256sum PROMPT_REGISTRY.csv COMPETITORS.csv` in the run log.
5. Never edit a frozen row. A text or tag change creates a new prompt ID and cohort version. Retire the old row without deleting it.

## Required denominators

Every rate must show its numerator and denominator. Invalid or unavailable observations are reported separately and are not scored as zero.

- Mention rate = valid answers naming the client / valid answers.
- Citation rate = valid answers citing at least one client URL / valid answers.
- Prompt citation coverage = distinct prompts with a client citation / distinct prompts with valid answers.
- Competitive citation share = client citations / citations to all domains in the frozen competitor set plus client.
- Entity accuracy = correct tested brand facts / tested brand facts in valid branded answers.

Do not report a trend unless at least 90% of the same prompt-engine cells are valid and comparable in both periods. Report raw counts, rates, and the range across repeats when repeats are commissioned. Never claim that content caused a movement from this observational panel alone.

## Technical cohort

The technical baseline is a fixed URL cohort, not a changing whole-site score. Include the homepage, offer pages, target pages for the core prompt panel, sitemap, robots file, and `llms.txt` if present. Record status code, canonical, index eligibility, title, robots directives, last modified evidence, structured data validity, and internal link count. Unavailable GSC or Bing access is marked `unavailable`, not zero.

## Evidence retention

- Store only business content needed for the engagement.
- Never place credentials, secrets, private customer data, or unapproved call transcripts in the evidence repository.
- Use least-privilege, client-specific access.
- Default retention is engagement term plus 90 days, then client-approved export or deletion. Contractual requirements override this default.
- The delivery owner records deletions and access changes.

## Roles

- Delivery owner: accountable for scope, client communication, access, and report release.
- Analyst: owns registry, runs, technical cohort, scoring, and analysis.
- Independent QA reviewer: re-scores the citation sample and checks evidence completeness.
- Client approver: freezes prompts and facts and approves any publication.
- Technical implementer and publisher: act only on approved change tickets; the orchestrator cannot approve its own work.

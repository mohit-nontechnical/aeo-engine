---
name: measure
description: Measure a client's answer-engine visibility with a frozen prompt panel — freeze the registry, run logged observations in ChatGPT Search/Perplexity, score citations, and produce the monthly report. Use whenever the user asks "are we showing up in ChatGPT/Perplexity/AI search", "run the baseline", "run the panel", "monthly AEO report", "measure answer visibility", or wants before/after evidence for AEO work. Requires a client workspace with a prompt registry.
---

# Answer Visibility Measurement

Produce evidence a skeptical client can trust. The entire value of this skill is **discipline**:
frozen prompts, archived evidence, honest denominators. The failure mode it exists to prevent is
the industry default — cherry-picked prompts making a dashboard improve while nothing real
changed. If the numbers can't survive the client's smartest engineer asking "how do you know?",
don't report them.

The workspace's `MEASUREMENT_CONTRACT.md`, `RUN_PROTOCOL.md`, `CITATION_RUBRIC.md`,
`ATTRIBUTION_RULES.md`, and `EVIDENCE_SCHEMA.json` are the full specification. This skill is the
operating summary; read the relevant spec file before each phase.

## Phase 1 — Freeze the panel

1. Curate `PROMPT_REGISTRY.csv`: real buyer questions (from intake + topics), tagged core vs
   expanded. A bounded core panel (~20 questions) is a deliverable; 40 is a registry.
2. Client approves exact prompt text, competitor set, engines, locale. Then flip rows
   `draft` → `frozen`, fill `approved_at`, and record `sha256sum PROMPT_REGISTRY.csv
   COMPETITORS.csv` in the run log.
3. **Never edit a frozen row.** A wording change = new prompt ID + new cohort version; retire the
   old row without deleting it. This is what makes month-over-month comparison honest.

## Phase 2 — Run observations

Follow `RUN_PROTOCOL.md` exactly. The essentials:

- One **clean conversation per prompt** (no follow-ups, no personalization/memory), exact frozen
  text, in the *consumer product* (ChatGPT Search, Perplexity).
- Capture per observation: full answer text, full-page screenshot (prompt + answer + product
  label + citations visible), every cited URL exactly as shown, timestamp/locale/model label.
  Hash the evidence. Never overwrite evidence.
- Evidence path: `measurement/runs/<run_id>/<engine>/<prompt_id>/<observation_id>/` with
  `evidence.json` (validates against `EVIDENCE_SCHEMA.json`), `answer.txt`, `screenshot.png`.
- Invalid observations (rate limit, partial render, wrong locale) are marked invalid and reported
  separately — **never scored as zero**.
- **API output is not product evidence.** What an API returns ≠ what the consumer product
  displayed. Product-level claims need product-level (browser/manual, e.g. Playwright + operator)
  capture. Automation of consumer surfaces must respect each product's terms — when in doubt,
  capture manually; slower-but-defensible beats fast-but-inadmissible.
- Single observations are snapshots, not stable truths — when repeats are commissioned, separate
  them ≥24h and report the range.

## Phase 3 — Score and report

- **Metrics with visible denominators** (per `MEASUREMENT_CONTRACT.md`): mention rate, citation
  rate, prompt citation coverage, competitive citation share, entity accuracy — each as
  numerator/denominator, never bare percentages.
- **Citation quality** per `CITATION_RUBRIC.md`: score claim↔citation pairs 0/1/2; an independent
  reviewer re-scores ≥20% and the batch needs ≥85% agreement before release.
- **Trends** only when ≥90% of the same prompt-engine cells are valid in both periods.
- **Attribution** per `ATTRIBUTION_RULES.md`: report answer visibility, verified answer-engine
  traffic, and assisted pipeline as **three separate numbers**. Direct/unknown traffic is not AEO
  traffic. Never claim content *caused* a visibility movement from this observational panel.
- Fill `MONTHLY_REPORT.md` from the template; attach the prompt-level evidence log; store the
  report in `measurement/reports/`.

## Hard lines

No guarantees of rankings, citations, or placement — ever, to anyone. No retroactive registry
edits. No blending sourced and influenced pipeline. Mark missing access (GSC, Bing WMT)
`unavailable`, not zero. If a client pushes for friendlier numbers, that's a Mohit conversation,
not a scoring adjustment.

# Consumer answer run protocol

## Before the run

1. Confirm the prompt and competitor CSV hashes match the approved run ticket.
2. Record operator, date, timezone, locale, language, country, device, browser, consumer product, visible product/model label, login state, subscription tier, and personalization or memory state.
3. Start a new clean conversation for every prompt. Do not use follow-up prompts.
4. Do not run if the product, network, or locale differs from the ticket. Mark the cell invalid and explain why.

## Per observation

1. Paste the exact frozen prompt with no extra punctuation or instruction.
2. Wait until the answer and citations finish rendering.
3. Save the full answer as text or HTML where permitted.
4. Save a full-page screenshot that contains the prompt, answer, product label, and citations.
5. Record every cited URL exactly as shown, the client mention text, position/context, errors, and any refusal or search failure.
6. Hash the screenshot and saved answer. Never overwrite evidence.

## Validity

A valid observation has the exact prompt, expected product and locale, a completed answer, a timestamp, full answer evidence, and legible citations. Login failure, rate limit, product error, partial render, changed prompt, wrong locale, or missing evidence makes the observation invalid. Invalid observations are rerun once in the same measurement window; otherwise they remain unavailable.

## Repeats and timing

The $750 founding baseline uses one observation for each core prompt in ChatGPT Search and Perplexity. Repeats or additional products require an expanded scope. When repeats are commissioned, separate them by at least 24 hours and report the range rather than treating one answer as stable.

## Evidence path

```text
measurement/runs/<run_id>/<engine>/<prompt_id>/<observation_id>/
  evidence.json
  answer.txt
  screenshot.png
```

The evidence record must validate against `EVIDENCE_SCHEMA.json`.

---
name: lint
description: Run the critic gate on a content draft — grounding check against the client's claim ledger, voice/banned-phrase enforcement, AEO shape check, and link canonicalization. Use on every draft before client review, whenever the user says "lint this", "check this draft", "does this pass", "review this article against the rules", or when the generate skill produces a draft. Also use to re-check a draft after client edits.
---

# Lint — the Critic Gate

The enforcement layer. Docs are guidance; this gate is what actually stops off-voice or
fabricated content from shipping, regardless of who (or what) wrote the draft. Run it on every
draft, every revision, and every client-edited version. The output is a **pass/reject verdict
with itemized reasons** — never silently fix a grounding violation, because the fix usually
requires a judgment call (drop the claim? add a ledger row? relabel as illustrative?) that
belongs to a human.

## Checks, in order

**1. Grounding (rejects, not warnings):**
- Extract every material claim: numbers, statistics, named results, customer references, quotes,
  comparisons, capability assertions, promises.
- Map each to `CLAIM_LEDGER.csv`. A claim with no allowed row (`verified`/`opinion`/
  `illustrative` + `review_state=supported` + public permission) and no named primary source that
  you can open **rejects the draft**.
- Client-results language with zero approved customer evidence ("we helped a client…", implied
  case studies, testimonials) rejects the draft.
- Unattributed statistics reject. A stat attributed to a source you cannot open rejects.
- Illustrative examples must be visibly labeled wherever they appear.
- Quotes must be verbatim ledger/customer-evidence entries with permission.

**2. Voice (deterministic — grep, don't vibe):**
- Run every deterministic rule in `VOICE.md` as a literal search: banned phrases, punctuation
  rules (e.g. em/en-dashes if banned), terminology mappings, competitor-naming policy.
- Default AI-tell blacklist (applies unless VOICE.md overrides): "in today's fast-paced world",
  "unlock", "elevate", "seamless", "game-changer", "delve", "landscape" (metaphorical),
  "supercharge", "revolutionize", "navigate the". These make content smell machine-written,
  which is fatal for a service whose value is *not* reading like AI content.

**3. AEO shape:**
- Direct answer to the primary query within the first 100 words.
- 3–5 self-contained FAQ pairs in frontmatter; **no markdown syntax inside frontmatter values**
  (it renders literally in JSON-LD).
- Clean heading hierarchy (one h1, h2/h3 structure), 1,200–1,800 words unless the registry says
  otherwise.
- Frontmatter matches the site's schema exactly (compare against a live article).

**4. Links:**
- Internal links exist in the topic registry's link plan and use the site's **canonical URL form**
  (check trailing-slash convention against a live page's `<link rel="canonical">`).
- External links: only to sources actually cited for claims, and each must resolve (spot-check
  with curl/WebFetch — a 404 citation is worse than none).

**5. Boundaries:** nothing from BRAND.md's confidential topics, no required disclaimer missing,
no channel the content isn't approved for.

## Verdict format

```markdown
# Lint: <slug> — PASS | REJECT
## Grounding: <pass / N violations>
- [claim text] → no ledger row / row CLM-xxx is needs_source / ...  → suggested resolution
## Voice: <pass / N hits>  (rule → line)
## AEO shape: <pass / issues>
## Links: <pass / issues>
## Boundaries: <pass / issues>
```

A REJECT goes back to the writer with reasons; fixes get a full re-lint (fixes introduce new
violations often enough that partial re-checks aren't safe). Log systemic patterns (same violation
across multiple drafts) in LOOP.md — recurring violations mean a VOICE.md rule or registry
grounding-risk flag needs sharpening, which is how the gate gets cheaper over time.

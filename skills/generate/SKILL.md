---
name: generate
description: Draft AEO-optimized articles for a client from their topic registry — grounded in the claim ledger, written in the client's voice, answer-shaped, with FAQ schema frontmatter. Use whenever the user asks to write articles, blog posts, or content for a client, "run a tranche", "draft the next batch", "generate content", or names topics/slugs from a client's topic registry. Requires a client workspace with a completed intake and topic registry.
---

# Content Generation

Draft articles from the client's topic registry, in tranches. The core contract: **every draft is
grounded, on-voice, and answer-shaped — and none of that depends on the writer's memory, because
the gates check it.** You are writing content designed to be *cited by answer engines*, which
means it must contain things worth citing: the client's defensible POVs, precise answers, and
claims that survive checking. Generic filler is not just low-quality here; it's invisible.

## Before writing (mandatory reads, every session)

1. `VOICE.md` — both judgment and deterministic rules.
2. `BRAND.md` — POVs, boundaries, competitor-naming policy.
3. `CLAIM_LEDGER.csv` + `CLAIM_LEDGER_RULES.md` — what may be asserted publicly.
4. The topic's registry entry — primary query, AEO note, grounding risk, link targets.
5. `LOOP.md` — pipeline state, lessons from prior tranches.
6. One or two files from `EXAMPLES/` — calibrate to the client's real voice before each tranche,
   not from memory of it.

## Per-article rules

**Answer shape (AEO):**
- The direct answer from the registry's AEO note appears **in the first 100 words**, before any
  narrative. Answer engines quote openings; make the opening quotable.
- Answer-shaped h2s where natural (the questions people actually ask, as headings).
- 3–5 FAQ pairs in frontmatter (from the topic's secondary queries; answers 2–4 sentences,
  self-contained — they render as FAQPage schema). **No markdown syntax inside frontmatter
  values** — it renders literally in JSON-LD.
- 1,200–1,800 words. Quality over padding; a complete 1,200 beats a padded 1,800.
- Internal links exactly as the registry specifies, in the site's **canonical URL form**
  (trailing slash or not — check; this has silently broken indexing before).

**Grounding (violations get the article rejected, not edited):**
- Every material claim maps to a claim-ledger row (`verified`/`opinion`/`illustrative`,
  `review_state=supported`) or a named, linkable primary source you actually opened. No row, no
  claim — drop the number, make the structural point, or reframe as "vendors claim X, verify it".
- Never repeat a statistic as fact because other articles in the niche cite it. Content-farm
  niches are full of fabricated vendor stats that launder themselves through repetition.
- Worked examples use clearly labeled illustrative numbers, never presented as real client data.
- Opinions belong to their named owner; ghostwriting may sharpen a POV, never invent one.
- Follow the topic's grounding-risk flag — it names the most likely fabrication for that topic.

**Voice:** the deterministic rules in VOICE.md are hard rules (banned phrases, punctuation,
terminology). The judgment rules shape everything else. When in doubt, reread an approved example.

## Tranche workflow

1. Pick the next `queued` topics from the registry (tranche size per LOOP.md — respect any
   indexing-backlog hold).
2. Draft each article to the site's content directory (or `drafts/` if no site repo), copying the
   frontmatter schema of an existing live article exactly.
3. **Run the `lint` skill on every draft.** Fix and re-lint until clean. A draft that hasn't
   passed lint doesn't exist.
4. Update registry statuses to `drafted` and log the tranche in LOOP.md (count, slugs, lessons,
   anything the critic caught — the lessons log is how the pipeline gets better per tranche).
5. Client review → approval is **explicit and version-specific** before anything publishes. A
   client edit that adds or changes a claim sends the draft back to grounding review — approval
   does not make an unsupported claim true.
6. After deploy (site-specific; see the workspace CLAUDE.md): ping IndexNow if wired
   (Bing/ChatGPT lane — not Google), and note the deploy in LOOP.md.

Nothing in this skill auto-publishes. Drafting is cheap; un-publishing fabrication is not.

---
name: topics
description: Build or extend a client's topic registry — researched buyer questions organized into hub-and-spoke clusters with per-topic briefs, AEO notes, and grounding-risk flags. Use whenever the user asks what content to write, wants keyword/topic research, says "build the topic registry", "plan the next content batch", "what should [client] write about", or when a client workspace has an intake but no topic registry yet.
---

# Topic Research

Produce the client's **topic registry**: the researched, prioritized list of articles the
`generate` skill will draft from. Every topic must trace to a real buyer question — the registry
is where "what buyers actually ask" becomes "what we write", and it's also the seed for the
measurement prompt panel. Generic keyword lists produce generic content that answer engines have
no reason to cite; grounded buyer questions produce content only this client can write.

## Inputs

Read first, always: `BRAND.md`, `CLIENT_INTAKE.md` (buyer questions + POVs), `COMPETITORS.csv`,
and `LOOP.md` (what already shipped — never duplicate a live topic).

## Research method

1. **Start from intake buyer questions.** Each recorded question, objection, and comparison from
   sales calls becomes a candidate topic in the buyer's own wording.
2. **Expand empirically, not by brainstorm.** For each candidate, check real SERPs and answer
   engines: WebSearch the question, note what ranks, run it through an answer engine and note what
   gets cited. Record People-Also-Ask variants and long-tail phrasings as secondary queries.
3. **Competitor gap pass.** Crawl the domains in `COMPETITORS.csv`: what do they answer, what do
   they answer badly, what can this client answer with a defensible POV that competitors can't?
   Differentiation angle > volume.
4. **Honesty filter.** Kill any topic the client has no standing to answer (no POV, no evidence,
   no ledger support). The source-sufficiency gate applies at the topic level: a topic without a
   named buyer question and a reason-this-client-is-qualified doesn't enter the registry.

## Registry format

Write `docs/topic-registry.md` in the client workspace. Organize as clusters, each with one hub
and its spokes (internal-linking structure is decided here, not at write time):

```markdown
## Cluster: <name> (hub: <hub-slug>)
### <NN>. <slug> — <working title>
- Primary query: <exact phrasing>
- Secondary queries: <2-4 variants / PAA>
- Persona / stage: <who, where in the funnel>
- AEO note: <the direct answer that must appear in the first 100 words>
- POV to use: <which BRAND.md position, if any>
- Grounding risk: <what a writer might fabricate here — e.g. "niche is full of fake vendor
  stats; no unverifiable numbers" — or "low">
- Links to: <hub + 2-3 sibling slugs>
- Status: queued | drafted | live
```

The **AEO note** and **grounding risk** fields are the two that earn their keep: the first makes
articles answer-shaped, the second pre-empts the most likely fabrication per topic before a
writer touches it.

## Sizing and cadence

Registry batches of 20–50 topics are plenty; a 300-topic registry is built in tranches as earlier
tranches prove out. Flag to the operator that **publishing rate should track indexing rate** — if
GSC shows a growing not-indexed backlog, pause generation rather than piling on (bulk-publishing
hundreds of pages on a young domain just grows the backlog). Note the recommended next tranche
size in LOOP.md.

## Feed the measurement panel

Propose the best 20–40 buyer questions as draft rows for `PROMPT_REGISTRY.csv` (status `draft` —
the `measure` skill owns freezing them). Topic research and measurement share a source of truth:
the questions buyers ask.

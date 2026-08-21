# Operator Runbook

You're running an AEO content service for a client using Claude Code and this plugin. You don't
need to be an SEO expert — the skills carry the method and the gates catch mistakes. Your job is
to run the sequence, keep the client informed, and escalate when something feels off.

## One-time setup

1. Install Claude Code and this plugin (see README).
2. Get the client folder from Mohit, or create it yourself with the intake skill (step 1 below).
3. Always work with Claude Code opened **inside the client's workspace folder** — that's how it
   picks up the client's voice and rules.

## New client (once)

| Step | What you run | What you get |
|---|---|---|
| 1 | `/aeo-engine:intake` | The interview. Do this as a call or async doc with the client. Output: their workspace |
| 2 | `/aeo-engine:tech-audit` | Site health report with a fix list to send to their web person |
| 3 | `/aeo-engine:measure` (baseline) | "Where you show up in ChatGPT/Perplexity today" — the before picture |
| 4 | `/aeo-engine:topics` | The list of articles worth writing, with reasons |

Don't skip or reorder. Content written before intake is done will be generic, and the lint gate
will bounce it anyway.

## Weekly rhythm (per client)

1. `/aeo-engine:generate` — draft the next tranche from the topic registry.
2. Claude runs `/aeo-engine:lint` on every draft automatically; if it doesn't, run it yourself.
   **A draft that hasn't passed lint never goes to the client.**
3. Send passing drafts to the client for approval. Approval must be explicit ("approved, publish
   these three") — silence is not approval.
4. Publish only approved drafts, however that client's site publishes (workspace CLAUDE.md has
   the specifics per client).
5. Note what shipped in the workspace's `LOOP.md` (Claude does this — just check it happened).

## Monthly

- `/aeo-engine:measure` — run the panel, produce the report, send it with the evidence attached.
- `/aeo-engine:topics` — top up the registry if it's running low.

## Escalate to Mohit (don't improvise) when:

- The client asks you to add a stat, testimonial, case study, or claim that isn't in their claim
  ledger. (The answer is "we need a source and sign-off first," but Mohit handles the pushback.)
- The client wants guaranteed rankings/citations/traffic. We never promise those.
- Anything legal, pricing, scope changes, or an unhappy client.
- The lint gate keeps rejecting for the same reason and you're not sure why.
- You're tempted to edit `PROMPT_REGISTRY.csv` after it's frozen. Don't — ask.

## Things that will get you (learned the hard way)

- **Don't hammer a client's live site** with repeated full-URL sweeps or polling — hosting
  firewalls will start serving 403s to real visitors. Sample ~10 URLs, spaced.
- **"Not ranking yet" on a young site is normal**, not an emergency. Indexing takes weeks.
  Don't let a client panic you into "fixing" content that just needs time.
- **Screenshots or it didn't happen.** Any "we showed up in ChatGPT!" claim needs the archived
  evidence from the measure skill, not a lucky anecdote.
- **Never paste client credentials into chat.** Access requests go through the client's own
  tools (GSC/Bing invites to your email).

---
name: tech-audit
description: Run a technical AEO/SEO eligibility audit on a client's website — canonicals, internal links, sitemap lastmod, schema, llms.txt, redirects, robots, index status. Use whenever the user asks to audit a site, check why pages aren't indexed, verify AEO readiness, run a "tech audit", diagnose "Discovered - currently not indexed", or before/after any content push for a client. Also use when a new client workspace exists but has no baseline audit yet.
---

# Technical AEO Audit

Answer engines can only cite pages they can crawl, index, and understand. This audit checks a
fixed defect list learned from real production incidents — every item here has actually broken
indexing or measurement on a live site. Work from the client workspace's `TECHNICAL_COHORT.csv`
(a fixed URL cohort: homepage, offer pages, target pages, sitemap, robots, llms.txt), not a
changing whole-site score, so audits are comparable over time.

## How to probe

Use `curl -sI` / WebFetch per URL and read the actual HTML. Record evidence (status code,
canonical, directives, retrieved_at) into `TECHNICAL_COHORT.csv` — a finding without evidence
is an opinion. Mark anything you can't access (GSC, Bing WMT) as `unavailable`, never zero.

**Politeness gotcha (learned the hard way):** never sweep every URL of a site repeatedly or poll
a live domain in a loop. Vercel and similar hosts have automatic bot mitigation that will serve
403s to real visitors for ~15 minutes after a hammering. Sample ~10 URLs, spaced out. Also:
spoofing a Googlebot user-agent from a residential IP gets challenged **by design** (rDNS
verification) — slow or blocked responses to a fake Googlebot UA are not evidence of a real
crawl problem. The only valid free crawl test is GSC → URL Inspection → Test Live URL.

## The defect checklist

Work through these in leverage order:

1. **Canonical ↔ internal-link mismatch (the big one).** Determine the canonical URL form
   (trailing slash or not) from `<link rel="canonical">`, then grep rendered pages for internal
   hrefs in the *other* form. If both forms return 200 with no redirect, the canonical URL set can
   end up with **zero internal inbound links** — Google discovers pages from the sitemap but never
   crawls them ("Discovered – currently not indexed" at scale). Fix: make every internal link emit
   the canonical form AND enforce one form with a 308 redirect at the host level. Test redirect
   config on a throwaway/preview deploy first — a redirect loop kills the site for crawlers.
2. **Sitemap hygiene.** Every URL needs a real `<lastmod>` (from content dates, not build time).
   No `noindex` pages listed in the sitemap. Sitemap URLs byte-match the canonical form.
3. **Redirect surface.** All four of `http/https × www/apex` must resolve to one canonical origin
   with 308/301s. Check with curl, not assumption.
4. **Structured data.** Articles: `Article` + `FAQPage` (real Q&A pairs) + `Person`. Site-wide: a
   standalone `Organization` JSON-LD somewhere (not only nested inside articles). FAQPage schema is
   real AEO leverage. Check that frontmatter-driven schema contains no raw markdown (links render
   literally in JSON-LD).
5. **llms.txt.** Present, current, and containing an anti-fabrication guard (what the company is,
   what may NOT be attributed to it). Treat llms.txt as an experimental disclosure surface, not a
   proven ranking factor — say so in the report.
6. **Flat/static pages.** Pages that bypass the site framework (raw HTML in `public/`) often ship
   with **no canonical tag at all** and stale og:url. Check them individually.
7. **Robots + crawler access.** robots.txt sane; GPTBot/PerplexityBot/bingbot not blocked (unless
   the client wants them blocked — ask, don't assume).
8. **Index reality check.** If GSC access exists, read the Pages report buckets and name the
   dominant bucket. Calibrate expectations honestly: a young domain with a bulk content drop takes
   2–6+ weeks to index and "absent from page 1" at week one is *expected, not failure*. Say so
   explicitly — panicked "fixes" to content that just needs time are a classic own-goal.
9. **IndexNow.** Wire it if possible, but report it accurately: IndexNow feeds Bing/ChatGPT,
   **not Google**. Don't count it as a Google indexing lever.
10. **Outbound citations.** Zero authoritative outbound links hurts AEO — being a citing hub
    correlates with being cited. Note it if the site links to nothing but itself.

## Report format

Write `audits/<date>-tech-audit.md` in the client workspace:

```markdown
# Technical AEO Audit — <client> — <date>
## Verdict (one paragraph, plain language)
## Defects found (leverage order: defect → evidence → exact fix → owner)
## Ruled out with evidence (so nobody re-audits them)
## Unavailable (GSC/Bing access gaps — access requests, not zeros)
## Re-check date
```

The "ruled out" section matters as much as the findings: it stops the next operator from
re-litigating settled questions. Every fix should be a change ticket someone can execute without
re-deriving the analysis. Log the audit in LOOP.md.

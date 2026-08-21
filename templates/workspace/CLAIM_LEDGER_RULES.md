# Claim Ledger Rules

The claim ledger is the factual control surface for canonical assets and every derivative. A polished draft cannot override it.

## One claim per row

Record one externally checkable assertion per row. Split compound sentences when the parts rely on different evidence.

Examples:

- Bad: `The platform is faster, cheaper, and used by 200 customers.`
- Better: three rows, each with its own source and permission.

## Allowed status values

- `verified`: supported by an allowed source and represented accurately.
- `opinion`: a named person's defensible judgment, not presented as objective fact.
- `illustrative`: a hypothetical example visibly labeled as illustrative wherever used.
- `confidential`: known internally but blocked from public use.
- `do_not_use`: rejected, disproven, unapproved, misleading, or otherwise prohibited.

`status` describes the claim class. `review_state` describes the workflow:

- `unreviewed`
- `supported`
- `needs_source`
- `conflicting`
- `stale`
- `blocked`

Only `verified`, `opinion`, or `illustrative` rows with `review_state=supported` and appropriate public permission may appear in public drafts.

## Source rules

Use these source types:

- `client_primary`: approved product docs, policies, contracts, first-party data, or records.
- `external_primary`: law, standard, official documentation, research paper, public filing, or named original study.
- `interview`: direct statement from a named subject-matter expert.
- `customer_evidence`: approved testimonial, result, case material, or quotation.
- `secondary_context`: third-party summary used for orientation, not as sole proof of a material claim.

Do not use another vendor's blog, generic SEO article, or model output as proof of a material claim. Follow it to the original source or drop the claim.

## Numbers and calculations

Every number needs one of:

- a named source and date;
- a reproducible calculation method and inputs;
- visible `illustrative` labeling.

Record the formula or source excerpt in `source_excerpt_or_method`. Set `recheck_date` for prices, product capabilities, market figures, laws, and other facts that can change.

## Opinions

An opinion row requires:

- the named owner;
- the exact position approved for public use;
- evidence or experience that makes the person qualified to hold it;
- the channels where it may appear.

Ghostwriting may clarify an opinion. It may not create one for the named expert.

## Quotations and customer evidence

- Keep quotations verbatim.
- Record the speaker, source, date, and public-use permission.
- Do not infer speaker identity from an ambiguous transcript.
- Do not reuse a private call quote because the call happened.
- Results and testimonials require explicit public approval for the same wording and context used.

## Permissions

Use `public_permission` values:

- `yes`
- `no`
- `conditional`

For `conditional`, state the condition in `notes`. List every permitted destination in `allowed_channels`. Approval for a website does not automatically include social, email, ads, sales collateral, or third-party outreach.

## Canonical-to-derivative rule

Every derivative must list the canonical asset ID and the claim IDs it uses. A derivative cannot introduce a new fact, number, quote, example, comparison, or promise.

If a derivative needs a new claim:

1. create a new ledger row;
2. complete source and permission review;
3. update the canonical source if the claim is material;
4. re-run grounding QA.

## Client edits

Any client edit that adds or materially changes a claim, number, quotation, comparison, commitment, or date returns the asset to grounding review. Approval does not make an unsupported claim true.

## Expiry and conflict

- Mark superseded or time-sensitive claims `stale` instead of silently deleting history.
- Mark conflicting sources `conflicting` and show the conflict to the fact owner.
- Never choose the more favorable source solely because it supports the desired narrative.

## Asset gate

A canonical asset passes grounding only when:

- every material claim maps to an allowed ledger row or named primary external source;
- no `needs_source`, `conflicting`, `stale`, or `blocked` claim remains in the draft;
- all quotes and customer proof have explicit permission;
- all illustrative examples are visibly labeled;
- the claim map matches the exact draft version reviewed.


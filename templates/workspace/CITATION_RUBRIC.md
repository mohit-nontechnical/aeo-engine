# Citation quality rubric

Score the relationship between one answer claim and one cited source, not the page or answer as a whole.

## Unit of review

Capture the exact answer claim span, cited URL, source passage, retrieval timestamp, score, rationale, and reviewer.

- **0, unsupported or wrong:** the source contradicts the claim, does not support it, is inaccessible, or the answer materially misrepresents it.
- **1, indirect or limited:** the source is topically relevant but supports only part of the claim, relies on an unclear method, is stale for a time-sensitive claim, or is secondary when a readily available primary source is required.
- **2, direct and accurate:** the source directly supports the material claim, the answer represents it accurately, and the source is current enough for the claim.

Record source type separately: `client-primary`, `external-primary`, `secondary`, `community`, or `unknown`. A source type does not automatically determine its score.

## QA calibration

- An independent reviewer re-scores at least 20% of claim-citation pairs, with a minimum of 10 pairs when available.
- Resolve disagreements before releasing the report.
- The rubric passes calibration at at least 85% exact agreement or Cohen's kappa of at least 0.70. If it fails, revise examples, re-train reviewers, and re-score the full affected sample.
- Report the sample size and agreement result.

## Required record fields

```text
citation_id,observation_id,prompt_id,engine,claim_span,cited_url,source_passage,retrieved_at,source_type,score,rationale,reviewer,qa_reviewer,qa_score,resolution
```

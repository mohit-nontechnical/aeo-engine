# Pipeline attribution rules

Keep three report sections separate: answer visibility, verified traffic and conversions, and assisted pipeline.

## Allowed evidence

- `verified_referral`: an analytics session exposes a recognized answer-engine referrer and reaches a first-party conversion event.
- `self_reported`: a form or discovery response explicitly names the answer engine or a specific client asset.
- `sales_confirmed`: dated CRM notes or a call transcript, with permission, explicitly name the answer engine or asset.
- `crm_first_touch` or `crm_last_touch`: the CRM stores a qualifying landing URL and source according to the client's documented attribution model.

## Disallowed inference

- Direct or unknown traffic is not AEO traffic by default.
- A citation without a visit is not a conversion.
- Branded-search growth is not automatically caused by answer visibility.
- An opportunity opened after publication is not influenced pipeline without one of the allowed evidence types.
- Do not combine sourced and influenced pipeline.

## Deduplication and windows

- Deduplicate people by the client's stable first-party contact or account ID, never by inferred identity.
- Record the client's existing attribution window before the baseline. If none exists, report evidence without assigning sourced revenue.
- Preserve raw evidence type, timestamp, landing URL, conversion ID, contact/account ID, opportunity ID, and reviewer.
- If sources conflict, report the conflict and use the more conservative category.

## Minimum report fields

```text
evidence_id,evidence_type,occurred_at,landing_url,referrer,conversion_id,contact_or_account_id,opportunity_id,asset_id,source_record_uri,reviewer,attribution_category,notes
```

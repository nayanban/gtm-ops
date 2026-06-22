# Privacy Redaction Checklist

Use this checklist before moving private operational work into a public portfolio repo.

## Remove

- Real customer, prospect, referral, employee, or attendee names.
- Record IDs, workflow IDs, account IDs, user IDs, and object IDs.
- OAuth, API, refresh-token, client-secret, keychain, or local credential details.
- Raw meeting notes, transcripts, call summaries, and CRM activity payloads.
- Private target-account lists, segmentation findings, territories, or query strings.
- Local paths, machine names, private repo names, and environment-specific commands.
- Vendor-specific endpoint mechanics, connection names, scopes, and UI setup details.

## Generalize

| Private Detail | Public Replacement |
| --- | --- |
| Real person or account | Synthetic person or example company. |
| Vendor-specific platform | Generic source system, CRM, outbound platform, or provider. |
| Live record identifier | Empty string, `example-id`, or omitted field. |
| Private count or batch result | Pattern-level statement or synthetic count. |
| Exact territory or query | Generic segment or account-universe filter. |
| Raw payload | Approved field list or schema shape. |

## Preserve

Keep the reusable judgment layer:

- routing decisions;
- approval boundaries;
- field write policy;
- verification expectations;
- cleanup criteria;
- review states;
- failure modes;
- rollback or resume behavior.

## Final Review

Before publishing, scan for:

- proper nouns that are not intentionally public;
- email addresses and domains;
- tokens, secrets, and credential words;
- real dates tied to identifiable events;
- IDs longer than a normal example value;
- comments or instructions written to internal tooling.

When in doubt, keep the private artifact private and publish only the generalized operating pattern.

# GTM Ops Automation Playbooks

Public-facing playbooks for building practical go-to-market and revenue-operations automation.

This repository is a sanitized portfolio/reference copy. It does not contain private client repositories, CRM exports, prospect records, meeting transcripts, credentials, implementation IDs, or private account strategy.

## What Is Included

- CRM enrichment architecture for keeping company and person data separate.
- Meeting-notes-to-CRM workflow design for reliable human-approved updates.
- Prospect data cleanup patterns for turning messy outbound lists into reviewable datasets.
- Signal-based prospect research architecture for converting public events into useful sales review queues.

## Design Principles

- Keep private systems private; publish only reusable patterns.
- Separate automation from judgment. Automation can prepare records, but humans should approve ambiguous sales or customer actions.
- Prefer source-backed notes over invented polish.
- Preserve auditability without exposing private records.
- Use small, reversible workflow steps before enabling unattended automation.

## Repository Map

```text
docs/
  crm-enrichment-architecture.md
  meeting-notes-to-crm-workflow.md
  prospect-data-cleanup-workflow.md
  signal-based-prospect-research.md
  publication-checklist.md
templates/
  approved-crm-write-input.example.json
  data-cleanup-review-columns.csv
```

## Not Included

This repository intentionally excludes:

- private CRM record IDs, workflow IDs, account IDs, or user IDs;
- OAuth, API, refresh-token, or local secret-handling details;
- raw meeting notes, transcripts, call summaries, or CRM activity payloads;
- customer, prospect, or referral names;
- private sales strategy, target-account lists, or segmentation findings;
- historical commits from private source repositories.

## License

MIT. See [LICENSE](LICENSE).

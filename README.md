# GTM Ops

Portfolio/reference playbooks for practical go-to-market and revenue-operations automation.

The playbooks focus on CRM enrichment, meeting-note workflows, outbound engagement routing, prospect data cleanup, account-research infrastructure, and signal-based research systems. The examples are synthetic and the patterns are designed to be adapted to different CRM and GTM stacks.

## What Is Included

- CRM enrichment architecture for keeping company and person data separate.
- Meeting-notes-to-CRM workflow design for reliable human-approved updates.
- Outbound engagement-to-CRM patterns for preserving provenance while enriching records.
- Prospect data cleanup patterns for turning messy outbound lists into reviewable datasets.
- Account-research infrastructure for building reviewed account universes before contact enrichment.
- Signal-based prospect research architecture for converting public events into useful sales review queues.

## Design Principles

- Keep private systems private; share only reusable patterns.
- Separate automation from judgment. Automation can prepare records, but humans should approve ambiguous sales or customer actions.
- Prefer source-backed notes over invented polish.
- Preserve auditability without exposing private records.
- Use small, reversible workflow steps before enabling unattended automation.

## Repository Map

```text
docs/
  selected-project-patterns.md
  crm-enrichment-architecture.md
  meeting-notes-to-crm-workflow.md
  outbound-engagement-to-crm.md
  prospect-data-cleanup-workflow.md
  account-research-infrastructure.md
  signal-based-prospect-research.md
templates/
  approved-crm-write-input.example.json
  data-cleanup-review-columns.csv
  crm-routing-proposal.example.json
  signal-queue-record.example.json
```

## Privacy

The repository intentionally excludes private implementation material:

- CRM record IDs, workflow IDs, account IDs, or user IDs;
- OAuth, API, refresh-token, or local credential-handling details;
- raw meeting notes, transcripts, call summaries, or CRM activity payloads;
- customer, prospect, or referral names;
- private sales strategy, target-account lists, or segmentation findings.

## Rights

Copyright (c) 2026 Nayan Banerjee. All rights reserved.

This repository is shared publicly for portfolio and reference purposes only. No license is granted for reuse, redistribution, sublicensing, or commercial use without written permission.

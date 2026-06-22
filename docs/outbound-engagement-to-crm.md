# Outbound Engagement To CRM

This playbook describes how to route outbound engagement events into CRM records while preserving provenance and avoiding noisy automation.

## Goal

When an outbound prospect engages with a campaign, the CRM should receive a useful record update without losing the source trail or bypassing existing enrichment rules.

## Pattern

```text
Outbound engagement event
        |
        v
CRM create/update record
        |
        v
Add source/provenance note
        |
        v
CRM workflow triggered by source note
        |
        v
Fetch person fields from source platform
        |
        v
Write approved nonblank person fields
        |
        v
Allow existing company enrichment workflow to run
```

## Why Use A Source Note

A source note can do two jobs:

1. Tell a human why the record exists or changed.
2. Give the CRM a reliable workflow marker.

This can be cleaner than adding a custom marker field only for automation. The note remains useful even if the automation is later disabled.

## Field Boundaries

Write person-level fields from the outbound platform:

- title;
- person profile URL;
- email, if sourced directly;
- campaign/source provenance in notes.

Do not write company enrichment fields from the outbound event itself unless they are directly sourced and verified. Let the existing company-enrichment path handle company profile data.

## Design Choices

| Decision | Reason |
| --- | --- |
| Use note-added workflow trigger | Keeps provenance visible and avoids relying on broad lead-source values. |
| Fetch the record after create/update | Some automation platforms do not expose the CRM record ID reliably in the first step. |
| Write only nonblank person fields | Avoids replacing existing CRM values with empty source values. |
| Let downstream enrichment run normally | Avoids duplicating enrichment logic in the outbound workflow. |

## Troubleshooting Checklist

- Did the source note get created?
- Does the note title or marker match the CRM workflow condition exactly?
- Is the workflow argument mapped to the current record ID, not a literal placeholder?
- Did the source API return the expected shape?
- Were the returned fields blank?
- Did downstream workflows run after create/update?

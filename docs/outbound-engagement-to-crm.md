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

## Implementation Sequence

1. Receive the outbound engagement event.
2. Create or update the CRM record using core person/company fields.
3. Fetch or confirm the resulting CRM record ID.
4. Attach a source/provenance note to the record.
5. Trigger a CRM workflow from that source/provenance marker.
6. Fetch person-level fields from the outbound platform.
7. Write only nonblank person-level fields.
8. Let existing company-enrichment workflows run through their normal path.

## Why Use A Source Note

A source note can do two jobs:

1. Tell a human why the record exists or changed.
2. Give the CRM a reliable workflow marker.

This can be cleaner than adding a custom marker field only for automation. The note remains useful even if the automation is later disabled.

## Source Note Contract

A useful source note should include:

- source system category;
- engagement type;
- campaign or sequence label, if public-safe;
- event time;
- source person identifier, if synthetic or non-sensitive;
- message or content reference, if available;
- reason the record was created or updated.

Avoid raw payload dumps. Keep the note readable for humans and stable enough for automation.

## Workflow Trigger Contract

The trigger should be narrow:

- marker type is exact;
- note title or metadata condition is exact;
- record ID is mapped from the current CRM record;
- the workflow should not fire on ordinary user notes;
- retries should not duplicate notes or overwrite fields with blanks.

## Field Boundaries

Write person-level fields from the outbound platform:

- title;
- person profile URL;
- email, if sourced directly;
- campaign/source provenance in notes.

Do not write company enrichment fields from the outbound event itself unless they are directly sourced and verified. Let the existing company-enrichment path handle company profile data.

## Field Update Rules

- Write only nonblank values.
- Do not overwrite higher-confidence human-entered fields without approval.
- Keep source provenance in notes, not in unrelated lead-source fields.
- Avoid broad marker fields when a source note can preserve the same evidence.
- Do not suppress downstream workflows unless the update is explicitly maintenance-only.

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

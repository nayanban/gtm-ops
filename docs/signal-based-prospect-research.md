# Signal-Based Prospect Research

This playbook describes an architecture for turning public events into a review queue for sales research.

## Goal

Identify potentially relevant organizations or people from public signals without pretending that every signal is outreach-ready.

## Architecture

Use a staged queue:

1. Search public event sources for candidate events.
2. Create a candidate record with source URL, event date, and signal type.
3. Extract the entity that may matter commercially.
4. Add source-backed notes.
5. Mark extraction status.
6. Review the candidate before outreach or CRM creation.

## Signal Statuses

| Status | Meaning |
| --- | --- |
| Pending | Candidate found; extraction or review not complete. |
| Entity found | A relevant organization or person was identified. |
| No entity found | Event did not produce a useful sales-review entity. |
| Extraction failed | Technical failure; needs retry or manual review. |
| Reviewed | Human reviewed and accepted, rejected, or deferred. |

## Notes Structure

Use separate notes for separate evidence:

```text
Event context
- Event name:
- Date:
- Source:
- Summary:

Entity identified
- Organization or person:
- Role in event:
- Evidence:
- Source URL:
- Confidence:
```

## What To Avoid

- Do not treat raw alerts as qualified leads.
- Do not create outreach records when the relevant entity is missing.
- Do not hide extraction failures.
- Do not enrich contacts or reveal private contact data unless the candidate set has been reviewed.
- Do not let automation decide commercial fit on its own.

## Human Review Questions

- Is this event actually relevant to the buyer profile?
- Is the identified entity actionable?
- Is the source evidence strong enough?
- Is this a new record or an update to an existing record?
- What follow-up, if any, is justified?

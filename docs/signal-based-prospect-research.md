# Signal-Based Prospect Research

This playbook describes an architecture for turning public events into a review queue for sales research.

## Goal

Identify potentially relevant organizations or people from public signals without pretending that every signal is outreach-ready.

This pattern is useful when public events create possible sales signals, but raw alerts are too noisy to use directly. The system should produce a review queue, not an outreach machine.

## Architecture

Use a staged queue:

1. Search public event sources for candidate events.
2. Create a candidate record with source URL, event date, and signal type.
3. Extract the entity that may matter commercially.
4. Add source-backed notes.
5. Mark extraction status.
6. Review the candidate before outreach or CRM creation.

## What Made The Signal Useful

The useful unit is not the raw event. It is the actionable entity surfaced by the event.

Public-safe lessons:

- Raw event alerts can be too noisy on their own.
- Parties, organizations, or summaries alone may not identify a useful next action.
- The useful unit is often an identified attorney, firm, buyer, vendor, or other actionable entity.
- Document or PDF extraction can outperform generic web search when source documents contain signature blocks, structured evidence, or official contact context.
- Deterministic enrichment from already-observed facts is safer than broad speculative enrichment.
- Unknown values should remain explicit instead of being hidden behind polished summaries.

## Extraction Boundary

Separate candidate creation from difficult extraction work.

| Layer | Owns |
| --- | --- |
| CRM or system of record | Schedule, candidate records, dedupe keys, status, and cleanup state. |
| External worker | Document retrieval, parsing, entity extraction, and evidence notes. |
| Human reviewer | Commercial judgment and next action. |

This keeps the system of record authoritative without forcing fragile parsing logic into a workflow engine that is poorly suited for it.

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

## Useful Dedupe Keys

Candidate signals should have deterministic external IDs. Depending on the source, a good dedupe key may combine:

- source system;
- event/document ID;
- event date;
- signal type;
- relevant entity, when known.

The dedupe key should prevent repeated alerts from creating duplicate candidate records while still allowing genuinely new events for the same organization to appear.

## Human Review Questions

- Is this event actually relevant to the buyer profile?
- Is the identified entity actionable?
- Is the source evidence strong enough?
- Is this a new record or an update to an existing record?
- What follow-up, if any, is justified?

# Meeting Notes To CRM Workflow

This playbook turns a meeting note into a proposed CRM update without creating duplicate records or burying useful context in the wrong field.

## Goal

Make CRM updates fast and reliable while keeping a human approval step before any write.

## Inputs

- Meeting title, date, attendees, and source reference.
- Meeting summary or private notes.
- Target person and company.
- Original lead source, when known.
- Any user-supplied corrections.

## Routing First

Before extracting fields, decide where the update belongs:

| Outcome | Use When |
| --- | --- |
| Create lead | The person is top-of-funnel and no matching record exists. |
| Update lead | A matching lead exists and the context belongs there. |
| Update contact/account/deal | The person is already in a customer or opportunity relationship. |
| No write | The note lacks enough grounded information. |
| Ask user | The target person, company, or relationship is ambiguous. |

## Duplicate Checks

Search narrowly before proposing a write:

1. Exact email match.
2. Person profile URL match.
3. Person name plus company.
4. Company/account name.
5. Opportunity or subscription context only when the note suggests it.

## Approved Write Shape

After approval, write only the approved content:

1. Create or update the lead/contact record.
2. Create a closed activity with full meeting intelligence.
3. Create one concise note for quick human review.
4. Create a task only when there is a concrete next action.
5. Verify the created or updated records.

## Field Policy

Good candidate fields:

- first name;
- last name;
- company;
- email;
- phone;
- title;
- person profile URL;
- lead source;
- lead status;
- company website, when confidently known.

Avoid by default:

- free-text description fields used by enrichment;
- currency or billing fields;
- annual revenue, employee count, or industry unless directly sourced and approved;
- system, audit, conversion, or ownership fields unless explicitly required.

## Note Quality

The quick note should be useful to a CRM user. It should not read like a technical audit log.

Include:

- business summary;
- next step, if captured;
- source reference.

Avoid:

- duplicate-check mechanics;
- target-selection reasoning;
- internal workflow reminders;
- unsupported facts added for polish.

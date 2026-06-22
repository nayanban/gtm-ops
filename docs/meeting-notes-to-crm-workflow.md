# Meeting Notes To CRM Workflow

This playbook turns a meeting note into a proposed CRM update without creating duplicate records or burying useful context in the wrong field.

## Goal

Make CRM updates fast and reliable while keeping a human approval step before any write.

This pattern is designed for high-context sales conversations where the raw note is not enough. The workflow separates routing logic, proposed CRM content, approval, write execution, and verification.

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

Related or referral people mentioned in a meeting should remain separate from the target record unless the user explicitly asks to create or update records for them. This avoids a subtle but damaging error: letting an interesting referral name replace the person the meeting was actually with.

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

The approval contract matters. Once the user approves a proposal, the write helper should treat the proposal as locked data, not as a draft to improve. Do not re-summarize, re-polish, enrich externally, or run unrelated rediscovery unless a write fails or a required value is missing.

## Idempotent Write Helper Pattern

For any CRM write helper, store state after each successful object write:

| Step | State To Store |
| --- | --- |
| Record create/update | Target record ID and mode. |
| Activity create | Activity ID. |
| Note create | Note ID. |
| Task create | Task ID, if any. |
| Verification | Readback result and timestamp. |

On rerun, skip anything that already has an ID. This makes the workflow recoverable if a later step fails after an earlier CRM write succeeded.

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

## Detailed Activity Quality

Use the activity or call description for rich interaction intelligence:

- meeting source and date;
- participants;
- pain points and use cases;
- buying signals;
- competitor or tool context;
- strategic insight;
- next step;
- material uncertainty.

Keep process logic out of the CRM activity. The CRM user needs the business context, not a log of how the workflow decided where to write.

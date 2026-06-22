# Operator Runbook

This runbook shows how the public playbooks fit together in an end-to-end, vendor-neutral workflow.

## Goal

Move from source context to a verified CRM update without exposing private systems, skipping human judgment, or losing provenance.

## Run Sequence

1. Review the source.
2. Decide the target person, company, and record surface.
3. Create a routing proposal.
4. Ask for human approval.
5. Convert the approved proposal into locked write input.
6. Execute writes in order.
7. Save state after each successful write.
8. Resume from state if interrupted.
9. Read back the results.
10. Record verification warnings.

## Step 1: Source Review

Use the strongest available source:

| Source | Role |
| --- | --- |
| Meeting metadata | Confirms date, participants, and title. |
| Summary or private notes | Provides business context. |
| Transcript or detailed note | Resolves ambiguity when available. |
| User correction | Overrides inferred or stale context. |
| Search result | Supporting evidence only, not primary write material. |

Do not write unsupported facts just because they make the record look more complete.

## Step 2: Routing Proposal

Create a proposal before writing anything.

Use `templates/crm-routing-proposal.example.json` as the shape for:

- source reference;
- routing outcome;
- duplicate checks;
- proposed fields;
- activity, note, and task decisions;
- uncertainties.

If routing is ambiguous, stop and ask. A clean pause is better than a confident write to the wrong record.

## Step 3: Human Approval

Approval locks the proposal.

After approval:

- do not rewrite the summary;
- do not add new enrichment;
- do not change routing unless the target is missing;
- do not add a task unless it was approved or clearly captured.

See [Human-Approved CRM Write Contract](human-approved-crm-write-contract.md).

## Step 4: Approved Input

Use `templates/approved-crm-write-input.example.json` to convert the approved proposal into write input.

The write input should include:

- target record mode;
- approved fields;
- activity payload;
- concise note payload;
- optional task payload;
- verification expectations.

## Step 5: Ordered Writes

Write in this order:

1. Target record.
2. Activity or call.
3. Concise note.
4. Task, if approved.
5. Verification report.

Do not create related objects until the target record is known.

## Step 6: Stateful Resume

Use `templates/crm-write-state.example.json` to avoid duplicate writes.

On rerun:

- skip existing record IDs;
- skip existing activity IDs;
- skip existing note IDs;
- skip existing task IDs;
- continue from the first missing object;
- verify everything at the end.

## Step 7: Verification Report

Use `templates/verification-report.example.json` to capture:

- what was created or updated;
- what was intentionally skipped;
- what was verified;
- warnings or uncertainty;
- next manual review, if needed.

Verification is not just a success message. It is the handoff record for the next operator.

## Public-Safe Handoff

Before publishing a runbook or example:

- replace names, IDs, dates, and source labels with synthetic values;
- remove vendor-specific endpoint, auth, and UI details;
- remove raw meeting notes and payloads;
- preserve the decision pattern, not the private facts.

See [Privacy Redaction Checklist](privacy-redaction-checklist.md).

# Enrichment Failure Modes

This checklist describes why enrichment may look configured but still fail operationally.

## Goal

Make enrichment behavior testable before it affects live records at scale.

## Common Failure Modes

| Failure Mode | Symptom | Public-Safe Check |
| --- | --- | --- |
| Configuration does not apply automatically | Mapping exists, but fields remain blank. | Create or update a synthetic test record and read it back. |
| Permission mismatch | Provider call succeeds in one place but writeback fails elsewhere. | Confirm the automation actor can read and write only approved fields. |
| Async enrichment pending | Record is checked before the provider has finished. | Store a pending status and retry with a bounded schedule. |
| Field type incompatibility | Data appears in logs but not in CRM fields. | Compare provider output type to the target field type. |
| Domain mismatch | Provider returns the wrong company. | Compare normalized source domain and returned domain before writing. |
| Blank overwrite | Existing values disappear after enrichment. | Write only nonblank values unless overwrite is explicitly approved. |
| Summary loss | A useful human-entered summary is replaced. | Preserve the old summary in a note before updating. |
| Downstream automation surprise | A field write triggers unrelated workflow behavior. | Audit workflows and protected surfaces before enabling. |
| Related-list invisibility | Created note or activity is not visible where expected. | Read back through the same surface a user would inspect. |
| Partial-run duplicate | A retry creates duplicate notes, tasks, or activities. | Save state after each successful object write. |

## Verification Checklist

Before enabling enrichment broadly, confirm:

- test records cover verified website, work-email domain, personal email, no result, and mismatched domain;
- field writes are limited to approved company-level fields;
- person-level fields remain protected;
- existing summaries are preserved;
- failed or skipped records are visible in output;
- readback confirms the values users will actually see;
- the final dry run explains remaining candidates.

## Decision Rule

If enrichment cannot be verified end to end, leave it disabled for that module and document the reason in a module decision record.

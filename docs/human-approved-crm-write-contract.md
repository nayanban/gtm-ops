# Human-Approved CRM Write Contract

This contract describes how to turn a reviewed proposal into safe CRM writes.

## Core Rule

Once a user approves a proposal, the approved content becomes locked input data.

Do not:

- re-summarize approved content;
- polish wording before writing;
- run unrelated discovery;
- add external enrichment;
- change routing unless the write fails or the approved target is missing.

## Write Modes

| Mode | Meaning |
| --- | --- |
| `create` | Create a new target record, then attach approved related objects. |
| `update` | Update approved fields on an existing record, then attach related objects. |
| `existing` | Do not update core fields; attach approved activity/note/task to the existing record. |

## Write Order

1. Target record create/update/existing selection.
2. Activity or call.
3. Concise note.
4. Task, only if approved.
5. Verification readback.

## State File

Store state after each successful write:

```json
{
  "source_reference": "synthetic-example",
  "record_id": "",
  "activity_id": "",
  "note_id": "",
  "task_id": "",
  "last_completed_step": "",
  "updated_at": ""
}
```

## Resume Behavior

On rerun:

- if `record_id` exists, do not create another record;
- if `activity_id` exists, do not create another activity;
- if `note_id` exists, do not create another note;
- if `task_id` exists, do not create another task;
- continue from the first missing approved step;
- finish with verification.

## Verification

Read back enough data to confirm:

- target record exists;
- approved fields are present;
- activity is attached to the right record;
- note is attached to the right record;
- task exists if approved;
- text encoding is intact;
- skipped objects were intentionally skipped.

Report both success and uncertainty. A created ID without related-list visibility should be treated as a warning, not silently ignored.

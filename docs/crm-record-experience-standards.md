# CRM Record Experience Standards

This playbook describes where different kinds of CRM context should live so records stay readable and automations stay predictable.

## Surface Roles

| Surface | Use For | Avoid |
| --- | --- | --- |
| Structured fields | Stable facts, routing, ownership, source, status, identifiers. | Long narrative, uncertain context, raw transcripts. |
| Notes | Concise human-readable context, provenance, source references, conflict notes. | Full meeting intelligence when an activity exists. |
| Activities/calls | Detailed interaction history and business context. | Duplicate-check logs or process chatter. |
| Tasks | Concrete future actions. | Vague reminders with no owner or next step. |
| Description/summary | Enrichment summary or profile, if that is the agreed policy. | Living conversation history from meetings. |

## Naming And Display

Keep data clean even when UI cards are imperfect:

- first name belongs in first-name fields;
- last name belongs in last-name fields;
- do not stuff full names into last-name fields for display;
- solve card/display issues in layout, formula, or view configuration;
- preserve accented characters and punctuation unless a normalization rule requires otherwise.

## Notes Versus Activities

Use both when they serve different jobs:

- activity/call: detailed meeting intelligence;
- note: concise summary and source reference.

This lets a CRM user scan the record quickly while preserving the full interaction context for deeper review.

## Task Creation Standard

Create a task only when there is a concrete next action.

Good task:

```text
Send security documentation requested in discovery call
```

Weak task:

```text
Follow up
```

If the next step is unknown, say no specific next step was captured rather than creating a vague task.

# CRM Enrichment Architecture

This playbook describes a safe pattern for enriching CRM records while keeping company-level and person-level data separate.

## Goal

Improve record quality without overwriting human-entered relationship context or mixing organization data into person fields.

This pattern came from a CRM rebuild where enrichment had to improve record completeness without disturbing customer-success, billing, subscription, or account-management workflows. The central lesson was that enrichment is not one feature. It is a set of module-specific decisions about identity, provenance, overwrite behavior, and downstream automation.

## Core Model

Use separate surfaces for separate jobs:

| Surface | Purpose |
| --- | --- |
| Lead or Contact fields | Stable person and company identifiers. |
| Company enrichment fields | Website, company profile, employee range, revenue range, industry, locations, and company social links. |
| Person enrichment fields | Title, person LinkedIn URL, email, phone, role, and seniority. |
| Notes | Source references, conflicts, provenance, and human-readable context. |
| Activities or calls | Interaction history and conversation intelligence. |

## Workflow Pattern

1. Use website or work-email domain as the primary company enrichment trigger.
2. Ignore personal email domains for company lookup.
3. Validate that the returned company domain matches the source domain before writing enrichment output.
4. Write company results only into company-level fields.
5. Preserve any existing free-text description before replacing it with an enrichment summary.
6. Do not overwrite person-specific fields with organization data.
7. Keep billing, subscription, customer-success, and account-management automations outside the enrichment change unless they are explicitly in scope.

## Implementation Lessons

The implementation story matters because enrichment can look simple in a settings screen while still failing operationally.

Useful lessons:

- Native enrichment configuration may define mappings without reliably applying values at the right moment.
- Company-name-only matching can produce false positives; a trusted website or domain is a better confidence gate.
- Enrichment output should be validated against the source identity before writeback.
- A workflow/function layer can bridge the gap between provider output and safe CRM updates.
- Backfills need stricter controls than ordinary single-record updates because a bad rule can affect many records at once.
- Person fields and company fields should be protected by code and policy, not just by reviewer memory.

Related patterns:

- [CRM Enrichment Implementation Pattern](crm-enrichment-implementation-pattern.md)
- [CRM Backfill Safety Pattern](crm-backfill-safety-pattern.md)
- [CRM Record Experience Standards](crm-record-experience-standards.md)

## Module Boundary Pattern

Treat CRM modules as different operational surfaces, not interchangeable tables.

| Module Type | Public Pattern |
| --- | --- |
| Top-of-funnel people | Start here when someone is not yet tied to a customer/account relationship. |
| Post-lead people | Keep only people with a meaningful relationship to an account, opportunity, subscription, or customer context. |
| Legacy people | Move stale/orphan records into a reviewable holding area instead of deleting them blindly. |
| Accounts | Own company-level enrichment and relationship context. |
| Opportunities/subscriptions | Treat as protected commercial/customer-success surfaces; audit before changing. |

This prevents a common CRM failure mode: pushing every person record through the same enrichment logic and then discovering that top-of-funnel, customer, and legacy data have been mixed together.

## Cleanup Before Enrichment

Before enabling enrichment on a person module, run a source and automation audit:

- active workflows;
- assignment rules;
- validation rules;
- blueprints;
- functions or automations attached to the module;
- native record-creation paths;
- protected downstream processes.

If the module contains stale standalone people, split them into a legacy/review area before enabling enrichment. Preserve provenance, original IDs where appropriate, ownership context, and raw values that may not pass current field validation.

## Why Some Enrichment Should Not Be Enabled

Useful architecture includes saying no.

Do not enable a native or third-party enrichment path when:

- the provider returns company fields for a person record;
- blank-only overwrite controls are unclear;
- current-company data could incorrectly change account relationships;
- marketplace permissions have not been reviewed;
- the output fields do not map cleanly to the CRM data model.

In those cases, document the decision, keep the module clean, and park enrichment until a controlled person-level path exists.

## Guardrails

- Do not assume one module's enrichment setup applies to every CRM module.
- Do not use a company enrichment provider as a person enrichment provider.
- Do not change linked account relationships automatically based on enrichment results.
- Do not treat enrichment output as ground truth when source identity is ambiguous.
- Do not write unsupported fields just because the enrichment provider returned data.

## Review Checklist

- Which module is in scope?
- Which fields can be safely written?
- Which fields must never be overwritten?
- How will old values be preserved?
- How will false matches be detected?
- What is the rollback path if enrichment behaves badly?
- Which downstream automations could be affected?
- Is this enrichment path company-level, person-level, or a mix that needs to be rejected?
- Does the module need cleanup before enrichment is useful?

# CRM Enrichment Architecture

This playbook describes a safe pattern for enriching CRM records while keeping company-level and person-level data separate.

## Goal

Improve record quality without overwriting human-entered relationship context or mixing organization data into person fields.

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

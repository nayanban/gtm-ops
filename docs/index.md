# Docs Index

This index groups the playbooks thematically while keeping the repository structure flat and easy to scan.

## Start Here

- [Selected Project Patterns](selected-project-patterns.md): portfolio-level overview of the work represented here.
- [Operator Runbook](operator-runbook.md): end-to-end flow from source review to verified CRM update.
- [Privacy Redaction Checklist](privacy-redaction-checklist.md): publication checklist for keeping examples public-safe.

## CRM Enrichment And Data Quality

- [CRM Enrichment Architecture](crm-enrichment-architecture.md): module boundaries, company/person separation, and enrichment guardrails.
- [CRM Enrichment Implementation Pattern](crm-enrichment-implementation-pattern.md): vendor-neutral enrichment control flow.
- [CRM Backfill Safety Pattern](crm-backfill-safety-pattern.md): dry-run, batch, and verification pattern for broad updates.
- [CRM Record Experience Standards](crm-record-experience-standards.md): where fields, notes, activities, summaries, and tasks belong.
- [Enrichment Failure Modes](enrichment-failure-modes.md): operational checks for enrichment that looks configured but fails in practice.

Related templates:

- [Module Enrichment Decision](../templates/module-enrichment-decision.example.md)
- [CRM Module Audit Checklist](../templates/crm-module-audit-checklist.example.md)
- [CRM Field Policy Matrix](../templates/crm-field-policy-matrix.example.csv)

## CRM Write And Engagement Workflows

- [Meeting Notes To CRM Workflow](meeting-notes-to-crm-workflow.md): proposal-first CRM writes from meeting context.
- [Human-Approved CRM Write Contract](human-approved-crm-write-contract.md): approval, write order, resume behavior, and verification.
- [Outbound Engagement To CRM](outbound-engagement-to-crm.md): preserving provenance while routing outbound engagement into CRM.

Related templates:

- [CRM Routing Proposal](../templates/crm-routing-proposal.example.json)
- [Approved CRM Write Input](../templates/approved-crm-write-input.example.json)
- [CRM Write State](../templates/crm-write-state.example.json)
- [Verification Report](../templates/verification-report.example.json)
- [Outbound Source Note](../templates/outbound-source-note.example.json)

## Prospect Research And Cleanup

- [Account Research Infrastructure](account-research-infrastructure.md): account-first research before contact enrichment.
- [Prospect Data Cleanup Workflow](prospect-data-cleanup-workflow.md): reviewable cleanup process for messy prospect datasets.
- [Practice-Area Tagging Workflow](practice-area-tagging-workflow.md): taxonomy-first tagging for unstructured descriptions.
- [Signal-Based Prospect Research](signal-based-prospect-research.md): turning public events into review queues.

Related templates:

- [Data Cleanup Review Columns](../templates/data-cleanup-review-columns.csv)
- [Practice-Area Taxonomy](../templates/practice-area-taxonomy.example.csv)
- [Sparse Practice Tags](../templates/sparse-practice-tags.example.csv)
- [Signal Queue Record](../templates/signal-queue-record.example.json)

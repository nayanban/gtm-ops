# Prospect Data Cleanup Workflow

This playbook describes how to clean outbound prospect datasets while keeping review decisions transparent.

## Goal

Turn a messy exported list into a usable prospecting dataset without hiding uncertainty or over-deleting ambiguous records.

This pattern is for datasets where the raw export is directionally useful but not sales-ready. The work is not simply deleting bad rows. It is creating a reviewable decision layer that lets humans understand why a record was kept, removed, or escalated.

## Recommended Final Columns

| Column | Purpose |
| --- | --- |
| Name | Organization name. |
| Description | Main source for business-model review. |
| Primary Industry | Useful signal, not an automatic decision rule. |
| Size | Segmenting and prioritization. |
| Location | Human-readable geography. |
| State or Region | Filtering and routing. |
| Domain | Website and dedupe support. |
| Profile URL | Public company profile or social profile. |

## Cleanup Process

1. Inspect the source file and confirm columns.
2. Preserve a working copy; never overwrite the original first.
3. Remove columns that do not support review or outreach.
4. Add and normalize geography fields.
5. Classify each row as `Keep`, `Remove`, or `Review`.
6. Use the description as the primary basis for removal.
7. Put low-confidence rows into `Review`, not `Remove`.
8. Create separate clean, removed, and review-needed files.
9. Reconcile row counts before using the cleaned dataset.

## Taxonomy Before Tagging

When the cleanup requires practice-area, segment, or buyer-fit tags, build the taxonomy from the dataset before tagging rows.

Do not force a new file into an old taxonomy. First inspect the actual descriptions and answer:

- Which categories appear in this dataset?
- Which categories matter commercially?
- Which categories are ambiguous?
- Which categories need conditional rules?
- Which prior categories should be dropped for this file?

For legal-market datasets, for example, a generic practice area may need a dispute-specific version. `Real Estate` and `Real Estate Litigation` are not interchangeable. `Estate Planning` and `Probate / Trust Litigation` are not interchangeable.

## Batch And Checkpoint Pattern

For large files, use sparse checkpoints rather than a full row-by-column matrix during review.

Recommended checkpoint fields:

| Field | Purpose |
| --- | --- |
| RowNumber | Stable mapping back to the source file. |
| Name | Human review aid. |
| Tags | Semicolon-separated selected tags only. |
| Decision | Keep, Remove, or Review. |
| Reason | Short human-readable rationale. |

Sparse checkpoints reduce noise, preserve progress between batches, and make final writeback auditable.

## Common Removal Categories

- data or workflow vendors;
- support services;
- staffing and recruiting firms;
- media, education, or directory companies;
- claims administration or corporate services;
- low-information placeholders;
- organizations outside the intended buyer profile.

## Review Principles

- Do not delete only because an industry label looks broad.
- Do not keep only because the company name sounds plausible.
- Read the description and business model.
- Keep an audit column with the reason for each remove or review decision.
- Use human review for edge cases.

## QA Checklist

- Source file was not overwritten.
- Final columns are present.
- Row counts reconcile.
- Geography normalization did not alter unrelated text.
- Removed rows have reasons.
- Review rows are preserved for a human decision.
- Tags use blanks for absence, not `No` values.
- The taxonomy reference is saved next to the cleaned outputs.
- Sampled tagged rows match the written rules.

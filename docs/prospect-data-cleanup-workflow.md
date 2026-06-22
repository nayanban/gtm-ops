# Prospect Data Cleanup Workflow

This playbook describes how to clean outbound prospect datasets while keeping review decisions transparent.

## Goal

Turn a messy exported list into a usable prospecting dataset without hiding uncertainty or over-deleting ambiguous records.

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

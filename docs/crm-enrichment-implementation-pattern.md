# CRM Enrichment Implementation Pattern

This pattern describes a vendor-neutral workflow/function layer for safe CRM enrichment.

## Why A Workflow Layer Exists

Native enrichment settings may define mappings but still fail to apply values at the right point in the record lifecycle. A small workflow/function layer can make the enrichment path explicit, testable, and safer.

## Flow

```text
Record created or updated
        |
        v
Read record
        |
        v
Choose trusted website/domain
        |
        v
Call enrichment provider
        |
        v
Validate returned domain
        |
        v
Archive old free-text summary if needed
        |
        v
Write approved company fields
        |
        v
Skip protected person fields
```

## Trusted Domain Selection

Use this priority:

1. Existing verified company website.
2. Work-email domain, if company website is blank.
3. No enrichment when only a personal email domain or ambiguous company name is available.

Maintain a denylist for common personal email providers. Company-name-only enrichment should be treated as low confidence unless another trusted identifier is available.

## Validation Gate

Before writing enrichment output:

- normalize the source website/domain;
- normalize the returned website/domain;
- compare domain roots;
- skip writeback if the returned company does not match the source domain;
- log or note the mismatch for review.

## Write Policy

Write company-level fields such as:

- company website;
- business type;
- size or employee range;
- revenue range, when compatible;
- company profile summary;
- company social/profile links;
- headquarters or location fields, when useful.

Do not write:

- person email;
- person phone;
- person profile URL;
- account relationship fields;
- opportunity, billing, subscription, or customer-success fields.

## Description Preservation

If a free-text summary field already contains useful text, preserve it before replacing it with an enrichment summary.

Public-safe pattern:

1. Read existing summary.
2. If nonblank and different from enrichment output, create a note titled generically, such as `Previous summary before enrichment`.
3. Write the enrichment summary.

## Test Cases

Use synthetic or approved test records for:

- verified company website;
- work-email-domain derivation;
- personal email exclusion;
- provider returns no useful data;
- provider returns mismatched domain;
- existing summary preservation;
- company fields present but person fields protected.

## Pseudocode

```text
record = read_record(record_id)
source_domain = choose_verified_website_or_work_email_domain(record)

if source_domain is blank or personal_email_domain(source_domain):
    mark_skipped("No trusted company domain")
    stop

provider_result = request_company_enrichment(source_domain)

if provider_result is blank:
    mark_skipped("No enrichment result")
    stop

if normalized_domain(provider_result.website) != normalized_domain(source_domain):
    mark_skipped("Returned company does not match source domain")
    stop

if record.summary is nonblank and record.summary != provider_result.summary:
    create_note("Previous summary before enrichment", record.summary)

safe_fields = select_approved_company_fields(provider_result)
write_record_fields(record_id, safe_fields)
read_back_record(record_id, safe_fields)
report_success_or_warning()
```

The pseudocode intentionally omits vendor API names, credentials, endpoints, and private field IDs. The reusable lesson is the control flow: read, choose a trusted domain, validate, preserve, write safe fields, and verify.

For operational checks, see [Enrichment Failure Modes](enrichment-failure-modes.md).

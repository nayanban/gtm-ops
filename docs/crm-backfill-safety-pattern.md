# CRM Backfill Safety Pattern

Backfills are riskier than single-record automations because a bad rule can affect many records at once. Treat them as controlled migrations.

## Backfill Principles

- Dry-run first.
- Make candidate criteria explicit.
- Exclude ambiguous or protected records.
- Test on a small reviewed sample.
- Apply in batches.
- Store run output.
- Verify final candidate count.
- Keep rollback/readback expectations clear.

## Candidate Criteria

Define candidates narrowly. Example criteria:

- target field is blank;
- trusted source field exists;
- source is not a personal email domain;
- record is in the intended module or status;
- record is not tied to protected customer, billing, or subscription workflows;
- record has not already been processed.

## Exclusions

Exclude:

- personal email domains;
- internal/company-owned domains when they would create noise;
- records with existing verified company websites;
- converted or archived records, unless explicitly in scope;
- records with ambiguous identity;
- records already in an incomplete provider job state.

## Dry Run Output

A useful dry run should report:

- candidate count;
- sampled candidate records;
- reason each candidate qualified;
- fields that would be written;
- records skipped and why;
- expected downstream workflow effects.

## Batch Apply

Apply in small batches first. After each batch:

- read back written records;
- inspect failed or skipped records;
- confirm no protected fields changed;
- confirm old summaries were preserved when needed;
- check for encoding or formatting problems.

## Completion Check

End with a final dry run. The ideal result is:

```text
candidate_count: 0
```

If candidates remain, explain why they remain and whether they should be processed, excluded, or reviewed manually.

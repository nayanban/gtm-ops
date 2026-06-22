# Practice-Area Tagging Workflow

This playbook describes how to convert unstructured organization descriptions into filterable practice-area or segment tags.

## Goal

Create useful review columns without pretending keyword matching is enough.

## Taxonomy First

Before tagging rows, inspect the dataset and build a taxonomy from the descriptions in that file.

Ask:

- Which practice areas or segments appear?
- Which are commercially relevant?
- Which are ambiguous?
- Which require conditional rules?
- Which older categories should not be reused for this file?

Do not force a new dataset into a taxonomy created for a different file.

## Litigation-Related Example

For legal-market data, distinguish litigation/adversarial work from transactional or advisory work.

Examples:

| General Area | Tag Only When |
| --- | --- |
| Real estate | Description signals disputes, litigation, foreclosure, eviction, or contested proceedings. |
| Probate/trusts | Description signals will contests, fiduciary disputes, or trust/estate litigation. |
| Intellectual property | Description signals infringement, enforcement, disputes, or litigation. |
| Tax | Description signals audits, appeals, controversy, or tax litigation. |
| Healthcare | Description signals disputes, enforcement, malpractice, or provider/payor litigation. |

Do not tag a litigation category merely because the underlying practice area appears.

## Output Values

Use:

```text
Yes
```

or blank.

Do not use `No`. Blank means not tagged.

## Sparse Checkpointing

For large files, checkpoint sparse tags instead of producing a full matrix during review.

Recommended checkpoint columns:

```text
RowNumber,Name,PracticeAreas
```

Use row number as the primary key for writeback. Names can repeat, change punctuation, or contain formatting differences.

## Reference Taxonomy

Create a reference file with:

```text
PracticeArea,TaggingRule,ExamplesOrSignals,Exclusions
```

This makes the judgment layer reusable and reviewable.

## Writeback And Verification

Before writing tags to the source file:

1. Create a timestamped backup.
2. Confirm source row count.
3. Confirm checkpoint row keys.
4. Add finalized tag columns after the existing review columns.
5. Write `Yes` only where the checkpoint says the row has that tag.
6. Verify row count is unchanged.
7. Sample tagged rows against the taxonomy.
8. Confirm blanks are blank, not `No`.

## Quality Risks

- Over-tagging broad practice areas.
- Treating advisory work as litigation.
- Ignoring ambiguous rows.
- Losing row alignment during writeback.
- Forgetting to preserve multiline descriptions.

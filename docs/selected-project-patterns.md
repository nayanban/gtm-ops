# Selected Project Patterns

This page summarizes the larger bodies of work represented by the playbooks in this repository. The examples are anonymized and generalized for public review.

## 1. CRM Enrichment Rebuild

Problem:

A CRM contained a mix of top-of-funnel people, post-lead contacts, stale prospecting records, and protected customer/account workflows. Enrichment could improve data quality, but enabling it broadly risked overwriting useful context or polluting person fields with company data.

What the work covered:

- Defined the boundary between lead, contact, account, opportunity, subscription, and legacy records.
- Built a company-enrichment pattern triggered from website or work-email domain.
- Preserved old free-text descriptions before enrichment replaced them.
- Kept person profile fields separate from company profile fields.
- Audited related automations before touching protected workflows.
- Moved stale/orphan person records into a reviewable legacy area instead of deleting them blindly.
- Parked person enrichment where the available provider path did not meet overwrite and data-model requirements.

Reusable lesson:

CRM enrichment is less about calling an enrichment API and more about deciding what each module is allowed to know, write, preserve, and ignore.

## 2. Meeting Notes To CRM

Problem:

High-context meetings needed to become CRM records without creating duplicates, losing nuance, or turning every meeting note into an unreviewed write.

What the work covered:

- Target-person selection before field extraction.
- Duplicate checks across likely record surfaces.
- A proposal step before any CRM write.
- Detailed activity creation for full meeting intelligence.
- Concise notes for quick human review.
- Task creation only when a concrete next action existed.
- Idempotent write helpers that saved state after each successful object creation.
- Verification readback after writes.

Reusable lesson:

The safest automation pattern is proposal, approval, idempotent write, verification. The automation should accelerate the work without taking over human judgment.

## 3. Outbound Engagement To CRM

Problem:

Outbound engagement events could signal interest, but they needed to update CRM records without losing source provenance or bypassing existing enrichment workflows.

What the work covered:

- Event-triggered create/update flow for leads.
- Source notes as both provenance and workflow markers.
- CRM-native function triggered by a specific note type.
- Person-field enrichment from the outbound platform.
- Downstream company enrichment through existing CRM workflows.
- Avoided creating a custom marker field when a note could preserve the source trail.

Reusable lesson:

Provenance can be part of the automation design. A source note can tell humans why a record changed while also giving the CRM a reliable trigger.

## 4. Prospect Data Cleanup And Tagging

Problem:

Exported account lists were directionally useful but included vendors, service providers, ambiguous organizations, thin records, and unstructured descriptions that were not filterable enough for outreach planning.

What the work covered:

- Defined final review columns.
- Normalized geography.
- Classified rows as keep, remove, or review.
- Used descriptions as the main basis for decisions.
- Built tagging taxonomies from each file instead of forcing old categories onto new data.
- Used sparse checkpoints to process large files in reviewable batches.
- Created clean, removed, and review-needed outputs with row-count reconciliation.

Reusable lesson:

The value is not only the cleaned file. The value is the decision trail that lets someone trust the cleaned file.

## 5. Signal-Based Prospect Research

Problem:

Public events created possible sales signals, but raw alerts were noisy. A useful system needed to identify actionable entities, preserve source evidence, and defer commercial judgment to a human reviewer.

What the work covered:

- Candidate signal records with deterministic dedupe keys.
- Queue status for extraction state.
- Separate event-context and entity-identified notes.
- External worker pattern for document retrieval and parsing.
- Explicit statuses for entity found, no entity found, and extraction failed.
- Cleanup rules that avoid deleting unprocessed pending records.

Reusable lesson:

Signal systems should expose uncertainty instead of hiding it. A raw event becomes useful only after entity extraction, evidence capture, and review.

## 6. Account Research Infrastructure

Problem:

Manual prospect research and broad scraping were too slow and uneven. Account selection needed a more repeatable way to compare known-fit accounts, build account universes, and deepen only the strongest candidates.

What the work covered:

- Account-level patterning before person-level enrichment.
- Technology and hiring signals as fit filters.
- Staged account universe creation.
- Credit-aware enrichment rules.
- Contact discovery only after account review.
- Saved-list workflows for reviewed target sets.

Reusable lesson:

Do account strategy before contact enrichment. It is cheaper, cleaner, and easier to review.

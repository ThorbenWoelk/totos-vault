---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - data-engineering
connections: []
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - Data
  - Implementation & Quality
---
## Failure mode

Migrating a growing slowly changing dimension type 2 (SCD2) table through intermediate files can create
duplicate rows in the destination table.

The open-ended row, whose validity end date is conventionally set to **9999**, may be updated after an earlier
export has already been loaded. The receiving table can then contain two rows for an identifier that is
expected to be unique.

## Required safeguard

Add a cleansing step that deduplicates the affected identifiers before publishing the destination table.

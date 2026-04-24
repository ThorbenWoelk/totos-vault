---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - dbt
  - bigquery
  - materialization
  - incremental
  - insert-overwrite
  - merge
  - partitioning
  - clustering
  - view
  - ephemeral
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[05 Incremental Processing Patterns]]"
  - "[[10 Modern Alternatives and Optimization]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## Introduction
"Materialization" defines how dbt persists your model in the database: as a Table, a View, or Ephemeral code. In a BigQuery environment processing terabytes of data, this choice dictates performance and cost.

## 1. Incremental Models
For large Fact tables, dropping and recreating the table daily is impossible (too slow, too expensive). We use **Incremental** materialization.

### Strategy A: `insert_overwrite`
This is the preferred strategy for event data (Fact Tables).
* **How it works**: It doesn't look at individual rows. It looks at **Partitions** (e.g., Days). If you run the model for '2023-10-25', it completely wipes the partition '2023-10-25' and replaces it with the new calculation.
* **Pros**:
    * **Atomic**: No half-written data.
    * **Cost**: No need to query the existing table to check for duplicates (unlike Merge).
    * **Idempotent**: Running it twice is safe; the result is strictly the new data.
* **Requirement**: The table MUST be partitioned.

```yaml
# dbt_project.yml
+incremental_strategy: insert_overwrite
+partition_by:
  field: date
  data_type: date
  granularity: day
```

### Strategy B: `merge` (Upsert)
This is used when data can change historically without respecting partition boundaries, or for Dimensions.
* **How it works**: dbt matches new rows to old rows via a `unique_key`. If a match is found, it updates. If not, it inserts.
* **Cons**: Expensive. BigQuery effectively has to scan the destination table to find matches.

## 2. Partitioning
Partitioning divides tables physically by a Date or Timestamp column.
* **Effect**: When you run `WHERE date = '2023-01-01'`, BigQuery *only* scans that one specific slice of the hard drive.

**Implementation**:
Every core Fact table in this project is partitioned by `date` (derived from timestamp).

```yaml
+partition_by:
  field: date
  data_type: date
  granularity: day
```

## 3. Clustering
Within a partition, data can be "Clustered" (sorted) by specific columns.
* **Usage**: Clustered by high-cardinality filter columns, typically `session_sk` or `user_sk`.
* **Benefit**: If you filter `WHERE date = '...' AND user_sk = 123`, BigQuery can skip blocks of data within the partition that don't contain that user.

## 4. Ephemeral Models
**Materialization**: `ephemeral`
* **Behavior**: These models are never created in the database. dbt compiles them as Common Table Expressions (CTEs) into the downstream models.
* **Use Case**: Reusable logic blocks used in the Intermediate layer (e.g., `intermediate/mapp/ephemerals`).
* **Warning**: Don't chain too many ephemeral models; the compiled SQL becomes unreadable and hard for the query optimizer to handle.

## 5. Views
**Materialization**: `view`
* **Behavior**: Saved as a logical view. No data storage. Code runs every time you query it.
* **Use Case**: low-computation layers like `bq_audit` or simple renames. Not suitable for complex transformations.


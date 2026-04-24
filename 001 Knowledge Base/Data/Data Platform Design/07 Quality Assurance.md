---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - data-quality
  - dbt
  - schema-tests
  - custom-tests
  - data-freshness
  - statistical-testing
  - testing-strategy
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[02 Dimensional Modeling Kimball]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## The Philosophy
In Data Engineering, **Trust is the Product**. If the numbers are wrong once, business users lose trust. Integrating tests should be in the definition of "done".

## 1. Schema Tests (The Basics)
Defined in the `.yml` files alongside model definitions.
* **`not_null`**: Essential for Primary Keys and critical Foreign Keys.
* **`unique`**: Essential for Primary Keys.
* **`accepted_values`**: Ensures a status column only acts within valid bounds (e.g., `['active', 'churned']`).

## 2. Custom Generic Tests
Sometimes built-in tests aren't enough. We write custom test logic in `tests/generic`.

**Example: `test_duplicates`**
The standard `unique` test allows checking one column. What if a combination of columns `(user_id, date)` must be unique?
The codebase implements `test_duplicates.sql` which accepts a list of columns and uses BigQuery's efficient `qualify` statement to find violations.

```sql
-- Efficient Duplicate Check in BigQuery
select * from {{ model }}
qualify count(*) over (partition by {{ columns_to_check }}) > 1
```

## 3. dbt Expectations (Statistical Testing)
The project utilizes the `dbt_expectations` package heavily (see `packages.yml`). This allows for much more sophisticated data quality checks than simple null checks.

**Use Cases in Codebase:**
* **Volume Anomaly**: "Did the `page_views` count drop by more than 40% compared to yesterday?"
    *   *Test*: `expect_table_aggregation_to_equal_other_table` matches today's sum vs yesterday's sum with a tolerance.
* **Type Safety**: `expect_column_values_to_be_of_type` ensures consistency (e.g., ensuring an ID is explicitly `int64`).
* **Standard Deviation**: `expect_column_values_to_be_within_n_stdevs` detects outliers that are statistically significant, rather than just hitting a hardcoded threshold.

## 4. Source Freshness
Crucial for a Snowplow pipeline.
* **Definition**: In `sources.yml`, we define `freshness` blocks.
* **Logic**: "If the data in the `events` table is older than 2 hours, WARN. If older than 6 hours, ERROR."
* **Impact**: We know immediately if the pipeline is stuck, before the daily dbt run fails on empty data.

## Testing Strategy Summary
1.  **Strictness**: Critical keys are always tested for uniqueness/nulls.
2.  **Severity**: Warnings are used for "suspicious" data (statistical outliers), Errors for "impossible" data (null primary keys).
3.  **Layering**: Testing happens at **Source** (Freshness), **Staging** (Types), and **Core** (Business Rules).


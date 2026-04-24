---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - incremental-processing
  - dbt
  - bigquery
  - cost-optimization
  - late-arriving-data
  - backfill
  - sql-macro
  - data-pipeline
  - windowing
  - join-optimization
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[03a BQ Optimization Guide]]"
  - "[[02 Dimensional Modeling Kimball]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## The Challenge
For "High Volume" , you cannot run `SELECT * FROM source` every day.
* **Full Refresh**: Processing 5 years of clickstream data might cost €100+ per run and take hours.
* **Naive Incremental**: `WHERE timestamp > (select max(timestamp) from target)` is often inefficient in BigQuery because finding the `max()` requires scanning the huge target table.

## The Solution: Smart Windowing
This project implements a sophisticated macro `configure_date_filter` to handle load windows deterministically.

### The Macro: `configure_date_filter`
Located in `macros/load_variables/configure_date_filter.sql`.

**Core Logic:**
It determines a `load_start_date` and `load_end_date` based on:
1.  **Is this a Full Refresh?** (dbt run --full-refresh) -> Load everything (or a large backfill window).
2.  **Is this an Incremental Run?** -> Load `[Today - X Days]` to `[Today]`.

**Code Snippet (Logic):**
```sql
{% macro configure_date_filter(...) %}
    -- Calculate Start Date (e.g., usually 'Today - 3 Days' for safety)
    {% set load_start_date = ... %}

    {% if source_table_type == 'event_data' %}
        -- For Facts: Strict Date Window
        date(column) between {{ load_start_date }} and {{ load_end_date }}

    {% elif source_table_type == 'scd2' %}
        -- For Dimensions: Intersection Window
        -- We need any version of the dimension that was valid at ANY point during the load window
        date(valid_from) <= {{ load_end_date }}
        and date(valid_to) >= {{ load_start_date }}
    {% endif %}
{% endmacro %}
```

### Why "Today - 3 Days"?
Why not just "Yesterday"?
1.  **Late Arriving Data**: Mobile devices might be offline and upload tracking events 24 hours later. We re-process the last ~3 days to catch these "late arrivals" and update the partitions correctly (`insert_overwrite`).
2.  **Safety**: If the pipeline breaks for a day, the next run covers the gap automatically without manual intervention.

### Optimizing Joins
This macro is applied **on every side of a join**.
When joining `Page Views` (Huge) to `Sessions` (Huge), we apply the filter to *both* CTEs before joining.
* **Challenge**: Join 10 years of Sessions to usage.
* **Better pattern**: Filter Sessions to `[Last 3 Days]`, filter Page Views to `[Last 3 Days]`, then join.

**Pattern:**
```sql
with source_data as (
    select * from source
    where {{ configure_date_filter(incremental_load_days_offset=3) }}
),
session_data as (
    select * from sessions
    where {{ configure_date_filter(incremental_load_days_offset=3) }}
)
select ... from source_data join session_data ...
```

This pattern ensures that our daily costs remain flat and low, regardless of how much historical data we accumulate.

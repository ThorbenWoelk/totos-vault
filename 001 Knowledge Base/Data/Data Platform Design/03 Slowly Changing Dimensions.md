---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - slowly-changing-dimensions
  - dbt
  - bigquery
  - dimensional-modeling
  - snapshots
  - point-in-time-join
  - asof-join
  - range-join
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[02 Dimensional Modeling Kimball]]"
  - "[[03a BQ Optimization Guide]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## The Problem: Mutability
Data in the real world changes.
*   A user moves from Berlin to Munich.
*   An article's title is fixed from "Teh" to "The".

If we simply overwrite the data, we lose history.
## SCD Types

### Type 1: Overwrite (SCD1)
* **Action**: Update the existing row with new values.
* **History**: Lost. We only know the *current* state.
* **Use Case**: Correcting spelling errors, or when history is genuinely irrelevant.
* **Example**: `core_dim_user` (for current status).

### Type 2: Add Row (SCD2)
* **Action**: Mark the old row as "expired" and insert a new row for the current state.
* **History**: Preserved. We have a row for every state the entity has ever been in.
* **Columns**: Requires `valid_from` and `valid_to` timestamps.
* **Use Case**: Tracking changes in Article Metadata (`core_dim_page_changelog`), Subscription Status.

## Implementing SCD2 in dbt

### 1. The Structure
An SCD2 table will look like this:

| page_sk | page_id | title | valid_from | valid_to |
| :--- | :--- | :--- | :--- | :--- |
| 101 | A | Teh End | 2023-01-01 | 2023-01-02 |
| 102 | A | The End | 2023-01-02 | 9999-12-31 |

* **Current Row**: Usually has `valid_to` set to a far-future date (e.g., `9999-12-31`).
* **Primary Key**: The `page_id` is no longer unique! You need a new Surrogate Key (`page_sk`) for each *version* of the page.

### 2. The Logic via dbt Snapshots (Standard)
dbt provides a `snapshots` feature to automate this. It compares source data to target data and handles the inserts/updates.
*   *Note*: In high-volume scenarios (like this project), custom SQL logic in `intermediate` models is sometimes used instead of standard snapshots for performance control.

### 3. Point-in-Time Joins (Crucial)
When joining a Fact table to an SCD2 Dimension, a simple join on ID is wrong (it would explode rows, matching all versions). You must join on **Time**.

**The Pattern:**
"Find the version of the Page that was valid *at the exact moment* the Page View occurred."

```sql
select
    fact.view_id,
    dim.title as page_title_historically_accurate
from core_fact_page_views_sp as fact
left join core_dim_page_changelog as dim
    on
        -- 1. Match the Entity
        fact.page_id = dim.page_id
        -- 2. Match the Time Window
        and fact.timestamp >= dim.valid_from
        and fact.timestamp <  dim.valid_to
```

## SCDs in `<client-dbt-project>`
The project uses `core_dim_page_changelog` to track edits to articles.
* **Impact**: When computing "Page Views by Ressort (Department)", we reference the department the article belonged to *at that time*, even if it moved later.
* **Complexity**: This makes joins slower. The `configure_date_filter` macro (discussed in Module 05) is vital here to ensure we don't scan the entire history of the dimension for every query.

# High-Cost Range Joins

### What could you have done?

To keep the Star Schema but fix the specific performance bottleneck in BigQuery:

1. **ASOF JOIN** BigQuery now supports `ASOF JOIN` (or the `RANGE_BUCKET` pattern). This is specifically optimized for "find the record valid at this timestamp". It tells the engine to sort both tables by time and scan them in parallel, avoiding a large intermediate join.

2. **Array Unnesting (The "BigQuery Native" Way)** Instead of joining, you aggregate the Dimension history into an ARRAY of structs (one row per Page, with a nested history).

    - Join on `Page ID` (Fast Hash Join).
    - Then, inside the `SELECT`, use a User Defined Function (UDF) or array logic to pick the correct version from the array based on the timestamp. This happens locally on the slot (CPU bound) rather than across the network (Shuffle bound).
3. **Bridge Tables** Create a "Day-Grain" bridge. If dimensions only change daily, you resolve the key _once_ per day/page combo, then join your facts to that daily table on purely `Page_ID` + `Date`.


### Key Lesson

**Strict Dimensional Modeling (SCD Type 2) can be high-cost on Columnar Cloud DBs** if implemented as a raw SQL `JOIN ... BETWEEN ...`. The "Modern Data Stack" often prefers **Snapshotting** (periodic full state) or **Activity Schemas** to avoid this specific range-join pattern.

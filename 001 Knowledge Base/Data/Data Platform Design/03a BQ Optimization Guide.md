---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - bigquery
  - dbt
  - scd2
  - join-optimization
  - denormalization
  - schema-evolution
  - bridge-table
  - one-big-table
  - asof-join
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[03 Slowly Changing Dimensions]]"
  - "[[05 Incremental Processing Patterns]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
# BigQuery Optimization Guide: Joins & Schema Evolution

## Part 1: Managing High-Cost SCD2 Joins

### The Scenario
You have a massive Fact table (e.g., `fact_page_views`) and a Slowly Changing Dimension Type 2 (e.g., `dim_page_changelog` with `valid_from` and `valid_to`).

### Baseline Range Join Pattern
```sql
select ...
from facts f
left join dim d
  on f.page_id = d.page_id
  and f.timestamp >= d.valid_from
  and f.timestamp < d.valid_to
```

### Why performance degrades
BigQuery (and most columnar stores) relies on **Hash Joins** (O(1) lookup on equality).
Adding a range condition (`>=`, `<`) forces the engine to perform a **Cross Join** (Cartesian Product) bounded by the filter.
* **Hash Join**: "Bucket A goes to Bucket A". Fast.
* **Range Join**: "Check every single version of Page X against every single view of Page X".
* **Result**: Massive "Shuffle" operations that exhaust slots.

---

## Solution 1: Simulating ASOF JOIN (The "BigQuery Native" Way)
*Note: BigQuery does not yet have the `ASOF JOIN` keyword found in Snowflake/DuckDB. You must implement the logic manually using Arrays.*

This approach moves the complexity from **Network/Shuffle** to **CPU/Memory**. It is currently the most efficient way to handle SCD2 lookups in BigQuery without custom UDFs.

1.  Group the dimension history into a single ARRAY per ID.
2.  Hash Join on ID (Fast).
3.  Search the array *inside* the select statement to find the correct version.

### Code Example (dbt)

```sql
/* models/core/facts/fact_page_views_optimized.sql */

with dim_array as (
    select
        page_id,
        -- Pack history into an array of structs, sorted by time
        array_agg(
            struct(valid_from, valid_to, title, author)
            order by valid_from asc
        ) as history
    from {{ ref('dim_page_changelog') }}
    group by 1
),

facts as (
    select * from {{ ref('fact_page_views') }}
)

select
    f.view_id,
    f.timestamp,
    -- Extract the correct version from the array in-memory calling the "ASOF" logic
    (
        select title
        from unnest(d.history)
        where f.timestamp >= valid_from
        and f.timestamp < valid_to
        limit 1
    ) as historical_title

from facts f
-- 1. Fast Hash Join on ID only (No Range Condition in the JOIN)
left join dim_array d using (page_id)
```

**Pros**:
* **Performance**: Avoids the "Range Join" shuffle explosion. Runs linearly with data volume.
* **Cost**: Significantly cheaper than Standard SQL range joins.

**Cons**:
* **Limits**: If the history array for a single ID exceeds BQ's row size limit (100MB), this fails. (Rare for page metadata).


---

## Solution 3: The Bridge Table (Day-Grain)
If your dimensions rarely change more than once a day, you don't need second-level precision joins. Resolve the key **once per day** and join on `(Date, ID)`.

### Step A: Create the Bridge (dbt)
```sql
/* models/core/bridge/bridge_page_day.sql */

select
    d.date_day,
    p.page_id,
    p.page_sk -- The surrogate key for that specific day
from {{ ref('util_dates') }} d
inner join {{ ref('dim_page_changelog') }} p
    on d.date_day >= date(p.valid_from)
    and d.date_day < date(p.valid_to)
```

### Step B: Join Fact to Bridge
```sql
/* models/core/facts/fact_page_views_bridge.sql */

select
    f.view_id,
    b.page_sk
from {{ ref('fact_page_views') }} f
left join {{ ref('bridge_page_day') }} b
    on f.page_id = b.page_id
    and date(f.timestamp) = b.date_day -- Double Hash Join (Date + ID)
```

**Pros**: Pure Equi-Join (fastest possible join type).
**Cons**: You lose intra-day precision (if a title changes at noon, morning views might get the noon title depending on logic).

---

## Solution 4: OBT (One Big Table) / Denormalization
Instead of resolving the data *after* it's stored, you capture the state *before* or *during* ingestion. This completely eliminates join complexity at the cost of duplicate storage.

### Strategy A: Schema on Write (Upstream)
Change the tracking code (Snowplow/Frontend) to send the "Page Title" and "Author" as part of the event payload itself.
* **Result**: The raw JSON already contains `{"page_title": "Breaking News", "author": "Alice"}`.
* **Transformation**: Trivial `CAST(json.page_title as STRING)`. Zero Joins.

### Strategy B: Denormalized Materialization (Downstream)
If you can't change the upstream tracker, use Solution 1 (`ASOF JOIN`) or Solution 2 (`Array`) **ONCE** to create a massive, wide table that contains all columns.

### Code Example (dbt)
```sql
/* models/marts/page_views_wide.sql */
/* Materialized as a TABLE (not View) so analysts pay 0 join cost */

{{ config(
    materialized='incremental',
    partition_by={'field': 'timestamp', 'data_type': 'timestamp'},
    cluster_by=['page_id', 'author_name']
) }}

select
    f.view_id,
    f.timestamp,
    -- Store the ACTUAL STRINGS, not IDs
    d.title as page_title_historically_accurate,
    d.author_name,
    d.category
from {{ ref('fact_page_views') }} f
-- Use the optimized join technique here ONCE during the nightly run
asof join {{ ref('dim_page_changelog') }} d
    on f.page_id = d.page_id
    and f.timestamp >= d.valid_from
```

**Pros**:
* **Query Speed**: The fastest possible performance for end-users (Analysts doing `select *` just works).
* **Simplicity**: No complex `JOIN` logic for BI tools like Tableau/Looker.

**Cons**:
* **State Reset**: If you find a bug in the `Author` logic, you must fully reload/reprocess the 2TB table (whereas in Star Schema, you just fix the small Dimension table).
* **Storage**: Uses more storage (repeating string "Breaking News" 1 million times), though columnar compression handles this very well.

### ⚠️ Safety Check: Does this duplicate rows?
**No**, not for SCD2 attributes.
* **Why**: A Page View happens at a single specific timestamp. At that exact moment, the Page had exactly **one** specific state (Title="A", Author="B").
* **Result**: The join is 1:1. Your row count remains identical. `SUM(page_views)` works perfectly.

**Danger Zone**:
If you denormalize **1:Many** relationships (e.g., joining `page_view` to `page_tags` where a page has 5 tags), **THAT** will explode your rows and break your sums.
* **Fix**: For 1:Many, use **Arrays** (Solution 2) inside your OBT table (e.g., `tags: ["news", "politics"]`). Do not flatten/explode them.

---

## The Tradeoff: Why Experts Often Push Back on OBT

If OBT is fast, data modeling reviewers may still raise three major concerns:

### 1. The "Single Source of Truth" Problem (Consistency)
* **Star Schema**: You define a "Customer" **once** in `dim_customers`. Every single report (Sales, Marketing, Finance) joins to that one table. If you update a definition, everyone sees it instantly.
* **OBT**: You copy "Customer Name" into `fact_sales_flat`, `fact_marketing_flat`, and `fact_finance_flat`.
* **Risk**: If logic drifts, Marketing might report "User A" as "VIP" while Finance reports them as "Standard". This reduces trust in data.

### 2. The "Refactoring Cost" Problem (Maintenance)
* **Scenario**: You discover a bug in your "User Segmentation" logic from 3 years ago.
* **Star Schema**: You fix the logic in `dim_users` (Small table). The 100-billion row fact table remains untouched.
* **OBT**: You must **reload and recompute** 100 billion rows in your massive flat table to backfill the fix. This can cost thousands of dollars and take days in a cloud warehouse.

### 3. The "Code Dryness" Problem
* **Star Schema**: DRY (Don't Repeat Yourself).
* **OBT**: WET (Write Everything Twice). You effectively hardcode dimension logic into every single pipeline that needs it.

### Recommendation
* **Use Star Schema (Kimball)** as your **Warehouse Layer** (Clean, Consistent, Normalized).
* **Use OBT** as your **Presentation Layer** (Strictly for BI/Performance).
*   *Don't replace the Star Schema with OBT; build OBT **on top** of it.*

---

## Part 2: The "Jigsaw Pattern" (Adding Features Without Large Rebuilds)

### The Problem: Schema Evolution Cost
You want to add **one new column** (e.g., `extracted_sentiment`) to your 2TB Fact Table.
* **Traditional ETL**: You must rewrite the entire 2TB table to add the column.
* **Impact**: Massive slot usage, long downtime, high cost. "Incremental" models struggle here because they only care about *new* rows, not *new columns for old rows*.

### The Solution: Vertical Decomposition
Instead of altering the "Main" Fact Table, create a "Sidecar" Fact Table and join them in a View.

#### Step 1: Create the Sidecar (The "Shard")
Create a small, narrow table that contains ONLY the key + the new feature.
* **Source**: Read raw data (or whatever source is needed).
* **Cost**: You only write the new column, not the other 500 columns.

```sql
/* models/features/fact_page_views_sentiment.sql */
{{ config(materialized='table') }}

select
    view_id, -- The Join Key
    ml_sentiment_score(content_body) as sentiment -- The New Expensive Feature
from {{ ref('stg_snowplow_events') }}
```

#### Step 2: Glue it together (The "View")
Update your "Presentation Layer" model to join the Main Fact with the Sidecar.
* **Join Type**: Left Join on Primary Key (`view_id`).
* **Performance**: BigQuery excels at 1:1 Hash Joins on Integer Keys. It is negligible cost at query time.

```sql
/* models/marts/fact_page_views_unified.sql */
{{ config(materialized='view') }} -- It's just a Virtual View!

select
    main.*,
    sidecar.sentiment
from {{ ref('core_fact_page_views_sp') }} main
left join {{ ref('fact_page_views_sentiment') }} sidecar
    using (view_id)
```

### Why this works
1.  **Zero Rewrites**: The 2TB Main Table is untouched.
2.  **Fast Backfill**: You only compute the specific new metric.
3.  **Modular**: Different teams can own different "Sidecars" (e.g., `fact_page_views_marketing`, `fact_page_views_product`).

---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - dimensional-modeling
  - star-schema
  - surrogate-keys
  - dbt
  - bigquery
  - data-warehouse
  - sql
  - kimball
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[03 Slowly Changing Dimensions]]"
  - "[[01 Architecture and Layering]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## Introduction
**Dimensional Modeling** is a technique popularized by Ralph Kimball. It is designed to maximize query performance and ease of use for business analysts. The central concept is the **Star Schema**.
## The Star Schema
In a Star Schema, data is organized into two types of tables:
1.  **Fact Tables**: The center of the star. They contain the *metrics* of the business.
2.  **Dimension Tables**: The points of the star. They contain the *context* or *attributes*.
### Fact Tables (`core/facts`)
Facts are "Long and Narrow". They capture events that happen at a specific point in time.

* **Characteristics**:
    * **High Volume**: Millions or billions of rows (e.g., every single click).
    * **Immutable**: An event that happened in the past doesn't change (usually).
    * **Keys**: Contain Foreign Keys (dim's Surrogate Keys) to link to Dimensions. In robust Data Warehouses, you rarely join on "Business Keys" (like an email address or a messy string Product ID) because they are slow and unstable. Instead, the Dimension table creates a clean, integer **Surrogate Key** (ID: 1, 2, 3...) for every row which you're using here.
    * **Measures**: Numeric values that can be aggregated (e.g., `page_views`, `revenue`).

* **Example**: `core_fact_page_views_sp`
    *   *Grain*: One row per page view.
    *   *Metrics*: `engaged_time_in_s`, `vertical_percentage_scrolled`.
    *   *Foreign Keys*: `session_sk`, `user_sk`, `page_sk`.

### Dimension Tables (`core/dims`)
Dimensions are "Short and Wide". They describe the "Who, What, Where" of an event.

* **Characteristics**:
    * **Low Volume**: Thousands or millions of rows (much smaller than facts).
    * **Descriptive**: Text-heavy columns (e.g., `page_title`, `user_city`).
    * **Referenced**: Joined by many different Fact tables.

* **Example**: `core_dim_page`
    *   *Attributes*: `title`, `rubric`, `author`, `publication_date`.

## Surrogate Keys (SKs) in Big Data
**Hashed Keys**
We use **Deterministic Hashing** to generate SKs.
* **Function**: `farm_fingerprint(string)` (BigQuery specific).
* **Input**: The unique Business Key (e.g., `user_id` string).
* **Output**: An integer-like hash (e.g., `857392019`).

**Advantages:**
1.  **Parallelizable**: Can be calculated on millions of rows simultaneously without coordination.
2.  **Deterministic**: The same input always gives the same output. You can recalculate it anytime.
3.  **Independence**: It creates a standard `int64` key regardless of whether the source ID is a UUID, a string, or an integer.

### Implementation Pattern
The macro `calculate_sk_for_id` encapsulates this logic to ensure safety (handling NULLs).

```sql
-- Pattern usage in dbt model
select
    -- If user_id is null, sk is 0. Else hash it.
    {{ calculate_sk_for_id('t', 0, 'user_id', 'user_sk') }}
from source_table t
```

## The "Unknown" Member
Every dimension should have a "row 0" or "Unknown" member.
* **SK**: `0` or `-1`.
* **Purpose**: When a Fact record has a missing or null foreign key (e.g., a page view where the user is not logged in), it joins to this "Unknown" row rather than dropping out of the join result (inner join) or having NULL attributes (left join).
* **Outcome**: Reports show "Unknown" instead of missing data, which is clearer for analysts.


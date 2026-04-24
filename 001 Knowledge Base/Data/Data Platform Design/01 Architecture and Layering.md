---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - data-platform-architecture
  - dbt
  - layered-architecture
  - naming-conventions
  - ingestion
  - transformation
  - data-warehouse
  - data-modeling
  - orchestration
  - monitoring
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[02 Dimensional Modeling Kimball]]"
  - "[[03 Slowly Changing Dimensions]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
# Introduction
A robust data platform is built on a foundation of clear, modular architecture. This ensures separation of concerns, modular replacement, and maintainability.

# High-level Data Platform Components

## Data Ingestion (EL)
## Data Transformation (T)

### DWH

#### dbt
In the `<client-dbt-project>` project, we strictly adhered to a multi-layered approach and "DAG" (Directed Acyclic Graph) structure.

```
models/
├── import/           # Raw sources
├── cleansing/        # Standardization
├── intermediate/     # Complex Logic / Pre-computation
├── core/             # Final Marts
│   ├── facts/        # Fact Tables
│   └── dims/         # Dimension Tables
└── exposures/        # Downstream Dependencies
```

The data flows from raw sources to consumption-ready marts through the following distinct layers:

**1. Import (Sources)**
**Location:** `models/import`

**Purpose:**
The entry point for all data. This layer maps raw database tables to dbt source definitions.
* **No Logic**: Do not perform transformations here.
* **Renaming**: Renaming only to strictly compliant snake_case names if necessary.
* **Selection**: Picking only the columns we intend to use downstream.

**Convention:**
*   Files are typically YAML source definitions or very simple SQL `select *`.
*   Prefix: `import_`.

**2. Cleansing (Staging)**
**Location:** `models/cleansing`

**Purpose:**
Standardize raw data into a consistent format.
* **Type Casting**: Converting strings to booleans, timestamps to standard UTC/Berlin time.
* **Null Handling**: Standardizing `NULL` vs empty strings.
* **Value Mapping**: converting `1/0` to `true/false`, or mapping status codes to readable names.
* **Grain**: The grain (one row per X) MUST match the source. No aggregations.

**Convention:**
*   Files are 1:1 with source tables.
*   Prefix: `cleansing_` (often abbreviated as `stg_` in other projects, but `cleansing_` here).
* **Example**: `cleansing_page_views_sp.sql` takes raw Snowplow data and casts/renames columns.

**3. Intermediate**
**Location:** `models/intermediate`

**Purpose:**
The "Engine Room" of the project. This is where complex business logic lives but is not yet exposed to the end-user.
* **Heavy Lifting**: Joins, aggregations, complex CASE statements.
* **Modular**: Break down big problems into smaller, reusable chunks.
* **Ephemeral**: Many models here are materialized as `ephemeral` to keep the warehouse clean.

**Convention:**
*   Prefix: `intermediate_`.
*   Example: `intermediate_abtest_assignments_sp.sql` calculates which user was in which A/B test group for a session.

**4. Core (Marts)**
**Location:** `models/core`

**Purpose:**
The "Showroom". These tables are consumer-facing, polished, and strictly modeled (Facts & Dimensions).
* **Facts**: Measurable events (Page Views, Orders).
* **Dimensions**: Context (Pages, Users).
* **Performance**: Highly optimized (partitioned, clustered) for BI tools (Looker).
* **Documentation**: Every column must be described.

**Convention:**
*   Prefix: `core_fact_` or `core_dim_`.
*   Strict adherence to Star Schema.


**Naming Conventions**
* **Suffixes**:
    *   `_sk`: Surrogate Key (join key).
    *   `_id`: Business Key (from source).
    *   `_timestamp`: UTC timestamp.
    *   `_at`: Timestamp (often local).
    *   `_date`: Date type.

## Processing / Data Consumer

## Orchestration

# Monitoring

---
created: 2026-01-11
last_edited: 2026-04-24
tags:
  - modern-data-stack
  - dbt
  - bigquery
  - looker
  - bi-engine
  - denormalization
  - pre-aggregation
  - performance-optimization
  - data-modelling
connections:
  - "[[001 Knowledge Base/Data/_index|Data Index]]"
  - "[[03a BQ Optimization Guide]]"
  - "[[04 Materialization and Storage]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - Data
  - Data Platform Design
---
## Introduction
The challenges faced in the `<client-dbt-project>` project are common in high-volume "Modern Data Stack" implementations. This module outlines how to move beyond classical modeling to solve performance and accessibility issues.

## 1. OBT (One Big Table) for BI Performance
**The Move**: Instead of keeping Facts and Dimensions separate in the final Mart, **denormalize everything** into a single, wide table.

* **Why?**: Columnar databases like BigQuery are much faster at scanning one massive table than joining 10 smaller ones.
* **For Looker**: By joining the `page_views` to the `page` and `session` dimensions *during the ETL process*, you eliminate the 10-join overhead at dashboard runtime.
* **Benefit**: Self-service analysts can drag and drop columns from a single table without needing to handle join logic or historical mapping (SCD2).

## 2. Pre-Aggregation (Rollups)
**The Move**: Stop serving raw events as the "Core" for everyone. Create a tiered access system.

* **Fact (Granular)**: Keep for Data Scientists / Power Users. (TB scale).
* **Rollup (Aggregated)**: Create daily/hourly sums by common dimensions (Country, Device, Ressort). (GB scale).
* **Benefit**: Users get the same answers as the Fact table, but the query scans 1/1000th of the data. Full rebuilds of Rollups are almost free.

## 3. Align with dbt Standards (The "Marts" Split)
**The Move**: Re-organize the directory structure to match the `staging` -> `intermediate` -> `marts` pattern.

| Old Layer | New Standard | Role |
| :--- | :--- | :--- |
| `import` | `sources` | Definitions in YAML. |
| `cleansing` | `staging` | 1:1 with sources, light cleaning. |
| `intermediate` | `intermediate` | Internal logic, pre-joins. |
| `core` | `marts` | OBT or Star Schema for exposure. |

* **Impact**: Better compatibility with dbt Cloud features (semantic layer, dbt mesh, cross-project refs).

## 4. Leverage BigQuery BI Engine
**The Move**: Allocate memory to the tables frequently used in Looker.

* **BigQuery BI Engine**: An in-memory analysis service. By defining "Reservations," BigQuery caches the most frequently used columns of your Fact tables in RAM.
* **Result**: Even complex joins can become sub-second. This uses allocated memory to improve runtime join latency.
## Summary: The "Service Level" Approach
Don't build one standard for everyone.
1.  **Self-service analysts**: Give them **OBT Aggregates** (Fast, Simple, Cheap).
2.  **Advanced users**: Give them **Star Schema Dimensions/Facts** (Flexible, Slightly Slower).
3.  **Data Engineers**: Own the **Staging & Heavy Logic** (Modular, DRY).

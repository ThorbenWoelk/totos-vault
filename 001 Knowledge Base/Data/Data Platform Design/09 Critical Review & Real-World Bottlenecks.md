---
created: 2026-01-18
last_edited: 2026-04-24
tags:
  - data-platform-design
  - performance-bottlenecks
  - full-refresh
  - runtime-joins
  - dbt-project-structure
  - dimensional-modeling
  - self-service-analytics
  - kimball-paradox
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
## Introduction
While the `<client-dbt-project>` project exhibits high engineering standards, real-world operation revealed several performance and organizational tradeoffs. These represent common scaling costs of classic dimensional modeling.

## 1. The Cost of "Full History"
**Observation**: Building the main tables from scratch across several years of TB-scale data can take days and require substantial cloud spend.
**The Issue**: The granularity of a "Single Tracking Event" at the core layer is computationally expensive.
* **Challenge**: Analysts may require event-level granularity for exploratory work. This can lead teams to keep billions of rows in the core layer.
* **Cost tradeoff**: While incremental runs can be cheap, any change in logic requiring a `--full-refresh` becomes a major rebuild. This raises the threshold for refactoring core models because full validation is expensive.

## 2. The "Runtime Join" Bottleneck
**Observation**: Joining Facts and Dimensions on every request from Looker can be too slow for interactive use. "Slice and dice at runtime" may not meet latency expectations at high volume.
* **Flexibility vs speed**: To give Looker users full flexibility, a team may avoid pre-joining data. Joining a 10-billion-row Fact table to 10+ Dimensions at dashboard runtime can exceed acceptable response times.
* **Freshness requirement**: If the business requires fully current data, static aggregate tables or persistent derived tables (PDTs) may be difficult to use. Each dashboard interaction can then re-trigger heavy joins.

## 3. Organizational Alignment & dbt Standards
**Observation**: The `import/cleansing/intermediate/core` structure felt disconnected from evolving industry standards.
**The Issue**: dbt Cloud and the dbt Labs team introduced a new standard project structure: `staging`, `intermediate`, `marts`.
* **Alignment**: The project structure used here is a "cousin" of the official standard, but not a sibling. This makes onboarding new dbt-savvy engineers slightly more confusing and makes it harder to leverage newer dbt features (like dbt Mesh) which assume the `staging/marts` pattern.

## 4. The "Kimball Paradox"
**Observation**: Classical Dimensional Modeling felt outdated and inaccessible.
**The Issue**: Star Schemas require join knowledge that self-service users may not have.
* **Complex joins**: Asking an analyst to join a Fact table to an SCD2 Dimension with timestamp logic increases the risk of incorrect numbers.
* **Barrier to entry**: Instead of making data accessible on its own, the modeled layer may require a translation layer from Data Engineers.

## Summary of Tradeoffs
| Tradeoff | Root Cause | Result |
| :--- | :--- | :--- |
| **Rebuild Cost** | High granularity + TB scale | Higher refactoring threshold; higher cloud spend. |
| **Looker Slowdown** | Runtime joins for freshness | Weaker user adoption and experience. |
| **Tooling Drift** | Non-standard naming | Increased maintenance overhead. |
| **Self-service gap** | Kimball complexity | Lower self-service success. |

---
created: 2025-12-19
last_edited: 2026-04-24
tags:
- data-platform
- data-architecture
- ingestion
- elt
- lakehouse
- data-warehouse
- data-lake
- data-engineering
connections:
- '[[001 Knowledge Base/Data/_index|Data Index]]'
- '[[01 Architecture and Layering]]'
- '[[11 Iceberg vs Delta Lake]]'
ai_generated: false
human_approved: false
category:
- Data
---
# Data Platform Engineering
## Architecture

Clean separation of concerns between ingestion (extract and load data), transformation (T in ELT), and compute/processing/data consumers.

Choice between three general architectures:
- Data warehouse centric
- Lakehouse design
- Data lake

Following one of these paradigms is important to make consistent decisions later on. For instance, if you're dwh-centric, you shouldn't see data transformation in the blob storage landing area. However, in practice, usually each implementation lends some properties from the other paradigms as well.

### Ingestion
Choose push/pull/real-time ingestion based on source and contractual agreements possible or not with provider. For instance, a third-party pushing data regularly is generally a good thing in terms of low maintenance costs. However, decent monitoring and SLAs should be agreed upon with the data provider. Generally real-time event-based ingestion is to be preferred. However, this comes with processing (more granular event level, more processing in load as well as data modeling) and increased complexity costs (e.g. potentially invalid aggregates due to lost events). Generally a good idea to check against batch-imported aggregates to sanity check. Single point of truth and tolerance level for discrepancies should be defined.

#### Technologies
Google Pub/Sub, Python (byo, dlt), AWS Glue, Dataflow (Apache Beam), Azure Data Factory, Databricks Lakeflow


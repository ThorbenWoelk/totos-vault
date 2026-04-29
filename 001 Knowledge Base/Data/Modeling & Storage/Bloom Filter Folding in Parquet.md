---
created: 2026-04-19
last_edited: 2026-04-29
tags:
- observability
- parquet
- bloom-filter
- data-engineering
- performance
- logfire
- apache-arrow
- rust
connections:
- '[[Everything is a Tradeoff]]'
- '[[Software Design Principles - Comprehensive Guide]]'
ai_generated: true
human_approved: false
category:
- Knowledge Base
- Data
- Modeling & Storage
source_url: https://pydantic.dev/articles/bloom-filter-folding-parquet-logfire?utm_source=linkedin
source_type: article
---
## Overview
Bloom filter folding is an Apache Arrow Rust Parquet optimization used by Pydantic Logfire to make sparse point lookups faster while reducing Bloom filter storage overhead.

The technique starts with a conservatively large Split Block Bloom Filter, writes the row group, then folds adjacent blocks together until the filter reaches a target false-positive probability. This removes the need to estimate distinct-value counts before the data is known.

## What Bloom Filters Do
A Bloom filter answers whether a value may exist in a group of rows. A negative answer is definitive, so the query engine can skip the row group without reading the data. A positive answer is probabilistic and may still require reading.

This matters for observability queries such as:

- `span_name = 'tool called'`
- `trace_id = '...'`
- high-cardinality user, span, trace, or attribute lookups

For selective point lookups, effective filters can skip most row groups and sharply reduce I/O.

## Sizing Problem
Bloom filters need to be sized against the expected number of distinct values. That value is often unknown at write time.

Common strategies are imperfect:

- fixed-size filters are simple but can be too large or saturated
- sampling-based estimators add read and compute overhead
- multiple candidate filters increase write-time memory and CPU cost

The failure modes are practical: high-cardinality columns can saturate a small filter, while low-cardinality columns can waste space with an oversized filter.

## Folding Technique
Bloom filter folding changes the flow:

1. Allocate a conservatively large Split Block Bloom Filter.
2. Populate it as the row group is written.
3. Merge adjacent blocks with bitwise OR.
4. Repeat until the target false-positive probability is reached.

Because Split Block Bloom Filters hash values into blocks, folding preserves the no-false-negatives guarantee. The filter gets smaller, but it still never rejects a value that was present.

## Logfire Impact
Logfire stores traces, spans, and logs in Parquet. Folded Bloom filters help Logfire by:

- skipping more irrelevant row groups for point lookups
- improving queries on `span_name`, `trace_id`, and attribute columns
- reducing per-file Bloom filter overhead
- keeping high-cardinality filters useful instead of saturated
- removing per-customer tuning of Bloom filter sizes

The article describes the write-time overhead as about 1 MiB of memory per Bloom filter, which is small compared with typical write buffers.

## Ecosystem Impact
The optimization was contributed upstream to Apache Arrow instead of kept in a Logfire-specific fork. Projects using the Rust Parquet implementation, including DataFusion and Polars users through the shared ecosystem, can benefit as the change propagates.

The broader lesson fits [[Everything is a Tradeoff]]: spend a small amount of write-time memory to reduce read-time I/O and storage waste. It also fits [[Software Design Principles - Comprehensive Guide]] because a better default removes a tuning burden from users.

## References

- Garcia Badaracco, A. (2026, April 15). *[Bloom filter folding in Parquet: Faster point lookups for observability](https://pydantic.dev/articles/bloom-filter-folding-parquet-logfire?utm_source=linkedin).* Pydantic Logfire.

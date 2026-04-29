---
created: 2026-04-19
last_edited: 2026-04-24
tags:
  - bloom-filter
  - parquet
  - observability
  - data-structures
  - query-optimization
  - pydantic
  - logfire
  - apache-arrow
  - data-engineering
connections:
  - "[[000 raw/_index|Raw Source Index]]"
  - "[[Bloom Filter Folding in Parquet]]"
  - "[[Databricks Data Engineer Professional]]"
ai_generated: false
human_approved: false
category:
  - raw
  - articles
compile_target: totos-vault/001 Knowledge Base/Data/Modeling & Storage/Bloom Filter Folding in Parquet.md
compiled: true
ingested_at: 2026-04-19
source_type: article
source_url: https://pydantic.dev/articles/bloom-filter-folding-parquet-logfire?utm_source=linkedin
tagging_processed_count: 1
---
Bloom filter folding in Parquet: faster point lookups for observability | Pydantic Logfire Logfire Gateway Pydantic AI Pricing About Careers Blog Case Studies Pydantic AI + Logfire + Evals Pydantic Logfire + Evals Pydantic AI + Logfire Pydantic AI Docs Pydantic Validation Pydantic Logfire Pydantic AI Get in touch Get in touch Log in / Pydantic Logfire Smarter bloom filters in Parquet: how filter folding speeds up point lookups in Logfire Adrian Garcia Badaracco 5 mins 2026/04/15 If you've used Logfire to search for a sparse equality filter, say span_name = 'tool called' — you're doing a point lookup type query. A point lookup is a query that matches handful of rows out of potentially billions. The faster we can skip irrelevant data, the faster your query returns, filtering is the bottleneck for these sorts of queries.
Bloom filters are one of the key mechanisms that make this possible in Parquet. We recently contributed an optimization to Apache Arrow that makes bloom filters both smaller and more effective. Here's what that means for Logfire.
# What bloom filters do
A bloom filter is a compact data structure that answers one question: "is this value in this group of rows?" The answer is either "definitely no" or "probably yes." There are no false negatives — if the filter says no, you can skip the entire row group without reading it.
When you query span_name = 'tool called' , the query engine checks the bloom filter for each row group before reading any data. If the filter says your value isn't there, the row group is skipped entirely. For selective queries on high-cardinality columns — trace IDs, span names, user IDs — this can mean skipping 99% of the data.
Bloom filters are part of the Parquet spec and supported by most Parquet systems out there. DuckDB recently showed 50x speedups on point lookups using bloom filters, and InfluxData measured up to 30x improvement. The gains are real.
# The problem: sizing bloom filters without knowing the data
Here's the catch. When you create a bloom filter, you need to decide how big it should be. Too small and it fills up with false positives, becoming useless — it says "probably yes" to everything. Too large and you waste space in every file.
The size depends on how many distinct values (NDV) you expect. But in practice, you rarely know that upfront. A row group might contain 10 distinct span names or 100,000 distinct trace IDs. Some of the strategies that systems use are:
Use a fixed guess : this is what the Rust Parquet implementation arrow-rs did before our contribution. It used a fixed size that worked okay for many cases but was often wrong, leading to either bloated filters or useless ones.
Use a sampling-based estimator : some systems sample the data to estimate NDV before. DuckDB does this. This adds some overhead and complexity.
Create multiple sizes bloom filters : some systems create multiple filters of different sizes and pick the best one after the fact. This also adds overhead and complexity at write time.
This created real problems for us. High-cardinality columns like trace IDs would saturate the filter in some cases, rendering it ineffective. In other cases we'd end up with filters that were much larger than they needed to be, wasting space across billions of rows. Since this all depends on how each users data is distributed, there was no one-size-fits-all configuration we could use.
# The fix: fold first, ask questions later
The optimization we contributed is called bloom filter folding . Instead of guessing the right size upfront, we start with a conservatively large filter and shrink it after all the data has been written.
The folding operation works by merging adjacent pairs of blocks in the filter using bitwise OR, halving its size with each fold. Because of how the Split Block Bloom Filter (SBBF) hashes values into blocks, this is mathematically sound — no false negatives are introduced.
After writing all the values, the filter folds itself down until it reaches a target false positive probability. The result is a filter that is optimally sized for the actual data, not a guess.
In practice this means:
Filters for low-cardinality columns shrink dramatically — a column with 50 distinct values doesn't need a filter sized for a million
Filters for high-cardinality columns stay effective — instead of being saturated and useless, they fold to a size that maintains the target false positive rate
No configuration required — the new behavior is the default; you don't need to estimate NDV or tune parameters
# What this means for Logfire
Logfire stores observability data — traces, spans, logs — in Parquet. Many of the most common queries are point lookups: find a specific trace by ID, filter spans by name, search for a particular attribute value.
With folded bloom filters:
Faster point lookups : queries like span_name = 'tool called' or trace_id = '...' skip more row groups, reading less data.
Smaller files : right-sized filters waste less space, which adds up across the billions of rows we store.
Better defaults : we won't need to guess or tune bloom filter sizes for different columns or different customers.
Minimal write time overhead : the only price we pay is ~1MiB of memory per bloom filter during write time. This is negligible compared to the size of the data and other write buffers.
This builds on top of the dynamic shredding work we shipped earlier this year. Shredding made attribute queries fast by giving them dedicated columns; bloom filters make point lookups on those columns even faster by skipping row groups entirely.
# Community contribution
We contributed this directly to Apache Arrow rather than maintaining a fork. This means every project that uses the Rust Parquet implementation — not just Logfire — benefits from smarter bloom filters. The Parquet Java maintainers have already expressed interest in porting the approach to other implementations, so there is potential for this to become a standard part of the Parquet ecosystem.
Want to see this in action? Get started with Pydantic Logfire and run a point lookup on your trace data. Related content View all articles / Pydantic Logfire Vercel AI SDK and Logfire: what happens when everyone speaks OpenTelemetry Petyo Ivanov 2026/04/14 / Pydantic Logfire Full-stack Kubernetes observability with Logfire Nicola Martino 2026/04/10 Explore Logfire Get started Get started Explore our open source packages Footer( &apos; /logfire &apos; &apos; /pydantic-ai &apos; &apos; /gateway &apos; &apos; /pydantic &apos; &apos; /about &apos; &apos; /privacy &apos; &apos; /terms &apos; &apos; /cookies &apos; &apos; /casestudies &apos; &apos; /blog &apos; &apos; /opensource &apos; &apos; /contact &apos; &apos; /pricing &apos; &apos; /enterprise &apos; &apos; /careers &apos; &apos; /alternatives &apos; &apos; /vs-langsmith &apos; &apos; /vs-langfuse &apos; &apos; /vs-arize &apos; libraries =[ &apos; /logfire &apos;, &apos; /pydantic-ai &apos;, &apos; /gateway &apos;, &apos; /pydantic &apos; ], company =[ &apos; /about &apos;, &apos; /privacy &apos;, &apos; /terms &apos;, &apos; /cookies &apos; ], resources =[ &apos; /casestudies &apos;, &apos; /blog &apos;, &apos; /opensource &apos;, &apos; /contact &apos; ], more =[ &apos; /pricing &apos;, &apos; /enterprise &apos;, &apos; /careers &apos; ], compare =[ &apos; /alternatives &apos;, &apos; /vs-langsmith &apos;, &apos; /vs-langfuse &apos;, &apos; /vs-arize &apos; ], ) The Pydantic Stack Sign up for updates on Pydantic AI, Pydantic Logfire, and Pydantic Evals. Email Subscribe Subscribe

_Image localization requested: yes_


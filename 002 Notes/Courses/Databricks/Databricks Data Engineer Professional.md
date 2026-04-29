---
created: 2026-02-26
last_edited: 2026-04-24
tags:
- databricks
- delta-lake
- data-engineering
- spark
- etl
- streaming
- cdc
- optimization
- data-modeling
connections:
- '[[Databricks Data Engineer Associate]]'
- '[[11 Iceberg vs Delta Lake]]'
- '[[Data Platform Architecture]]'
ai_generated: false
human_approved: false
category:
- Notes
- Courses
- Databricks
tagging_processed_count: 1
---
# Delta Tables

## Data Skipping

Dynamic file pruning, can significantly improve the performance of many queries on Delta Lake tables. Dynamic file pruning triggers for queries that contain filter statements or `WHERE` clauses. You must use Photon-enabled compute to use dynamic file pruning in `MERGE`, `UPDATE`, and `DELETE` statements. Only `SELECT` statements leverage dynamic file pruning when Photon is not used.

|                           |                                          |                               |
| ------------------------- | ---------------------------------------- | ----------------------------- |
| Per Partition             | Per File (Delta Lake on Databricks only) |                               |
| Static (based on filters) | Partition Pruning                        | File Pruning                  |
| Dynamic (based on joins)  | _Dynamic_ Partition Pruning              | _Dynamic_ File Pruning (NEW!) |

## CONSTRAINTS

> [!WARNING]
> Don't confuse with SDP EXPECTATIONS!


**ALTER TABLE t ADD CONSTRAINT c CHECK (<bool_exp>);**

If one entry of an INSERT INTO query fails, nothing is inserted. Atomic update = entire set of values in query. 

## MERGE INTO

### Non-ambiguity

Merge operation can not be performed if multiple source rows matched and attempted to modify the same target row in the table. The result may be ambiguous as it is unclear which source row should be used to update or delete the matching target row.
For such an issue, you need to preprocess the source table to eliminate the possibility of multiple matches.

### INSERT ONLY MERGE

```
MERGE INTO logs  
USING newDedupedLogs  
ON logs.uniqueId = newDedupedLogs.uniqueId  
WHEN NOT MATCHED  
THEN INSERT *
```

## AUTO CDC (formerly APPLY CHANGES INTO)

### Syntax
```python
dp.create_auto_cdc_flow(  
	source=employees_cdf_table,  
	target=f"{catalog}.{schema}.{employees_table_current}",  
	keys=["id"],  
	sequence_by=col("sequenceNum"),  
	apply_as_deletes=expr("operation = 'DELETE'"),  
	except_column_list = ["operation", "sequenceNum"],  
	stored_as_scd_type = 1  
)
```


### Optimizing
Enabling deletion vectors allows Delta to efficiently track and manage rows that are deleted or updated without requiring full rewrites of the underlying files, which significantly reduces the overhead for CDC operations. Applying liquid clustering on the CDC merging keys organizes the data physically based on these keys, ensuring that related records are colocated and minimizing the amount of data scanned during updates and deletions. Together, these optimizations help maintain high ingestion and query performance, reduce latency for CDC workloads, and make the table more manageable at scale.

## Vacuum
7 days is the default retention period for Delta Lake table history before VACUUM removes old files

## Multiple CDC changes in one batch

SEQUENCE BY works with AUTO CDC and APPLY CHANGES INTO.
It does not work with MERGE INTO. 

MERGE INTO fails on multiple changes to same row: `Cannot perform MERGE as multiple source rows matched and attempted to modify the same target row`
## CDF

### When to use
![[Pasted image 20260228200952.png]]

### VACUUM
The files in the \_change\_data_ folder follow the retention policy of the table. Therefore, if you run the VACUUM command, change data feed data is also deleted.


## Row Filters

```
CREATE FUNCTION fr_filter(region STRING)
RETURN IF(IS_ACCOUNT_GROUP_MEMBER('hr_team'), true, region='FR');
   
ALTER TABLE employees SET ROW FILTER fr_filter ON (region);
```

## Aggregates

### Stateful (streaming) aggregates
You must use watermarks when computing stateful aggregates. Omitting a watermark from a stateful aggregate query results in state information building up infinitely over time. This results in processing slowdowns and can lead to out-of-memory errors.

You should not use a stateful aggregate to calculate statistics over an entire dataset. Databricks recommends using materialized views for incremental aggregate calculation on an entire dataset.

### Incremental updates
Materialized views automatically track changes in the data source and apply appropriate updates to aggregate values on refresh.

## Cloning

```SQL
CREATE OR REPLACE TABLE orders_archive
DEEP CLONE orders
```
  
Cloning can occur incrementally. Executing the `CREATE OR REPLACE TABLE` command can sync changes from the source to the target location.

Now, If you run `DESCRIBE HISTORY orders_archive`, you will see a new version of CLONE operation occurred on the table.

## Time Travel
Delta Lake’s default transaction log retention period is 30 days. If you want to keep deleted files the same period: 
`ALTER TABLE x SET TBLPROPERTIES ('delta.deletedFileRetentionDuration' = 'interval 30 days')

## Sharing

### Protocols
The Databricks-to-Databricks sharing protocol is the only Delta Sharing implementation that supports sharing not just static Delta tables, but also additional Unity Catalog assets such as Volumes, Models, and Notebooks. This protocol extends the open Delta Sharing standard, enabling seamless collaboration and governance between Databricks workspaces and organizations within the Databricks ecosystem.

In contrast, the open Delta Sharing protocol and customer-managed implementations of the open-source Delta Sharing server are limited to sharing static Delta tables only. They do not support Unity Catalog assets or Databricks-native objects beyond data tables.

### WITH HISTORY
Allows CDF & time travel. Automatically exposes the complete table directory, enabling CDF consumption and historical queries.


# Spark

## Pandas UDFs

**Use Apache Arrow**
which provides an efficient columnar in-memory data format that allows Spark to transfer data between the JVM and Python processes without serialization overhead. This significantly speeds up data processing compared to standard row-based formats.

## Hashing 

In Apache Spark, the `sha2(expression, bitLength)` returns a checksum of the SHA-2 family as a hex string of an expression. It only supports specific SHA-2 bit lengths: 224, 256, 384, and 512. Any other bit length, such as 128, is invalid and would cause the function to fail. Notice that bitLength 0 is equivalent to 256.

## Structured Streaming

### Window limitations

Non-time-based window operations are not supported on streaming DataFrames.
Instead, these window operations need to be implemented inside a foreachBatch logic.

### Deletes on streaming source
If you are using a table as a streaming source, deleting data breaks the append-only requirement of streaming sources, which makes the table no more streamable. To avoid this, you can use the ignoreDeletes option when streaming from this table. This option enables streaming processing from Delta tables with partition deletes.

`option("ignoreDeletes", True)`

## Optimizer

### Dynamic File Pruning
Dynamic file pruning in Apache Spark is an optimization technique that skips reading irrelevant data files during query execution by leveraging runtime filter information, which allows Spark to avoid scanning files that do not match the query predicates, thereby improving performance and reducing I/O.

Dynamic file pruning triggers for queries that contain filter statements or `WHERE` clauses. You must use Photon-enabled compute to use dynamic file pruning in `MERGE`, `UPDATE`, and `DELETE` statements. Only `SELECT` statements leverage dynamic file pruning when Photon is not used.

# Lakeflow Spark Declarative Pipelines

## Flows
**SDP**
Append - streaming
Materialized view - batch

**LDP ONLY**
AUTO CDC - streaming

## Targets
A **streaming table** can have one or more streaming flows written into it.
**Materialized views** differ from streaming tables in that you always define the flows implicitly as part of the materialized view definition.

## Sinks
supports Delta tables, Apache Kafka topics, Azure EventHubs topics, and custom Python data sources.

## Pipeline Development Tools

### Dev mode
When you run your LSDP pipeline in development mode, the system does the following:
- Reuses a cluster to avoid the overhead of restarts. By default, clusters run for two hours when development mode is enabled. You can change this with the `pipelines.clusterShutdown.delay` setting in the [Configure classic compute for pipelines](https://docs.databricks.com/aws/en/ldp/configure-compute).
- Disables pipeline retries so you can immediately detect and fix errors.
# Databricks

## Jobs

## Ownership

A job cannot have a group as an owner.

## Notebooks

### From scripts
You can convert Python, SQL, Scala, and R scripts to single-cell notebooks by adding a comment to the first line of the file:
`# Databricks notebook source`

## Cluster Permissions

![[Pasted image 20260302130152.png]]

## Cluster Debugging

### Issue: Exploding joins
If you see a few rows going into a node and magnitudes more coming out, you may be suffering from an exploding join or explode().

### Issue: One spark task
If you see a long-running stage with just one task, that's likely a sign of a problem. While this one task is running only one CPU is utilized and the rest of the cluster may be idle. This happens most frequently in the following situations:

- Expensive [UDF](https://docs.databricks.com/aws/en/udf/) on small data
- [Window function](https://docs.databricks.com/aws/en/sql/language-manual/sql-ref-window-functions) without `PARTITION BY` statement
- Reading from an unsplittable file type. This means the file cannot be read in multiple parts, so you end up with one big task. Gzip is an example of an unsplittable file type.
- Setting the `multiLine` option when reading a JSON or CSV file
- Schema inference of a large file
- Use of repartition(1) or coalesce(1)

### Issue: high I/O

How much data needs to be in an I/O column to be considered high? To figure this out, first start with the highest number in any of the given columns. Then consider the total number of CPU cores you have across all our workers. Generally each core can read and write about 3 MBs per second.

Divide your biggest I/O column by the number of cluster worker cores, then divide that by duration seconds. If the result is around 3 MB, then you're probably I/O bound. That would be high I/O.

Speeding up your reads:

- Use [Delta](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#lakehouse-format).
- Use liquid clustering for better [data skipping](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#data-skipping). See [Use liquid clustering for tables](https://docs.databricks.com/aws/en/delta/clustering).
- Try [Photon](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#photon). It can help a lot with read speed, especially for wide tables.
- Make your query more selective so it doesn't need to read as much data.
- [Reconsider your data layout](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#data-layout) so that [data skipping](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#data-skipping) is more effective.
- If you're reading the same data multiple times, use the [Delta cache](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#delta-cache).
- If you're doing a join, consider trying to get [DFP](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#dynamic-file) working.
- Increase the [size of your cluster](https://docs.databricks.com/aws/en/compute/cluster-config-best-practices#cluster-sizing) or use [serverless compute](https://docs.databricks.com/aws/en/compute/serverless/).

Speeding up writes:
- Are you rewriting a lot of data? See [How to determine if Spark is rewriting data](https://docs.databricks.com/aws/en/optimizations/spark-ui-guide/spark-rewriting-data) to check. If you are rewriting a lot of data:
    - See if you have a [merge that needs to be optimized](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#delta-merge).
    - Use [deletion vectors](https://docs.databricks.com/aws/en/delta/deletion-vectors) to mark existing rows as removed or changed without rewriting the Parquet file.
- Enable [Photon](https://www.databricks.com/discover/pages/optimize-data-workloads-guide#photon) if it isn't already. Photon can help a lot with write speed.
- Increase the [size of your cluster](https://docs.databricks.com/aws/en/compute/cluster-config-best-practices#cluster-sizing) or use [serverless compute](https://docs.databricks.com/aws/en/compute/serverless/).
# Unity Catalog

## Predictive Optimization
Predictive Optimization in Databricks Unity Catalog automatically optimizes managed tables in Unity Catalog by:
- Running background maintenance tasks like VACUUM, OPTIMIZE, and ANALYZE to reduce fragmentation and improve performance.
- Collecting table statistics during writes, which helps the query optimizer make better decisions and improve query speed.

## Foreign catalogs

Lakehouse Federation in Databricks allows Unity Catalog to register external data sources like MySQL as foreign catalogs.


---
created: 2026-02-19
last_edited: 2026-04-24
tags:
- databricks
- data-engineering
- delta-lake
- spark
- etl
- data-governance
- lakehouse
- notebooks
- cloud-data
- best-practices
connections:
- '[[Databricks Data Engineer Professional]]'
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
# Databricks Notebooks

## Limitations
- Table results from NB cell are limited to 10,000 rows or 2 MB, whichever is lower.
- Job clusters have a maximum notebook output size of 30 MB.

# Delta Table

## OPTIMIZE
Compacting small files into larger ones. 
`OPTIMIZE` is idempotent. Once files have already been compacted and no new data has been added or modified, re-running `OPTIMIZE` does not trigger any further changes.

# SQL

## INSERT
- **`INTO` or `OVERWRITE`**
    `OVERWRITE`:
    - Without a `partition_spec` the table is truncated before inserting the first row.
    - Otherwise, all partitions matching the `partition_spec` are truncated before inserting the first row.
    `INTO`: 
    - all rows inserted are additive to the existing rows.

## UNION

With `UNION`, you can return the result of subquery1 plus the rows of subquery2

```
subquery1 
UNION [ ALL | **DISTINCT** ]
subquery2
```

- If `ALL` is specified duplicate rows are preserved.
- If `DISTINCT` is specified the result does not contain any duplicate rows. **This is the default.**

## filter

`filter(input_array, lamda_function)` is a higher order function that returns an output array from an input array by extracting elements for which the predicate of a lambda function holds.

`SELECT filter(array(1, 2, 3, 4), i -> i % 2 == 1);`
--> [1, 3]

# Spark

## Spark Connect vs Classic
Spark connect executes schema analysis lazy instead of eagerly at execution planning time. 


## AutoLoader

### .option("cloudFiles.schemaEvolutionMode", "...")

| Mode                      | Behavior on reading new column                                                                                                                                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `addNewColumns` (default) | Stream fails. New columns are added to the schema. Existing columns do not evolve data types.                                                                                                                                   |
| `rescue`                  | Schema is never evolved and stream does not fail due to schema changes. All new columns are recorded in the [rescued data column](https://docs.databricks.com/aws/en/ingestion/cloud-object-storage/auto-loader/schema#rescue). |
| `failOnNewColumns`        | Stream fails. Stream does not restart unless the provided schema is updated, or the offending data file is removed.                                                                                                             |
| `none`                    | Does not evolve the schema, new columns are ignored, and data is not rescued unless the `rescuedDataColumn` option is set. Stream does not fail due to schema changes.                                                          |

## Lakeflow Spark Declarative Pipelines

### Expectations
| Action                                                                       | SQL syntax                            | Python syntax       | Result                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------- | ------------------------------------- | ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [warn](https://docs.databricks.com/aws/en/ldp/expectations#retain) (default) | `EXPECT`                              | `dp.expect`         | Invalid records are written to the target.                                                                                                                                                                           |
| [drop](https://docs.databricks.com/aws/en/ldp/expectations#drop)             | `EXPECT ... ON VIOLATION DROP ROW`    | `dp.expect_or_drop` | Invalid records are dropped before data is written to the target. The count of dropped records is logged alongside other dataset metrics.                                                                            |
| [fail](https://docs.databricks.com/aws/en/ldp/expectations#fail)             | `EXPECT ... ON VIOLATION FAIL UPDATE` | `dp.expect_or_fail` | Invalid records prevent the update from succeeding. Manual intervention is required before reprocessing. This expectation causes a failure of a single flow and does not cause other flows in your pipeline to fail. |

# Platform Services

## App Logs

Access application logs in the following ways:

- **Apps UI:** On the app details page, click the **Logs** tab to view standard output and error. For details, see [View the details for a Databricks app](https://docs.databricks.com/aws/en/dev-tools/databricks-apps/view-app-details).
- **Direct URL:** Append `/logz` to your app URL. For example, if your app URL is `https://my-app-1234567890.my-instance.databricksapps.com`, logs are available at `https://my-app-1234567890.my-instance.databricksapps.com/logz`.

## Avoid egress costs
Cloudflare R2 object storage incurs no egress fees.
- **Ridiculously Reliable:** R2 is designed for high durability and resilience, providing 99.999999999% (eleven 9's) annual durability, equivalent to other major providers.
- **Radically Reprogrammable:** Fully integrated with the [Cloudflare Workers serverless runtime](https://blog.cloudflare.com/introducing-r2-object-storage/), it allows for dynamic content handling.
- **S3 Alternative (R before S, 2 before 3):** It is often seen as a competitor to Amazon's S3 (Simple Storage Service), positioning R2 as a modern, lower-cost, and more flexible alternative.
- **Zero-Egress Fee Focus:** R2 was developed to eliminate expensive data transfer charges, allowing users to move data freely without penalizing egress fees

## Delta Sharing

### Protocols
#### databricks to databricks

#### open-sharing protocol
Delta Sharing is designed to securely share data across platforms using an open protocol. Since the vendor does not use Databricks, Delta Sharing ensures secure, real-time access without manual exports or third-party workarounds.

## Lakehouse Federation
Allows databricks to run queries on external dbs. 

*Query* federation executes query in databricks AND in source db.
Example: PostgreSQL federation. 

*Catalog* federation only executes query in databricks only.
Example: Snowflake federation. 

Both are powered by unity catalog.

## Liquid Clustering

Liquid clustering is a data layout optimization technique that replaces table partitioning and `ZORDER`. It simplifies table management and optimizes query performance by automatically organizing data based on clustering keys.

Unlike traditional partitioning, you can redefine clustering keys without rewriting existing data. This allows your data layout to evolve alongside changing analytic needs. Liquid clustering applies to both streaming tables and materialized views.

## Logs

### Audit Logs
Audit logs are JSON events. 
The serviceName and actionName properties identify the event. 

### Cluster Logs
Folders:
- **driver**: Logs from the Spark driver node, which we are most interested in.
- **executor**: Logs from each Spark executor in the cluster.
- **eventlog**: logs of events you can find in the “Event Log” tab of the cluster, such as cluster starting, cluster terminating, resizing, etc.
- **init_scripts**: This folder is generated if the cluster has init scripts

log stream output buffers:
- **stdout**: Standard Output buffer from the driver node’s JVM. This is where `print()` statements and general output are written by default.
- **stderr**: Standard Error buffer from the driver node’s JVM. This is typically where exceptions/stacktraces are written, and many logging libraries also default to stderr.
- **log4j**: Specifically filtered to log messages written with a log4j logger. You may see these in messages in stderr as well.

`logger = PySparkLogger.getLogger()`

### Automatic Liquid Clustering
Based on **predictive optimization** from analyzing **query workload**. 
Databricks changes clustering keys only when the predicted cost savings from data skipping improvements outweigh the data clustering cost.

## Git
The following tasks are not supported by Databricks Repos, and must be performed in your Git provider:
- Create a pull request
- Delete branches

## SQL Warehouses

### Types

|Warehouse type|Photon Engine|Predictive IO|Intelligent Workload Management|
|---|---|---|---|
|Serverless|✓|✓|✓|
|Pro|✓|✓||
|Classic|✓|||
Pro deploys to Cloud and allows private VPC.

### Auto-Stop
Automatically shut down the SQL warehouse after it has been idle for a specified period.

## Resources

## Memory optimized
For Spark joins, particularly when dealing with large datasets or complex join conditions. This is due to the need to shuffle and potentially store data in memory during the join operation.
Ensures no disk spills, thereby significantly reducing shuffle overhead and improving performance.

# Data Engineering Problems

## Data ingestion

## Databricks/Lakeflow Jobs

### Task-level notifs
Databricks Jobs allow notifications at the task level, enabling targeted alerts when a specific task fails. The engineer can configure the alert only for the final task and direct it to Slack, ensuring other task failures do not trigger notifications.

## read_files()

Use path globs for **where to look** (path structure). 
Use pathGlobFilter for **which files to keep** (name/suffix filtering). Does **not** change partition discovery behavior.

### input_file_name()
Returns the name of the file being read, or empty string if not available.

## Skewed JOINS: small subgroup in A matches massive part B

### Understanding the Data Skew Problem

In the context of the question, data skew is occurring because a small handful of users (specific `user_id`s) are generating millions of clickstream events, while the vast majority of users generate only a tiny fraction of that.

During a standard distributed join, Spark groups data by the join key. It attempts to send all events with the same `user_id` to a single partition on a single worker node (a process called a "shuffle"). Consequently, the worker node tasked with processing the high-frequency user is overwhelmed by gigabytes of data and runs out of memory or gets stuck, while other worker nodes finish their smaller tasks quickly and sit idle.

### Why is "Broadcasting skewed keys" the WRONG approach?

Broadcasting (specifically executing a **Broadcast Hash Join**) is a powerful optimization in Databricks designed to bypass the expensive shuffling phase of a join. However, it operates on a very strict premise: **You only broadcast the small dataset.**

Here is what happens under the hood when a broadcast join is initiated: Spark takes the dataset to be broadcasted and copies it entirely into the RAM of the driver node, and then sends a full copy to the memory of _every single executor node_ in the cluster.

If you attempt to broadcast the "skewed keys" (which, in this context, refers to the massive amount of clickstream event data generated by those specific high-frequency users), you are essentially trying to take the bulkiest, heaviest part of your dataset and duplicate it across the entire cluster.

- **The Result:** This completely negates the benefit of a distributed system. It causes severe network congestion and dramatically increases memory pressure on each executor. It will almost certainly result in an `Out of Memory (OOM)` exception, crashing the job. You should broadcast a small lookup table to avoid shuffling a large table; you must never broadcast the large, skewed data itself.

### Why the other options ARE appropriate solutions:

Here is why the other three methods successfully mitigate data skew without crashing the cluster:

#### 1. Salting the skewed keys

Salting is the classic big data technique for addressing key-bound skew directly.

- **Mechanism:** You append a random integer or prefix to the skewed `user_id`s in your massive clickstream data (e.g., `userA` becomes `userA_1`, `userA_2`, `userA_3`). You then replicate the corresponding user profile row for `userA` to match those new suffixes.
- **Why it works:** When the network shuffle happens for the join, `userA`'s massive event log is no longer recognized as a single key. It is split into multiple distinct sub-keys. This allows Spark to successfully distribute the workload for that single heavy user across multiple worker nodes in parallel, eliminating the bottleneck.

#### 2. Separate processing of skewed keys

This isolates the problematic data so it doesn't drag down the rest of the pipeline.

- **Mechanism:** You split your clickstream dataset into two distinct dataframes: one for "normal" users and one for "skewed" (high-frequency) users. You perform a standard merge on the normal users. For the high-frequency users, because there are only a handful of them, their _user profile_ records form a minuscule dataset. You can safely **broadcast just the user profiles** of those few users to join against their massive clickstream logs.
- **Why it works:** The skew is neutralized with targeted resources. Interestingly, Databricks' Adaptive Query Execution (AQE) often performs a variation of this skewed join optimization automatically under the hood when it detects skewed partitions at runtime.

#### 3. Repartitioning before the join

- **Mechanism:** Invoking `.repartition(N)` on the clickstream dataset forces Spark to redistribute the data across `N` partitions using a simple round-robin algorithm, completely ignoring the `user_id` distribution.
- **Why it works:** While this introduces an additional shuffle step before the join, it guarantees that the data entering the join phase is evenly distributed in terms of partition size. This brute-force load balancing ensures that no single worker node receives an egregiously large chunk of data at once, enhancing parallelism and masking the effects of the skew.

## Skewed JOINS: Broadcast small lookup table to avoid shuffle
### The "Shuffle" in a Standard Join

In a standard join (often a **Sort-Merge Join**), Spark needs to ensure that rows with the exact same join key (e.g., `user_id = 123`) from _both_ tables end up on the exact same worker node so they can be matched together.

Because the data for both tables is initially scattered randomly across all the worker nodes in the cluster, Spark has to physically move the data over the network to group it by the join key. This massive reorganization and network transfer of data is called a **shuffle**.

- Shuffles are highly resource-intensive. They involve reading from disk, serializing data, transferring it over the network, deserializing, and writing back to disk.
- If the large table has billions of rows, shuffling it means moving terabytes of data across the network.

### The Broadcast Hash Join Solution

A **Broadcast Hash Join** completely bypasses this shuffle phase by using a different strategy. It works under one specific condition: **one of the tables being joined must be small enough to fit completely into the memory (RAM) of a single worker node.**

Here is exactly how it avoids the shuffle:

**1. Replicating the Small Table (Broadcasting)** Instead of moving rows around based on the join key, Spark takes the entire "small" table (e.g., a lookup table of user profiles mapping `user_id` to `account_tier`) and sends a full, identical copy of it to the local memory of _every single worker node_ in the cluster.

**2. Local Processing of the Large Table** Because every worker node now has a complete copy of the small table, they never need to talk to each other to find matches. Each worker node simply looks at the slice (partition) of the **large table** (e.g., the billions of clickstream events) that it already happens to hold locally. For every row in its chunk of the large table, the worker does a quick lookup in the small table held in its own RAM.


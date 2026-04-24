---
created: 2026-04-16
last_edited: 2026-04-24
tags:
- database-concurrency
- transactions
- mvcc
- locking
- optimistic-concurrency
- crdt
- last-write-wins
connections:
- '[[SQL in Python]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- Data
---
## Core idea

When multiple users or processes try to change the same data at the same time, databases need concurrency control. Without it, systems can produce **lost updates**, where one write silently overwrites another write.

Concurrency strategies trade off latency, throughput, implementation complexity, and the risk of conflict handling failures.

## Multi-Version Concurrency Control

**MVCC** is the default strategy in many modern relational databases, including PostgreSQL, MySQL InnoDB, and Oracle.

Instead of making readers and writers wait on the same physical row, the database keeps multiple versions of a data item. A transaction reads from a snapshot of the database as it existed when the transaction started. If another transaction updates the same row during that time, the first transaction can keep reading the old version.

### Strength

- Readers do not block writers.
- Writers do not block readers.
- Read-heavy workloads can scale much better than with coarse locking.

### Cost

- The database must manage old row versions.
- Cleanup and vacuuming become important operational concerns.
- Write conflicts still need to be resolved according to the isolation level.

## Pessimistic Concurrency Control

Pessimistic control assumes conflicts are likely, so it prevents them with locks.

Common lock types:

- **Shared lock**: multiple transactions can read the resource, but writes are blocked.
- **Exclusive lock**: one transaction can write the resource, and other reads or writes may be blocked depending on the isolation semantics.

### Strength

- Strong control over conflicting access.
- Useful when correctness matters more than waiting, such as financial systems or inventory reservation.

### Cost

- Lock contention can slow the system down.
- Deadlocks can occur when transactions wait on each other in a cycle.
- Long transactions can block unrelated work if the lock scope is too broad.

## Optimistic Concurrency Control

Optimistic control assumes conflicts are rare. Transactions proceed without blocking each other, then validate before commit.

Typical process:

1. **Read**: read data and remember a version number, timestamp, or revision token.
2. **Validate**: before commit, check whether the version has changed.
3. **Write or abort**: commit if the version is unchanged; abort and retry if another transaction already modified the data.

### Strength

- Very high throughput when contention is low.
- Avoids holding locks during user think time or long application workflows.

### Cost

- Conflicting transactions must be retried.
- High-contention records can become expensive because many attempts abort.
- Application code often needs explicit retry logic.

## Conflict-free Replicated Data Types

**CRDTs** are specialized data structures for distributed systems. They are designed so concurrent updates can merge deterministically.

The key property is convergence: if every replica eventually receives the same updates, all replicas reach the same final state, even if updates arrive in different orders.

### Strength

- Excellent fit for distributed and offline-first systems.
- Useful for counters, sets, likes, presence state, and collaborative editing models.
- Avoids central coordination for many operations.

### Cost

- Data structures are more specialized.
- Not every business invariant can be represented as a CRDT.
- The merge logic can be conceptually difficult and must match the product semantics.

## Last Write Wins

**Last Write Wins** keeps the write with the newest timestamp and discards older writes.

This is simple and fast, which makes it common in some distributed and NoSQL systems. It is also risky: an update can be silently lost because the system decides that another write is newer.

Clock skew makes this more dangerous. If two machines disagree about time, the "latest" write may not be the write the user expects to win.

## Comparison

| Strategy | Performance | Complexity | Main risk | Best fit |
| --- | --- | --- | --- | --- |
| MVCC | High | High | storage/version cleanup and isolation surprises | general-purpose relational databases |
| Pessimistic locking | Lower under contention | Medium | deadlocks and waiting | strict consistency workflows |
| Optimistic concurrency | Very high when conflicts are rare | Medium | aborted transactions and retries | low-contention web apps |
| CRDTs | Very high in distributed settings | Very high | difficult merge semantics | collaborative and offline-first apps |
| Last Write Wins | Very high | Low | silent data loss | low-value or naturally overwriteable data |

## Heuristic

Use the weakest strategy that still protects the business invariant.

- If readers and writers both need throughput, start with MVCC.
- If conflicts are rare and retries are acceptable, use optimistic concurrency.
- If overwriting would be expensive or legally risky, avoid Last Write Wins.
- If the system must accept concurrent offline writes, evaluate CRDTs.
- If one actor must have exclusive control over a scarce resource, use pessimistic locking or an equivalent reservation mechanism.


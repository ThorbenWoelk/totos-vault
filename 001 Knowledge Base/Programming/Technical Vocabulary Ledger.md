---
created: 2026-04-02
last_edited: 2026-04-24
tags:
- technical-vocabulary
- learning
- programming
- ai
- architecture
connections:
- '[[Rust]]'
ai_generated: true
human_approved: false
category:
- Knowledge Base
- Programming
---
# Technical Vocabulary Ledger

A running ledger of short technical word explanations across programming, AI, Rust, Python, TypeScript, and software architecture.

## 2026-04-02

### Predicate
**Tags:** Programming, Concept, Logic, Rust, SQL, Validation
**In one line:** A predicate is a test that answers true or false.
**Definition:** A predicate is the small gate inside a program or query: it looks at some value or state and decides whether it matches.
It does not perform the whole action; it decides whether the action, branch, filter, or rule should apply.
**Where it shows up:** You meet predicates in `if` conditions, SQL `WHERE` clauses, Rust `#[cfg(...)]`, validation rules, authorization checks, and search filters.
**Example:** `age >= 18` is a predicate. In SQL, `WHERE active = true` is also a predicate: each row either passes through or stays behind.
**Related concepts:** Boolean expression, guard clause, filter, validator, pattern matching, selector.
**Keep in mind:** When something selects, guards, or filters, a predicate is usually doing the deciding.

### State Bag
**Tags:** Programming, Architecture, Object Design, Data Modeling, Refactoring
**In one line:** A state bag is an object that mostly carries mutable fields instead of protecting behavior.
**Definition:** A state bag holds raw values and lets other code decide what those values mean.
It is useful as a transport shape, but risky as a domain object because rules spread outward into callers.
**Where it shows up:** You see state bags in DTOs, config objects, Redux-like stores, form state, API payloads, and plain structs used as shared context.
**Example:** An object with `status`, `retries`, and `approvedAt` plus many `setX()` calls is a state bag. An object with `approve()` and `retry()` pulls rules back inside.
**Related concepts:** DTO, anemic domain model, value object, aggregate, encapsulation, record type.
**Keep in mind:** The more code mutates fields directly, the less the object protects its own meaning.

### Idempotency
**Tags:** Distributed Systems, APIs, Reliability, Payments, Messaging
**In one line:** Idempotency means repeating the same operation leaves the system in the same final state.
**Definition:** An idempotent operation can be retried after a timeout, crash, or duplicate message without accidentally doing the work twice.
The caller may send the request again, but the system recognizes it as the same intent rather than a fresh command.
**Where it shows up:** You meet idempotency in payment APIs, message consumers, webhooks, deployment scripts, infrastructure automation, and database upserts.
**Example:** `set_email("a@b.com")` is idempotent because the second call lands in the same state. `add_credit(10)` is not unless tied to a unique request ID.
**Related concepts:** Retry safety, deduplication, upsert, exactly-once delivery, at-least-once delivery, request ID.
**Keep in mind:** Ask whether a retry replays the same intent or accidentally performs the action again.

## 2026-04-03

### Macro
**Tags:** Programming Languages, Rust, Metaprogramming, Compile Time, Tooling
**In one line:** A macro is code that expands into more code before normal execution.
**Definition:** A macro rewrites or generates program structure at compile time or before runtime interpretation.
It can remove boilerplate and create expressive APIs, but the code you read may not be the exact code the compiler finally checks.
**Where it shows up:** <human> will see macros in Rust (`println!`, `vec!`), code generation tools, schema-derived helpers, test fixtures, and framework annotations.
**Example:** `println!("hi {}", name)` is not a normal function call. Rust expands the macro into lower-level formatting code before compilation continues.
**Related concepts:** Code generation, procedural macro, template, annotation, compiler plugin, syntax extension.
**Keep in mind:** When syntax feels magical, check whether a macro rewrites the program shape before the rest of the compiler sees it.

### Backpressure
**Tags:** Systems, Streaming, Queues, Async, Reliability, Performance
**In one line:** Backpressure is a signal from a slower consumer that a faster producer must slow down.
**Definition:** Backpressure keeps a system from accepting work faster than it can process it.
Without it, buffers grow, memory rises, latency stretches, and a temporary burst turns into a lasting failure.
**Where it shows up:** You meet backpressure in message brokers, streaming pipelines, async runtimes, HTTP rate limits, database connection pools, and bounded work queues.
**Example:** If a worker handles 100 messages per second but receives 1,000, a bounded queue plus throttling applies backpressure instead of buffering forever.
**Related concepts:** Rate limiting, flow control, bounded queue, load shedding, congestion control, retry storm.
**Keep in mind:** The important question is not only whether the system can absorb a burst, but who tells the sender to stop.

### Dispatch
**Tags:** Programming, Architecture, Events, State Management, Runtime Behavior
**In one line:** Dispatch means handing work to the part of a system that should handle it.
**Definition:** Dispatch is a routing moment: the current code packages intent and sends it to another layer, handler, runtime, or implementation.
It separates the decision to request work from the code that actually performs the work.
**Where it shows up:** <human> may see dispatch in Redux-style `dispatch(action)`, event buses, command handlers, web routers, job runners, and method dispatch in language runtimes.
**Example:** `dispatch({ type: "save" })` does not save by itself. It forwards intent to code that knows how to react to `save`.
**Related concepts:** Event handler, command bus, router, reducer, dynamic dispatch, message passing.
**Keep in mind:** Dispatch is about forwarding responsibility, not necessarily about `switch` statements.

## 2026-04-04

### FAISS
**Tags:** AI, Vector Search, Retrieval, RAG, Similarity Search, Infrastructure
**In one line:** FAISS is a library for fast similarity search over dense vectors.
**Definition:** FAISS builds indexes that make nearest-neighbor lookup practical when you have many embeddings.
Instead of comparing a query vector with every stored vector one by one, it uses specialized data structures to search quickly.
**Where it shows up:** You meet FAISS in semantic search, RAG pipelines, recommendation systems, deduplication, clustering, and embedding-heavy AI backends.
**Example:** A RAG system can embed a question, ask FAISS for the closest document chunks, and pass those chunks to an LLM as context.
**Related concepts:** Embedding, vector database, ANN search, HNSW, cosine similarity, semantic search.
**Keep in mind:** FAISS is a vector search engine, not a full product for persistence, auth, orchestration, and application logic.

## 2026-04-06

### Zero-Copy
**Tags:** Systems Programming, Performance, Rust, Networking, Memory
**In one line:** Zero-copy means using data without making an extra full copy of its bytes.
**Definition:** Zero-copy techniques avoid duplicating payloads between buffers, processes, or abstraction layers.
They matter most when data is large or movement happens often enough that memory bandwidth becomes the bottleneck.
**Where it shows up:** <human> may see zero-copy in file-to-socket transfers, Rust slices and borrowed buffers, Python memory views, network stacks, and columnar data systems.
**Example:** Linux `sendfile()` can stream file data to a socket without first copying it into a user-space buffer.
**Related concepts:** Borrowing, memory view, slice, DMA, buffer ownership, serialization overhead.
**Keep in mind:** Zero-copy does not mean bytes never move; it means the system avoids duplicating the payload into another full buffer.

### Phantom Types
**Tags:** Rust, Type Systems, Compile-Time Safety, API Design, Invariants
**In one line:** Phantom types are type parameters that carry compile-time meaning without stored runtime data.
**Definition:** A phantom type lets the type checker distinguish states, units, or permissions that share the same runtime representation.
The program stores the same bytes, but the compiler allows different operations depending on the phantom marker.
**Where it shows up:** You see phantom types in Rust wrappers for validated versus unvalidated data, typed IDs, stateful builders, protocol stages, and safety markers using `PhantomData`.
**Example:** `Token<Verified>` and `Token<Unverified>` can both hold the same string, while sensitive functions accept only `Token<Verified>`.
**Related concepts:** Type-state pattern, marker type, generics, newtype, branded type, static invariant.
**Keep in mind:** If a type parameter changes what code is allowed without changing stored fields, it is probably doing phantom work.

## 2026-04-07

### Bloom Filter
**Tags:** Data Structures, Probabilistic Algorithms, Caching, Databases, Search
**In one line:** A Bloom filter is a compact probabilistic structure for fast membership checks.
**Definition:** A Bloom filter can tell you that an item is definitely absent or maybe present.
It saves space by allowing false positives, which makes it useful as a cheap pre-check before slower storage or network work.
**Where it shows up:** <human> may encounter Bloom filters in cache systems, storage engines, web crawlers, duplicate detection, recommendation pipelines, and large-scale indexing.
**Example:** A database can ask a Bloom filter whether a key might be in a disk segment before paying the cost to read that segment.
**Related concepts:** Hash function, set membership, false positive, cache prefilter, Cuckoo filter, index.
**Keep in mind:** Trust `definitely absent`; verify `maybe present` somewhere authoritative.

## 2026-04-08

### Structured Concurrency
**Tags:** Concurrency, Async, Architecture, Reliability, Python, Kotlin, Rust
**In one line:** Structured concurrency keeps child tasks inside a clear parent scope with shared lifetime rules.
**Definition:** Structured concurrency treats concurrent work like a block of code with ownership and boundaries.
When the parent completes, fails, or is cancelled, the child tasks have a defined fate instead of drifting in the background.
**Where it shows up:** You meet structured concurrency in async runtimes, request handlers, task groups, parallel fan-out code, worker pools, and modern language concurrency libraries.
**Example:** A request starts three child tasks, waits for all of them, and cancels the rest if one fails. Fire-and-forget tasks are the opposite shape.
**Related concepts:** Task group, cancellation, async runtime, goroutine leak, nursery, supervisor tree.
**Keep in mind:** If the parent is done, you should be able to say what happens to every child immediately.

### Monomorphization
**Tags:** Rust, Compilers, Generics, Performance, Type Systems
**In one line:** Monomorphization turns generic code into concrete per-type code during compilation.
**Definition:** A compiler monomorphizes by copying and specializing generic functions for the actual types used in the program.
This gives generic code runtime speed close to handwritten concrete code, at the cost of more generated machine code.
**Where it shows up:** <human> will see monomorphization in Rust generics, C++ templates, ahead-of-time compilers, and performance tradeoffs involving binary size versus specialization.
**Example:** A generic `parse<T>()` used with `i32` and `String` may compile into two separate code paths, one for each concrete type.
**Related concepts:** Generics, template instantiation, static dispatch, dynamic dispatch, code bloat, zero-cost abstraction.
**Keep in mind:** Fast generics often come from duplicated specialized code, not from runtime magic.

## 2026-04-09

### Circuit Breaker
**Tags:** Reliability, Distributed Systems, APIs, Microservices, Resilience
**In one line:** A circuit breaker stops calling a failing dependency for a while instead of hammering it continuously.
**Definition:** A circuit breaker watches failures and opens when a dependency looks unhealthy.
While open, calls fail fast or use fallback behavior, giving the caller and dependency time to recover.
**Where it shows up:** You meet circuit breakers in API clients, service meshes, payment gateways, database proxies, microservices, and resilience libraries.
**Example:** After five failed calls to a billing API, the breaker opens and future requests fail fast until a later probe checks whether the API recovered.
**Related concepts:** Timeout, retry, bulkhead, fallback, health check, cascading failure.
**Keep in mind:** A circuit breaker protects the caller as much as the dependency; fast failure is often healthier than stubborn waiting.

### CRDT
**Tags:** Distributed Systems, Collaboration, Sync, Offline-First, Data Structures
**In one line:** A CRDT is a data structure whose replicas can merge concurrent changes and converge without coordination.
**Definition:** CRDTs are designed so independent replicas can accept local edits and later combine them deterministically.
The merge rules are built into the data type, so the same updates eventually produce the same state everywhere.
**Where it shows up:** <human> may meet CRDTs in collaborative editors, note apps, shared whiteboards, offline-first products, sync engines, and replicated state systems.
**Example:** Two users add different items to the same shared list while offline. When they reconnect, both additions survive the merge.
**Related concepts:** Operational transform, eventual consistency, vector clock, conflict resolution, replica, merge function.
**Keep in mind:** CRDTs trade simple central control for data types that are carefully shaped to merge.

## 2026-04-10

### Arena Allocation
**Tags:** Systems Programming, Memory Management, Compilers, Rust, Performance
**In one line:** Arena allocation means many objects are allocated from one region and freed together as a batch.
**Definition:** An arena is a memory region used for objects with roughly the same lifetime.
Instead of freeing each object separately, the program drops the whole arena when that phase of work ends.
**Where it shows up:** You see arena allocation in compilers, parsers, AST storage, game engines, request-scoped memory pools, and temporary graph processing.
**Example:** A parser allocates every syntax node in one arena, uses them during analysis, then drops the whole arena when parsing ends.
**Related concepts:** Allocator, object pool, bump allocation, lifetime, garbage collection, region-based memory.
**Keep in mind:** Arenas shine when lifetimes align; they get awkward when a few objects must escape the batch.

### Vector Clock
**Tags:** Distributed Systems, Consistency, Replication, Sync, Causality
**In one line:** A vector clock tracks causal ordering across distributed nodes.
**Definition:** A vector clock records how much history each replica has seen from every other replica.
By comparing those counters, a system can tell whether one event happened before another or whether two events are concurrent.
**Where it shows up:** <human> may run into vector clocks in replicated databases, sync engines, conflict detection, event histories, and academic discussions of distributed consistency.
**Example:** `{A:2, B:1}` says replica A has seen two A events and one B event. Comparing that vector with another reveals ordering or conflict.
**Related concepts:** Lamport clock, causal consistency, logical clock, CRDT, conflict resolution, happens-before.
**Keep in mind:** Vector clocks answer who saw what before this change, not what wall-clock time it was.

## 2026-04-11

### Discriminated Union
**Tags:** TypeScript, Type Systems, API Design, State Machines, Modeling
**In one line:** A discriminated union is a type made of several shapes identified by a shared tag field.
**Definition:** The tag marks which variant a value currently is, and the compiler uses that tag to narrow the available fields.
This makes mutually exclusive states explicit instead of scattering optional properties across one loose object.
**Where it shows up:** You meet discriminated unions in TypeScript result types, API response models, reducers, parser outputs, UI state machines, and protocol messages.
**Example:** `{ kind: "ok", data } | { kind: "error", message }` is safer than one object with both `data?` and `message?`.
**Related concepts:** Sum type, enum, tagged union, pattern matching, type narrowing, algebraic data type.
**Keep in mind:** If cases are mutually exclusive, encode that exclusivity in the type shape.

### Embedding
**Tags:** AI, Machine Learning, RAG, Vector Search, Semantic Search
**In one line:** An embedding is a vector that places meaning into a machine-comparable numeric space.
**Definition:** An embedding model turns text, code, images, or other inputs into numbers that preserve useful similarity signals.
Nearby vectors are treated as semantically related, which lets systems retrieve by meaning rather than exact words.
**Where it shows up:** <human> will see embeddings in RAG, semantic search, recommendation, clustering, code search, duplicate detection, and ranking pipelines.
**Example:** The phrases `fix bug` and `resolve defect` should land closer together in embedding space than either lands to `bake bread`.
**Related concepts:** Vector, cosine similarity, FAISS, vector database, retrieval, nearest neighbor search.
**Keep in mind:** Embeddings do not understand like people, but their geometry can still be useful enough to build with.

### Quantization
**Tags:** AI, Model Serving, Performance, Hardware, Optimization
**In one line:** Quantization stores model numbers with fewer bits to shrink memory and compute cost.
**Definition:** Neural networks usually contain many numeric weights, and lower-precision formats can represent those weights more cheaply.
The trade is that smaller numbers may lose detail, so speed and fit must be balanced against quality.
**Where it shows up:** You meet quantization in LLM deployment, edge inference, GPU and CPU optimization, ONNX pipelines, mobile ML, and model-serving discussions.
**Example:** A 4-bit model may fit into VRAM on a small local machine where the 16-bit version would not even load.
**Related concepts:** Precision, FP16, INT8, model compression, pruning, distillation, latency.
**Keep in mind:** Quantization is a trade, not a spell; always check quality, latency, and hardware constraints together.

## 2026-04-12

### MVCC
**Tags:** Databases, Transactions, PostgreSQL, Concurrency, Consistency
**In one line:** MVCC lets a database keep multiple row versions so readers and writers interfere less.
**Definition:** Multi-version concurrency control gives each transaction a snapshot of the database that is valid for its isolation rules.
Writers create new versions while readers can keep using older visible versions instead of blocking every time.
**Where it shows up:** <human> may encounter MVCC in PostgreSQL, MySQL internals, transaction isolation, vacuum behavior, and debugging why a query saw a certain value.
**Example:** One transaction reads yesterday's committed version of a row while another transaction is still preparing a newer update.
**Related concepts:** Transaction isolation, snapshot, vacuum, row version, lock, write skew.
**Keep in mind:** With MVCC, the row often means the version of the row visible to your transaction.

### Write-Ahead Log (WAL)
**Tags:** Databases, Storage, Reliability, Recovery, Replication
**In one line:** A write-ahead log records intended changes durably before the main data files are updated.
**Definition:** The WAL is the recovery trail a database writes before it mutates its normal storage pages.
If the process crashes mid-write, the database can replay or roll back from the log instead of guessing what happened.
**Where it shows up:** You see WALs in databases, storage engines, replication streams, recovery tooling, and checkpoint discussions.
**Example:** A database appends `update row X` to the WAL, flushes it, and only then mutates the actual table pages.
**Related concepts:** Commit log, transaction log, checkpoint, fsync, replication, crash recovery.
**Keep in mind:** The log is the oath the database makes before it touches the furniture.

## 2026-04-13

### Pinning
**Tags:** Rust, Async, Memory Safety, Futures, Systems Programming
**In one line:** Pinning means a value must stay at a stable memory location and not be moved.
**Definition:** Some Rust values may contain references into themselves or be represented by state machines that assume a stable address.
Pinning tells safe code that the value's address is fixed, while still allowing controlled internal mutation.
**Where it shows up:** <human> will see pinning in Rust async code, futures, generators, self-referential types, and APIs using `Pin<Box<T>>` or `Pin<&mut T>`.
**Example:** A future that stores pointers into itself cannot be safely moved after polling has begun, so Rust uses pinning to lock its address down.
**Related concepts:** `Unpin`, future, self-referential struct, borrowing, move semantics, memory address.
**Keep in mind:** Pinning is about address stability, not immutability.

### Trait Object
**Tags:** Rust, Type Systems, Polymorphism, API Design, Runtime Dispatch
**In one line:** A trait object is a runtime value used through a trait interface instead of a concrete type.
**Definition:** Trait objects let different concrete types share one behavioral contract behind a pointer such as `Box<dyn Trait>`.
The caller loses compile-time knowledge of the exact type and uses a vtable to call the right implementation at runtime.
**Where it shows up:** You meet trait objects in Rust plugin-style systems, heterogeneous collections, dependency injection patterns, UI components, and APIs returning `Box<dyn Trait>`.
**Example:** A vector of `Box<dyn Renderer>` can hold both `SvgRenderer` and `CanvasRenderer`, while callers only invoke shared renderer methods.
**Related concepts:** Dynamic dispatch, vtable, generics, monomorphization, interface, polymorphism.
**Keep in mind:** Trait objects buy flexibility with runtime dispatch; generics buy specialization with compile-time knowledge.

## 2026-04-14

### Attention
**Tags:** AI, Transformers, LLMs, Deep Learning, Sequence Modeling
**In one line:** Attention lets a model weigh which input parts matter for the current token or position.
**Definition:** Attention computes relationships between positions in the input and uses those weights to mix relevant information.
It gives transformer models a flexible way to connect distant context instead of relying only on local order.
**Where it shows up:** <human> will see attention in transformers, LLMs, translation systems, image captioning, multimodal models, and explanations of context windows.
**Example:** In `The cat sat on the mat`, attention can make `sat` look strongly at `cat` and `mat` while processing the sentence.
**Related concepts:** Transformer, self-attention, query-key-value, context window, embedding, sequence model.
**Keep in mind:** Attention is selective lookup over context, not human attention or conscious focus.

## 2026-04-16

### Gradient Checkpointing
**Tags:** AI, Training, Deep Learning, GPU Memory, PyTorch, Optimization
**In one line:** Gradient checkpointing saves training memory by recomputing some activations later instead of storing all of them.
**Definition:** During backpropagation, neural networks need intermediate activations from the forward pass.
Checkpointing stores only selected activations, then rebuilds the missing ones when gradients are computed.
**Where it shows up:** You meet gradient checkpointing in deep-learning training configs, Hugging Face setups, PyTorch memory tuning, large-model finetuning, and GPU budget tradeoffs.
**Example:** A transformer block can save a few checkpoints during the forward pass and recompute the missing tensors during backward pass.
**Related concepts:** Backpropagation, activation, memory-compute tradeoff, batch size, finetuning, mixed precision.
**Keep in mind:** Checkpointing trades compute for memory; training usually gets slower while memory pressure drops.

## 2026-04-17

### Outbox Pattern
**Tags:** Architecture, Distributed Systems, Messaging, Databases, Reliability
**In one line:** The outbox pattern writes a message record in the same transaction as the main data change.
**Definition:** The outbox pattern captures the intent to publish an event inside the database transaction that changes business state.
A separate worker later reads the outbox table and delivers the message to an external broker.
**Where it shows up:** <human> may see the outbox pattern in event-driven systems, microservices, CDC pipelines, order workflows, payment flows, and reliable integration design.
**Example:** Creating an order also inserts an `OrderCreated` row into an outbox table. A worker later publishes that row to Kafka or another message bus.
**Related concepts:** Transactional messaging, CDC, event sourcing, message broker, idempotent consumer, saga.
**Keep in mind:** Capture atomic intent first, then deliver externally after the database has made the change durable.

### Duck Typing
**Tags:** Python, Dynamic Languages, API Design, Testing, Polymorphism
**In one line:** Duck typing accepts a value because it behaves correctly, not because it has a declared type label.
**Definition:** In duck typing, an object's usable methods and behavior matter more than its class name.
This makes APIs flexible, but it moves correctness toward documented protocols, tests, and runtime behavior.
**Where it shows up:** You meet duck typing in Python file-like objects, mocks, iterables, web frameworks, plugin systems, and many dynamic-language interfaces.
**Example:** If an object has a working `.read()` method, code may treat it like a file even when it is not an actual file class.
**Related concepts:** Protocol, structural typing, nominal typing, interface, mock, polymorphism.
**Keep in mind:** In duck typing, the contract lives in expected behavior; the label is secondary.

## 2026-04-18

### Linearizability
**Tags:** Distributed Systems, Consistency, Databases, Concurrency, Correctness
**In one line:** Linearizability means each operation appears to take effect at one single instant in a shared global order.
**Definition:** A linearizable system behaves as if every completed operation is placed on one real-time timeline.
Once a write succeeds, later reads must reflect that write or a newer one, even if the system is distributed underneath.
**Where it shows up:** <human> may encounter linearizability in distributed databases, coordination systems, locks, compare-and-swap APIs, and discussions of strong consistency.
**Example:** If one client writes `x = 1` and gets success, a later read that returns `0` would violate linearizability.
**Related concepts:** Serializability, sequential consistency, consensus, strong consistency, read-after-write consistency, CAP theorem.
**Keep in mind:** Linearizability feels like one correct timeline, but that guarantee is expensive and not always necessary.

### Type Erasure
**Tags:** Type Systems, TypeScript, Java, Runtime Validation, API Boundaries
**In one line:** Type erasure means type information helps during compilation but is not fully preserved at runtime.
**Definition:** A language or compiler may use types to check code, then emit runtime code where some of those types no longer exist.
That gap matters when runtime behavior needs proof of shape, identity, or generic parameters.
**Where it shows up:** You see type erasure in TypeScript-to-JavaScript compilation, Java generics, runtime validation libraries, reflection limits, and API boundary bugs.
**Example:** A TypeScript `User` interface guides the editor and compiler, but the JavaScript object carries no built-in `User` stamp to inspect.
**Related concepts:** Runtime type checking, reflection, generics, schema validation, structural typing, nominal typing.
**Keep in mind:** If runtime behavior depends on type shape, add explicit metadata or validation because compiler help may be gone.

## 2026-04-19

### Schema Drift
**Tags:** Data Engineering, AI, APIs, Validation, Architecture, JSON
**In one line:** Schema drift is when the shape or meaning of data changes over time and downstream code quietly stops matching it.
**Definition:** Schema drift happens when fields are added, renamed, removed, or repurposed without every consumer updating in lockstep.
The break is often subtle: the system still runs, but reports go wrong, parsers drop fields, prompts miss context, or validations start failing at the edges.
**Where it shows up:** You meet schema drift in ETL pipelines, event streams, JSON APIs, warehouse tables, LLM structured output, analytics dashboards, and long-lived service contracts.
**Example:** Yesterday an event had `user_id`; today it ships `userId`, and `plan` changed from a string to an object. Old consumers may not crash, but they now read the wrong shape.
**Related concepts:** Schema evolution, backward compatibility, contract testing, data validation, protobuf, migration.
**Keep in mind:** If data crosses a boundary, treat its shape like an API and version or validate it before trusting it.

## 2026-04-23

### Copy-on-Write (CoW)
**Tags:** Programming, Concept, Rust, Operating Systems, Databases, Performance
**In one line:** Copy-on-write means data is shared until someone tries to modify it, and only then is a private copy made.
**Definition:** Copy-on-write avoids eager duplication when multiple readers can safely share the same bytes or pages.
The cheap path is read-only sharing; the expensive path happens only at the first write, when the system clones just enough data to preserve isolation.
**Where it shows up:** You meet copy-on-write in OS process `fork()`, Rust's `Cow<T>`, persistent data structures, snapshotting filesystems, database page management, and memory-efficient string or buffer APIs.
**Example:** After `fork()`, parent and child initially point at the same memory pages. If the child writes to one page, the kernel copies that page first, so the parent's view stays unchanged.
**Related concepts:** Zero-copy, cloning, snapshot, immutability, persistent data structure, reference counting.
**Keep in mind:** CoW delays copying, it does not remove it; the trick is winning when reads are common and writes are rare.

### Variance
**Tags:** Type Systems, Generics, TypeScript, Rust, Python, API Design
**In one line:** Variance describes how subtype relationships behave when values are wrapped inside another type like a list, box, or function.
**Definition:** Variance answers whether `Container<Cat>` should be accepted where `Container<Animal>` is expected.
If that sounds harmless, function inputs are where it bites: a callback that only handles cats is not safe where the program may send any animal.
**Where it shows up:** You meet variance in TypeScript callback types, Rust lifetimes and generic wrappers, Python typing, library API design, and compiler error messages about why one generic type does not fit another.
**Example:** A `List<Cat>` can often be read as a `List<Animal>` if you only read from it. But a handler of type `(Cat) => void` cannot stand in for `(Animal) => void`, because someone might pass a `Dog`.
**Related concepts:** Subtyping, covariance, contravariance, invariance, generics, Liskov substitution.
**Keep in mind:** A good shortcut is: outputs tend to be covariant, inputs tend to be contravariant, mutable containers tend to get restrictive.

## 2026-04-27

### Beam Search
**Tags:** AI, LLMs, Search, Inference, Decoding, Transformers
**In one line:** Beam search explores several candidate sequences in parallel during generation instead of greedily picking the best token at each step.
**Definition:** At each decoding step, beam search keeps the top k complete sequences (the beam width) rather than committing to a single token.
This lets the model consider multiple paths, making it more likely to find a high-quality overall trajectory.
The tradeoff: a wider beam costs more computation and a very wide beam can favor safer, repetitive, or less surprising outputs.
**Where it shows up:** You meet beam search in LLM sampling configs (`num_beams` in Hugging Face), machine translation, speech recognition, grammar-constrained generation, and structured output decoders.
**Example:** With a beam of 3, the model keeps the three most promising partial sentences after each token and expands each one, selecting the best full sentence at the end instead of the first token on the greedy path.
**Related concepts:** Greedy decoding, top-k sampling, top-p (nucleus) sampling, temperature, contrastive search, constrained decoding.
**Keep in mind:** Beam search trades throughput for coverage; use greedy or stochastic sampling when you want speed or variety, and beam when you want the best path.

### Mmap (Memory-Mapped Files)
**Tags:** Systems Programming, Rust, Operating Systems, Performance, Databases, File I/O
**In one line:** Mmap maps a file into a process address space so reads and writes happen through memory operations instead of system calls.
**Definition:** Instead of calling `read()` and `write()` to shuttle data between kernel and user buffers, mmap lets the operating system page file contents in and out on demand.
The kernel manages which parts are loaded, and the program just reads and writes memory as if the whole file were already in RAM.
Access to pages that are not yet resident triggers a page fault that transparently loads the data.
**Where it shows up:** You meet mmap in database storage engines (SQLite, LMDB, RocksDB), fast IPC via shared memory, binary parsers, loaders that need random access into large files, and Rust crates like `memmap2` or `bytemuck`.
**Example:** A 10 GB database can be mmap'd into a `&[u8]` slice. The program navigates offsets into the slice directly; the OS fetches pages when they are touched, without explicit read calls or manual buffer management.
**Related concepts:** Virtual memory, page cache, page fault, zero-copy, buffer pool, direct I/O.
**Keep in mind:** Mmap shines for random access and lazy loading but can be tricky with file truncation, concurrent writes, SIGBUS on underlying file errors, and high page-fault cost on first access.


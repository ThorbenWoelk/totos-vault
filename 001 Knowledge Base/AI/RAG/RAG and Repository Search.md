---
created: 2026-04-02
last_edited: 2026-04-29
tags:
  - rag
  - search
  - retrieval
  - semantic-search
  - hybrid-search
  - bm25
  - vector-search
  - pgvector
  - lancedb
  - ollama
connections:
  - "[[RAG in Practice]]"
  - "[[Entity Graph RAG Architecture]]"
  - "[[RAG Best Practices]]"
ai_generated: true
human_approved: true
category:
  - Knowledge Base
  - AI
  - RAG
---
## RAG-like architectures across repository patterns

#2026-03 I found five concrete buckets:

1. **Production-grade hybrid retrieval + agentic chat**
   - `<video-transcript-rag-app>`

2. **Solid semantic retrieval with a simpler RAG stack**
   - `<message-archive-rag-app>`
   - `<vault-agent-root>`

3. **Minimal semantic memory retrieval**
   - `<journal-memory-app>`

4. **Managed memory search, not fully owned RAG**
   - `<assistant-memory-app>`

1. **RAG evaluation or scenario definitions**
   - `<rag-evaluation-suite>`

## Core Concepts

Not all "search with AI" is the same thing.

### Exact Or Keyword Search

This means:
- SQL `LIKE`
- FTS
- BM25-ranked keyword retrieval
- title and metadata matching

Strengths:
- precise for names, acronyms, IDs, filenames, exact quotes
- easy to debug
- deterministic

Weaknesses:
- misses paraphrases
- poor at semantic similarity

### Semantic Search

This means:
- embed query
- embed documents or chunks
- retrieve nearest neighbors in vector space

Strengths:
- good for paraphrase and topical similarity
- useful when user wording differs from stored wording

Weaknesses:
- can miss exact string matches
- harder to debug
- quality depends heavily on chunking and embedding model

### Hybrid Retrieval

This means:
- keyword retrieval and semantic retrieval both run
- results are merged and reranked or fused

This is the strongest general-purpose pattern in the local examples.

Grounded high-level example from `<video-transcript-rag-app>`:

```text
BM25 FTS leg + vector leg + RRF fusion + rerank
```

Reference:
- Local project source. (n.d.). `<video-transcript-rag-app>/docs/search-indexing.md`.

### RAG

This means:
- retrieval is used to build model context
- the LLM answers from retrieved evidence

### Agentic RAG

This means:
- the model can decide which tools to call
- retrieval can happen in multiple passes
- planning, reformulation, tool budgets, and evidence selection matter

This is meaningfully more complex than `embed -> top-k -> prompt`.

### Practical takeaway

`<journal-memory-app>` teaches the simplest `pgvector` pattern.

`<message-archive-rag-app>` teaches the strongest local `LanceDB` pattern.

`<vault-agent-root>` shows the real cost of split metadata + vector storage in a personal knowledge app.

## Top-K Retrieval vs Multi-Pass Agentic Retrieval

### Top-K Retrieval

This is the classic basic RAG pattern:
- embed query
- retrieve top `k` chunks
- pass them to the model

It is often enough for:
- small corpora
- single-fact lookup
- narrow semantic-memory tasks

Grounded example from `<journal-memory-app>`:

```rust
.bind(&embedding)
.bind(threshold)
.bind(limit)
.fetch_all(&state.pool)
```

That is effectively a top-k semantic retrieval flow.

Reference:
- Local project source. (n.d.). `<journal-memory-app>/backend/src/rag/mod.rs`.

Strengths:
- simple
- cheap
- fast

Weaknesses:
- one weak retrieval pass can sink the answer
- no adaptation if first-pass evidence is poor

### Multi-Pass Agentic Retrieval

This means:
- the system plans retrieval
- may rewrite or expand queries
- may run multiple retrieval passes
- may decide that more evidence is needed

Grounded example from `<video-transcript-rag-app>`:

```rust
ChatStatusPayload::new("classifying", "Planning search")
...
let planned = ... prompt_with_fallback("chat_query_plan", ...)
...
retrieval_passes = pass_count
```

Reference:
- Local project source. (n.d.). `<video-transcript-rag-app>/backend/src/services/chat/retrieval.rs`.

This is much better for:
- synthesis
- comparison
- broad evidence gathering
- ambiguous questions

### Practical takeaway

Top-k retrieval is the right starting point.

Multi-pass agentic retrieval is what you build when the remaining problem is not "can I retrieve anything?" but "can I retrieve the right evidence in the right way?"

## Comparison Matrix

| Example | Problem | Retrieval Type | Storage | Embeddings | LLM Role | Maturity |
| --- | --- | --- | --- | --- | --- | --- |
| `<video-transcript-rag-app>` | Search and chat over video transcripts/summaries | Hybrid: BM25 + vector + RRF + optional rerank + HyDE | S3 + libSQL/Turso + S3 Vectors | Ollama embeddings | Full agentic chat with retrieval planning | Highest |
| `<message-archive-rag-app>` | Search and ask over message archives | Vector-first with extra keyword/entity ranking | LanceDB + SQLite-derived analytics DB | `fastembed` BGE small | RAG assistant over archive content | High |
| `<vault-agent-root>` | Search and agent chat over Obsidian vaults | FTS + LanceDB semantic retrieval | SQLite + LanceDB | `fastembed` | ADK tool-using chat with safe patch proposals | Medium, still evolving |
| `<journal-memory-app>` | Retrieve memories for journaling context | Simple pgvector similarity search | PostgreSQL + pgvector | Ollama embeddings | Memory enrichment, not broad agentic retrieval | Medium-low but clean |
| `<assistant-memory-app>` | Long-term assistant memory lookup | Managed memory search | Mem0 service | Hidden behind provider | Memory lookup tool for agent | Low ownership |
| `<rag-evaluation-suite>` | Compare frameworks on RAG tasks | Scenario/eval only | Varies by framework | Varies | Evaluation, not a deployed search product | Not a real RAG app |


## Recurring Design Patterns 

### Pattern 1: Retrieval Should Be A Projection, Not The Source Of Truth

Meaning:
- canonical content lives elsewhere
- search index is rebuildable

### Pattern 2: Hybrid Beats Pure Vector Search For Real User Queries

Seen most clearly in:
- `<video-transcript-rag-app>`

Why:
- exact model names, tags, filenames, acronyms, IDs, and titles often need keyword matching
- semantics alone are not enough

### Pattern 3: Chunking Quality Matters More Than People Think

Good chunking decisions:
- preserve headings or section structure
- use overlap carefully
- keep metadata close to chunk text

### Pattern 4: Agentic RAG Needs Budgets And Guardrails

Why:
- without budgets, agents over-search
- without scope control, agents retrieve irrelevant context
- without safe tool design, the system becomes hard to trust

### Pattern 5: Search And Write Paths Should Be Separate

This matters especially when the underlying data is editable user-owned content.

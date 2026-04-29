---
created: 2026-04-04
last_edited: 2026-04-29
tags:
  - rag
  - indexing
  - performance
  - best-practices
  - embeddings
  - incremental-sync
connections:
  - "[[RAG and Repository Search]]"
  - "[[RAG in Practice]]"
ai_generated: true
human_approved: false
category:
  - Knowledge Base
  - AI
  - RAG
---
## Pseudo-local RAG

Commercial tools like Cursor and Tabnine look like they do local RAG, but they mostly don't. Cursor computes file hashes locally, ignores files via .gitignore/.cursorignore, uploads only changed files, and does chunking and embedding on the server. Tabnine is even more explicit: for chat RAG, it computes vector embeddings on server GPUs because doing that locally would stress the user's machine.

## References

- Cursor. (n.d.). [Security](https://cursor.com/security/). Cursor.
- Tabnine. (n.d.). [Personalization](https://docs.tabnine.com/main/welcome/readme/personalization). Tabnine Docs.

So when you build local RAG and your machine hammers, it is not because local RAG is inherently flawed. It is because you are doing the expensive parts that commercial tools offload.

### The Six Ways Local RAG Goes Wrong

1. **You are indexing more than you think.** Without strict exclude lists, you index node_modules, .obsidian, .git, dist, build, generated docs, app internals, and the vector DB itself.
2. **You are re-embedding unchanged files.** Without content hashing, every watcher event triggers full re-embedding of a note, even if nothing changed.
3. **A watcher is triggering on lots of filesystem noise.** Git operations, IDE temp files, and .obsidian config writes all generate events.
4. **You are embedding at startup instead of treating indexing as a separate batch job.** A full rebuild at each start is devastating for large vaults.
5. **You are embedding every chunk locally with a CPU model.** fastembed and sentence-transformers run on the CPU. Hundreds of chunks × local inference = hot CPU.
6. **Your chunking is too fine.** A "couple of docs" can become hundreds or thousands of chunks if chunking is too aggressive.

## What Commercial Tools Do Better

### Strict Ignore Lists

They do not index everything. They use .gitignore, .cursorignore, and exclude lists to skip:
- node_modules, dist, build, target
- hidden dirs (.git, .obsidian, .vscode)
- generated files (lock files, compiled output)
- the vector DB / index files themselves

### Hash-Based Incremental Sync

They hash file content and only process files whose hash changed since last index. This means:
- opening a file without editing = no reindex
- renaming a file = minimal work
- bulk git operations = only changed files re-indexed

### Embed Only Changed Content

On each change, they:
1. Check content hash against stored hash
2. Skip if unchanged
3. Delete old chunks for that path
4. Re-chunk and re-embed only the changed file

### Offload Embedding and Search Infra

Cursor and Tabnine send content to servers for embedding. For local-only: use larger chunks and hard caps on chunk count per file to reduce embedding calls.

### Keep IDE Runtime and Indexing Pipeline Separate

Indexing never blocks the main application. It runs as a background worker with its own lifecycle.

### Cache Chunk Embeddings by Content Hash

If a chunk's text hasn't changed, its embedding is reused from cache. This avoids redundant embedding calls even when restructuring documents.

## Concrete Local RAG Checklist

### Must Have

- [ ] **Exclude list**: node_modules, .obsidian, .git, dist, build, target, generated docs, app internals, vector DB data
- [ ] **Content hashing**: hash note content (SHA-256 of body text) and skip upsert when unchanged
- [ ] **Debounce watcher events**: 500–2000 ms. A single save should not trigger 5 reindexes
- [ ] **Don't rebuild index on normal app startup**: startup should load existing index, not rebuild it. Rebuild is a separate explicit command
- [ ] **Limit chunks per file**: hard cap (e.g., 20–50 chunks per file max)
- [ ] **Use larger chunks**: 500–1000 tokens per chunk is usually better than 100–200. Fewer chunks = fewer embeddings = less CPU

### Should Have

- [ ] **Background indexing**: index in a separate thread/process from the serving path
- [ ] **Batch embedding**: embed multiple chunks in one call rather than one at a time
- [ ] **FTS first, embeddings second**: for small corpora, SQLite FTS / BM25 is often enough. Use vector search only when keyword search is insufficient
- [ ] **Indexing status endpoint**: report progress so you know what's happening
- [ ] **Graceful degradation**: if vector search is unavailable or still indexing, fall back to FTS

### Nice To Have

- [ ] **Content-hash embedding cache**: store `hash(chunk_text) → embedding vector` so identical chunks across files share embeddings
- [ ] **Priority indexing**: recently modified files first, old files last
- [ ] **Stale detection**: periodically check if indexed paths still exist on disk
- [ ] **Memory-mapped vector store**: use mmap-based stores (LanceDB already does this) to avoid loading all vectors into RAM

## When To Skip Vector RAG Entirely

For "a couple of docs", skip vector RAG. Use instead:
- **Direct file reads**: load the full document text into context
- **SQLite FTS**: fast keyword search with no embedding overhead
- **BM25 ranking over FTS**: still no embeddings, still effective
- **Simple grep / ripgrep**: for exact pattern matching

Vector RAG adds value when:
- corpus is too large to fit in context
- user queries are semantically diverse
- paraphrase matching matters more than exact keyword matching

## Lessons From Local Examples

### Vault Agent Pattern

Problems found:
- `indexer.py` only excludes `.git/` and `/target/`, indexing 753+ markdown files including 73 in node_modules, 172 under apps, 101 in hidden dirs
- `vector_store.py` deletes prior chunks and re-embeds the full note on every change, regardless of whether content changed
- `watcher.py` watches entire vault roots with no debounce
- `start_app.sh` can trigger a full rebuild before starting the watcher
- `embedding.py` uses local fastembed, so all embedding is CPU/RAM bound

### Video Transcript RAG Pattern

What it does right:
- Indexing is a background worker that claims pending items
- Canonical content is separate from search projection
- Hybrid retrieval (BM25 + vector + RRF)
- Batch embedding with size caps
- Graceful degradation when one retrieval leg fails

### Message Archive RAG Pattern

What it does right:
- Batch indexing pipeline separate from serving
- LanceDB for local vector storage (mmap-friendly)
- fastembed BGE small (lightweight model)
- Clear separation between analytics DB and vector store

## Bottom Line

The gap between "my machine is dying running local RAG" and "Cursor/Tabnine seem fine" is almost entirely explained by:
1. Strict vs. no ignore lists
2. Hash-based incremental vs. full re-embed
3. Server-side vs. local CPU embedding
4. Background indexing vs. blocking startup
5. Coarse vs. fine chunking

Fix the first four and local RAG is practical even on a laptop.

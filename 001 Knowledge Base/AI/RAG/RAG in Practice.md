---
created: 2026-01-29
last_edited: 2026-04-24
tags:
  - rag
  - llm
  - vector-space-model
  - bm25
  - vector-database
  - indexing
  - retrieval
  - generation
  - embeddings
connections:
  - "[[RAG Best Practices]]"
  - "[[Entity Graph RAG Architecture]]"
  - "[[RAG and Repository Search]]"
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - AI
  - RAG
---
# RAG concepts

## Indexing

= The process from raw data to storage in a vector database.

1. convert data in various formats (PDF, HTML, Markdown, or XML) to text. 
2. text is chunked and processed by embedding model (chunks that must be smaller in size than context length of model). Chunks are transformed into a vector representation, assigned an identifier, and stored in a vector database.

## Retrieval

When a query arrives, the most relevant chunks must be found. The same encoder
used for document embedding is used to obtain a vector for the query. The similarity score
between the query vector and the vectors stored in the database is then calculated. Top K chunks are selected based on the similarity score. 

## Generation

The chunks found together with the query are incorporated into a consistent prompt for LLM used for generation. Different LLMs may require different elements in the prompt to work best; similarly, we can have prompts that are tailored for specific tasks. In addition, we can also add elements from the previous conversation (history).

# Methods

## BM25

In search, **BM25** is a way to rank results.

It is a variant of **TF-IDF** where two parameters are added: b, which controls the importance of document length normalization, and k, which controls the relationship between term frequency (TF) and inverse document frequency (IDF). The usually recommended values are b=0.75 and k between 1.2 and 2.

BM25 is much more flexible than TF-IDF and can be adapted to different scenarios.

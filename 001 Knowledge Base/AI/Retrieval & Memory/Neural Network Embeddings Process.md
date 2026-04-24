---
created: 2025-07-22
last_edited: 2026-04-24
tags:
- neural-network
- embeddings
- transformer
- tokenization
- attention
- pooling
- nlp
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2017 Attention Is All You Need - Vaswani et al]]'
- '[[How Transformer LLMs Work]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- Retrieval & Memory
---
## Token to Embedding Pipeline

### 1. Tokenization → Numbers
- Each token gets unique ID: "Hello" → 1247, "world" → 8934
- Input sequence becomes number sequence

### 2. Initial Embedding Lookup
- Token IDs mapped to dense vectors via learned embedding matrix
- Each token → initial vector (e.g., 768 dimensions)

### 3. Transformer Processing
**Attention Mechanisms**
- Each token "attends to" all other tokens for context
- "bank" near "river" vs "money" gets different processing
- Learns which tokens matter for understanding each token

**Feed-Forward Layers**
- Mathematical transformations capturing complex patterns
- Multiple layers: early = syntax, later = semantics
- Each layer builds more abstract understanding

### 4. Pooling Strategy
- Multiple tokens → single embedding
- Methods: averaging, CLS token, attention-weighted combination

### 5. Output Projection
- Final layer projects to target dimensions (3072 for Gemini)
- Semantic meaning compressed into fixed-size vector space

## Semantic Learning
- Training on millions of text examples
- Similar concepts → similar vectors
- Vector arithmetic: "king" - "man" + "woman" ≈ "queen"
- Each dimension captures different meaning aspects

## Key Insight
Embeddings emerge from the network learning to predict/understand text relationships across massive datasets.


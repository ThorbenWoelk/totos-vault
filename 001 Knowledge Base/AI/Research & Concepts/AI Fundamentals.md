---
created: 2026-08-09
last_edited: 2026-08-09
tags:
  - ai
connections: []
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - AI
  - Research & Concepts
---
## Questions

- What is a large language model?
- How is an LLM trained?
- How can an LLM learn information about a user?
- How do embeddings work?

## Tokenization

An LLM processes tokens rather than whole words. Tokens are often subword pieces:

- “tokenization” might become **token** + **ization**.
- “Hello world!” might become **Hello** + ** world** + **!**.

Each token maps to a numeric token ID. Illustrative IDs from the source note were **Hello → 1247**, **world →
8934**, and **! → 15**.

## Initial embeddings

The model looks up each token ID in a learned embedding matrix. The lookup maps the token to a dense numeric
vector—768 dimensions in the source example; for example:

- Token 1247 → **[0.23, -0.87, 0.45, …]**
- Token 8934 → **[-0.12, 0.93, -0.34, …]**

## Transformer processing

- **Attention:** Each token considers other tokens to build a context-sensitive representation. “Bank” near
“river” differs from “bank” near “money.”
- **Feed-forward layers:** Mathematical transformations capture patterns and relationships.
- **Multiple layers:** Earlier layers tend to represent lower-level patterns such as syntax; later layers can
represent more abstract semantics.

## Pooling into one embedding

When a sequence needs one vector representation, common strategies include:

- Average all token vectors.
- Use a special classification token that aggregates meaning.
- Compute an attention-weighted average based on token importance.

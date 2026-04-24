---
created: 2025-07-22
last_edited: 2026-04-24
tags:
- gemini-embedding
- embeddings
- multilingual
- semantic-search
- vertex-ai
- pricing
- google-genai
- llm
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[RAG in Practice]]'
- '[[Neural Network Embeddings Process]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- Models & Research
---
**Model**: gemini-embedding-001  

## Key Capabilities
- **3072-dimensional vectors** (scalable to 1536, 768)
- **Multilingual**: 100+ languages supported
- **Input limit**: 2048 tokens
- **Performance**: Top MTEB leaderboard performer

## Primary Use Cases
- Text retrieval and semantic search
- Text classification and categorization
- Cross-language applications
- Domain-specific tasks (science, legal, finance, coding)

## Technical Features
- Matryoshka Representation Learning (MRL) for flexible dimensionality
- Unified model across diverse domains
- Compatible with embed_content endpoint

## Pricing & Access
- $0.15 per 1M input tokens
- Available via Gemini API and Vertex AI
- Free tier available for experimentation

## Code Example
```python
from google import genai
client = genai.Client()
result = client.models.embed_content(
    model="gemini-embedding-001",
    contents="What is the meaning of life?"
)
print(result.embeddings)
```

Output format: 3072 float values representing semantic meaning in high-dimensional space.

## Processing Flow
**Text → Tokens → Embeddings**
1. Input text gets tokenized into subword units
2. Tokens processed through neural network  
3. Output: 3072-dimensional semantic vector

**Token vs Words**: Tokens are subword pieces, roughly 1,500-2,000 words = 2048 tokens for English.


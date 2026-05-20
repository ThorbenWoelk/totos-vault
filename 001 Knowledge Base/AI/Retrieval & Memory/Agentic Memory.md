---
created: 2025-12-16
last_edited: 2026-04-24
tags:
- agentic-memory
- short-term-memory
- episodic-memory
- semantic-memory
- procedural-memory
- vector-db
- graph
- llm
connections:
- '[[Entity Graph RAG Architecture]]'
- '[[Memory Systems]]'
- '[[Memory Practices]]'
ai_generated: false
human_approved: false
category:
- AI
tagging_processed_count: 10
tagging_last_processed: 2026-05-15
---
## Types of Memory

| type       | primary       | secondary        | avoid           | insight                 |
| ---------- | ------------- | ---------------- | --------------- | ----------------------- |
| short-term | LLM context   | in-memory buffer | SQL             | fast and ephemeral      |
| episodic   | vector db     | sql              | pure JSON       | temporal & similarity   |
| semantic   | graph         | vector db        | fine-tune alone | relationships matter    |
| procedural | system prompt | fine-tuned model | vector db       | behavior, not retrieval |


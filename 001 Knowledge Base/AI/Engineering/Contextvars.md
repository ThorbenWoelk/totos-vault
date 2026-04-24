---
created: 2026-01-09
last_edited: 2026-04-24
tags:
- python
- async
- contextvars
- langchain
- langgraph
- pydantic-ai
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2026-03-31 Claude Code Leak]]'
- '[[2025-12 AI Recap]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
---
In frameworks like LangChain, PydanticAI, or LangGraph, the execution often passes through layers of third-party code.

- **The Issue:** You cannot easily pass scenario_id as an argument through `Framework -> Agent -> Executor -> Tool` because you don't control the function signatures of the intermediate layers.
- **The Solution:** `contextvars` act like "thread-local storage" (but async-safe), allowing the scenario_id set in runner.py to "teleport" directly to the lookup_knowledge function, bypassing the intermediate layers.


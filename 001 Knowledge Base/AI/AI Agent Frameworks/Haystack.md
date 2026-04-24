---
created: 2026-01-24
last_edited: 2026-04-24
tags:
- haystack
- ai-agent-framework
- graph
- python
- performance
- static-typing
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[OpenAI Agents SDK]]'
- '[[2026-03-31 Claude Code Leak]]'
- '[[Agentic Memory]]'
ai_generated: false
human_approved: false
category:
- Knowledge Base
- AI
- AI Agent Frameworks
---
# Graphs

We create components (nodes) which can be anything. You register instances with arbitrary string names.

## I like

- seems fast

## I don't like

- builds string-based references instead of using Python objects. the internal graph is then rendered at runtime. No static type-checking. No automatic refactoring.
-

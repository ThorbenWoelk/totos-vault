---
created: 2026-03-23
last_edited: 2026-04-24
tags:
  - lcm
  - context-memory
  - ai
  - knowledge-base
  - compression
connections:
  - "[[Agentic Memory]]"
  - "[[AI Agent Development Guidelines]]"
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - AI
tagging_processed_count: 1
tagging_last_processed: 2026-04-24
---
# OpenClaw
#2026-03 

- **LCM Layered Summary Approach**: LCM (Lossless Context Memory) compacts messages into layered summaries where nothing is ever lost. Instead of truncation, it creates an indexed structure with nodes that can represent entire days or huge lengths of conversation. The system maintains relationships to all data while still allowing access to specific conversations.    
- **Fresh Tail Protection**: The Fresh Tail is a protected portion of the most recent raw messages that are never compacted. When raw messages outside the Fresh Tail exceed 2 tokens, LCM fires incremental compaction asynchronously, meaning conversations aren't interrupted.


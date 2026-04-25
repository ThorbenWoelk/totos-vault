---
created: 2025-07-25
last_edited: 2026-04-24
tags:
  - ai-agent
  - kv-cache
  - filesystem-context
  - error-recovery
  - attention-manipulation
  - context-engineering
  - agent-architecture
connections:
  - "[[001 Knowledge Base/AI/_index|AI Index]]"
  - "[[Attention Optimization for LLMs]]"
  - "[[AI Coding Guidelines]]"
ai_generated: true
human_approved: false
category:
  - AI
---
#### Design Around KV-Cache
- KV-cache hit rate is the single most important metric for production AI agents
- Cached input tokens cost 10x less than uncached ones (Claude Sonnet: 0.30 vs 3 USD/MTok)
- Keep prompt prefix stable—avoid timestamps at beginning of system prompt
- Make context append-only, avoid modifying previous actions
- Use deterministic serialization (watch JSON key ordering)

#### Use File System as Context
- Treat file system as unlimited, persistent context
- Models learn to write/read files as externalized memory
- Always make compression strategies restorable (keep URLs, file paths)
- Avoid irreversible compression—you can't predict which observation becomes critical later

#### Manipulate Attention Through Recitation
- Create and update todo.md files step-by-step
- Constantly rewrite objectives into end of context
- Pushes global plan into model's recent attention span
- Avoids "lost-in-the-middle" issues and goal misalignment

#### Keep the Wrong Stuff In
- Leave failed actions and errors in context
- Error recovery is a key indicator of true agentic behavior
- Model learns from mistakes and shifts priors away from similar actions
- Don't clean up traces—embrace failure as part of the loop

#### Don't Get Few-Shotted
- Models are excellent mimics—they follow patterns in context
- Too much uniformity leads to drift and overgeneralization
- Introduce structured variation in actions/observations
- Use different serialization templates, alternate phrasing, controlled randomness

## References
https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus


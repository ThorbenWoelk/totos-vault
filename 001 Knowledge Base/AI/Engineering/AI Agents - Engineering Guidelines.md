---
created: 2025-07-25
last_edited: 2026-04-29
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
## Best Practices

### Multi-vs-single agent
**The architecture decision matrix**

The core principle for developing agentic systems is to treat a strong single-agent baseline as the default starting point, not a weak baseline to be replaced. Developers should only scale complexity when the workload characteristics demand it.

To test architectures internally, developers need to implement a strict evaluation discipline. With open source models, it is easier to measure reasoning traces and control token budgets. With closed models, you have to use workarounds and proxies such as costs and response times. Overall, having a thorough logging system helps keep tabs on the cost/accuracy tradeoffs of your agentic architecture.

Here is how you can map your architecture choices to your workload properties:

- **Is the integration tool-heavy (>10 tools)?** Build a single-agent system. If a multi-agent system is strictly required, use a decentralized topology for better parallel efficiency.
- **Is the system failing due to reasoning depth?** Stick to a single-agent architecture, but implement pre-answer scaffolding in your prompts to force the model to compute longer.
- **Is the system failing due to context degradation?** If you are passing massive, noisy, or contradictory inputs that break a coherent context window, move to a multi-agent system for filtering and structuring.
- **Are there natural decomposition boundaries?** If sub-tasks can be processed entirely independently, move to a multi-agent system. If tasks need to be accomplished sequentially, stay with a single-agent design.
- **Does the output require strict regulatory verification?** Move to a centralized multi-agent system and program the orchestrator to aggressively check for logical contradictions and context omissions before aggregating the final output.

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

- Manus. (2025, July 18). [Context engineering for AI agents: Lessons from building Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus). Manus.

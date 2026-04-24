---
created: 2025-12-19
last_edited: 2026-04-24
tags:
- ai
- case-study
- decision-support
- corporate-use
- integration-issues
- model-issues
- large-language-models
connections:
- '[[001 Knowledge Base/AI/_index|AI Index]]'
- '[[2025-11-26 Reward Hacking and Misalignment]]'
- '[[2025-12 AI Recap]]'
ai_generated: false
human_approved: true
category:
- AI
---
# The age of AI-influence everywhere

A large card-game website recently (2025-12) updated their scoring to measure which cards go well together in deck building. The score was replacing a 10-year old version that tried to do the same thing but has several issues, e.g. pushing staples to the top. For fun, i typed in the following query into chatgpt in 2 minutes:

> come up with a score that measures in a trading card game which cards go good together. it should be based on data containing decks the users are playing. it should be symmetric meaning the score of card A to B should be equal to B to A. Also, it should not push staples that are played everywhere anyways.

It gave me a surprisingly similar score and even some possible opportunities to improve this further. 

So I think the question if AI is useful or not is settled. It has a huge impact, it supports decision making in actual corporate life. It gives developers a head start on difficult problems to solve.

# Still young

Many moving parts. APIs aren't stable, yet. As a result, incompatibility between, e.g. model outputs and agentic frameworks. 

> E.g. technical issue discovered with Pydantic AI 0.5.0 and Ollama's gpt-oss:20b model integration. The gpt-oss:20b model (OpenAI's open-source reasoning model) sends chain-of-thought reasoning in `delta.reasoning` instead of `delta.content` during streaming. Pydantic AI doesn't extract this field, yet, causing loss of reasoning information.


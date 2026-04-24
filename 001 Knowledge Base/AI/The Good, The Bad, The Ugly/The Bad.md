---
created: 2025-09-01
last_edited: 2026-04-24
tags:
- ai-limitations
- context-drift
- code-deletion
- custom-implementations
- creativity-limitations
- search-issues
- revision-artifacts
- destructive-laziness
connections:
- '[[Evals for AI Systems]]'
- '[[Vibe Engineering]]'
- '[[The Ugly]]'
ai_generated: false
human_approved: false
category:
- AI
---
# Preface

The bad stuff - as well as the good stuff - changes quickly. In 2024, we thought LLMs might never be able to do maths. In 2025, we knew better. Only one example of our belief system being shattered. Below, some of the still relevant, annoying things AI models suffer from are listed.

# General 

## Compounding Errors and hallucinations

Because the model samples from a probability distribution for every prediction, a single early mistake shifts the context and causes subsequent predictions to diverge exponentially.

This compounding error rate makes hallucinations a fundamental flaw rather than a fixable bug.

# Coding

## Documentation and User Guides

AI is not good at evaluating what are important details and what is implicit knowledge the user already gains from, e.g. UI cues. It is oddly specific in some areas and does not mention important concepts in others. 

## Context Amnesia / Context Drift 

#2026-01 
**AI Runs away with non-intended instructions**
Reviewing a codebase, AI can stumble across a TODO.md file with instructions to work on the code. It is quick to forget the original instructions and just builds away. Bet there is a name for this. 

## Deletes the annoying stuff

... and sometimes your home directory. 

But in a less serious sense, even the best AI coding models are still (2025-12) prone to just delete stuff they can't get under control: 

> I found the issue: The logic for generating the preview was removed in a recent commit to fix build errors. I plan to restore it properly.

## BYO Custom Slob Generator

AI loves custom implementations where proper solutions already exist. Even when using the frameworks in question already. 

# Others
## Creativity
AI is not creative. It does not generate new ideas. It does not really understand subtleties. It does not understand the big picture behind a statement in context.

## Slop
I'm finding myself questioning youtube videos of landscapes and travel destinations. I'm wondering when might be the first time when what I see is not even real. 

## Needle in a haystack

AI models don't find text in large documents reliably. Google has a [package](https://github.com/google/langextract?utm_source=alphasignal&utm_campaign=2025-08-11&asuniq=5735757a) to improve that. 

# ... Archive
## Revision accumulation: Artifacts of editing in writing 

**Document evolution artifacts** or **Revision Accumulation** captures the fact that hints to previous versions of a document are included by the AI in a document it's creating. 

Example headline: Updated Developer Guide (cleaned)

### Edit
This is not as prevalent in late 2025 as it was in early 2025 and depends on harness (AI client) more than model itself.

## Finding data

40% failure rate to find data in a large dataset for (older) Haiku 4.5 in optimized [Toon](https://github.com/toon-format/toon) data structure. 

## MCP

The original MCP problem: tool definitions fill the context window upfront — ten definitions can critically saturate the window — and every intermediate result stacks up as the agent loops. For large document workflows, the same data passes through the model multiple times.

**The fix: code execution with MCP.**

Instead of handing the model all tool definitions at once, present MCP servers as code APIs on a filesystem. The agent reads only the tools it needs, when it needs them. A Salesforce and Google Drive example that would have consumed 150,000 tokens through direct tool calls costs 2,000 tokens through code execution — a 98.7% reduction.

Three concrete mechanisms:

- **Progressive disclosure** — the agent navigates a file tree of available tools, loading definitions on-demand rather than all at once. Includes search tools that let the agent filter by detail level (name only, name and description, full schema).
- **Context-efficient filtering** — large datasets are processed in the execution environment before reaching the model. A 10,000-row spreadsheet can be filtered to five rows of interest; the model never sees the full set.
- **Privacy-preserving data flow** — the MCP client intercepts PII and tokenizes it before it reaches the model. Email addresses, phone numbers, names flow from Google Sheets to Salesforce without ever entering the model's context. Security rules become deterministic: you define where data can flow, not whether the model accidentally logs it.

**Control flow in code** replaces the agent loop for conditionals and retries. A deployment notification poller becomes a `while(!found)` loop in code, executing in the sandbox rather than alternating between tool calls and sleep commands. Latency drops too — the execution environment evaluates conditionals without waiting for the model to generate tokens first.

**State persistence and skills** — agents write intermediate results to files and resume later. Reusable code gets saved as named skills that the model can reference across sessions. This ties into the broader concept of agent skills: scaffolding that evolves as the agent learns what works.

The old critique still stands for direct tool-calling patterns. The ecosystem has moved toward code execution as the default harness pattern for serious agent work. Cloudflare calls this "Code Mode." Anthropic describes it as applying established software engineering patterns to the agent loop.

Note: code execution introduces its own complexity — sandboxing, resource limits, monitoring. The operational overhead is real. Weigh it against the token and latency savings before adopting.

https://www.anthropic.com/engineering/code-execution-with-mcp


---
created: 2026-04-16
last_edited: 2026-04-29
tags:
  - openai
  - agents-sdk
  - ai-agent-framework
  - sandboxing
  - agent-harness
  - mcp
  - skills
  - agents-md
connections:
  - "[[AI Agents - Engineering Guidelines]]"
  - "[[AI Tooling and Harnesses]]"
  - "[[AI Agent Protocol Architecture]]"
ai_generated: true
human_approved: false
category:
  - AI
  - AI Agent Frameworks
---
## Core idea

OpenAI's April 2026 Agents SDK update shifts the SDK from a basic orchestration library toward a model-native agent harness: a runtime layer for agents that need to inspect files, call tools, edit code, run commands, and keep working over long tasks in controlled environments.

The important architectural claim is that frontier models need more than API access. They need a harness shaped around how agents actually work: filesystem context, tool calls, shell access, patch application, memory, orchestration, tracing, sandbox state, and repeatable workspace layout.

## What changed

- **Model-native harness**: The SDK now standardizes more of the agent loop instead of leaving teams to compose custom glue around tools, files, and runtime state.
- **Sandbox-aware orchestration**: Agents can run inside controlled computer environments with the files, dependencies, ports, commands, and packages needed for a task.
- **Codex-like primitives**: The announcement explicitly frames the harness around filesystem inspection, shell execution, file edits, `apply_patch`, MCP tools, skills, custom instructions, and AGENTS.md-style guidance.
- **Manifest abstraction**: Developers can describe the agent workspace in a portable way: mounted local files, output directories, and storage-backed data from S3, Google Cloud Storage, Azure Blob Storage, and Cloudflare R2.
- **Sandbox providers**: The SDK can use external sandbox providers, including Blaxel, Cloudflare, Daytona, E2B, Modal, Runloop, and Vercel.
- **Durability**: OpenAI describes snapshotting and rehydration so a run can recover in a fresh sandbox instead of losing state when a container expires or fails.
- **Scale pattern**: Runs can use one sandbox or many, route subagents into isolated environments, and parallelize work across containers.

## Why it matters

This is OpenAI productizing the pattern behind Codex-style work: the model is only one part of the system; the harness determines whether the agent can reliably use context, tools, files, and long-running state.

For enterprise adoption this matters because it makes several previously bespoke concerns first-class:

- controlled execution boundaries
- repeatable workspace setup
- separation between orchestration and untrusted model-generated code
- portable sandbox provider choice
- clearer path from local prototype to production runtime
- observability and resumability for long-horizon tasks

The most strategic part is the separation of **harness** and **compute**. The harness keeps orchestration, state, credentials, and policy control outside the environment where model-generated code runs. That reduces blast radius for prompt injection and exfiltration attempts while still giving the model a useful computer to work in.

## Current constraints

- New harness and sandbox capabilities launch first in Python.
- TypeScript support is planned later.
- Additional capabilities, including code mode and subagents, are planned for Python and TypeScript.
- Pricing follows standard API pricing based on tokens and tool use.

## When I would use it

Use OpenAI Agents SDK when I want to own the app runtime, tools, state, approvals, and infrastructure integration, but do not want to rebuild the agent loop from scratch.

Good fit:

- coding agents
- document analysis agents that need file access
- data-room or due-diligence workflows
- long-running research tasks
- agents that need shell access or package installs
- workflows where prompt injection and credential isolation matter
- production systems that need resumability and traces

Less obvious fit:

- simple single-call model interactions
- pure chat interfaces without tool orchestration
- workflows where hosted Agent Builder is enough
- cases where the execution environment cannot be sandboxed cleanly

## Open questions

- How mature are the sandbox provider integrations under real enterprise networking constraints?
- How much of Codex's practical reliability comes from the SDK primitives versus product-specific policy, prompting, and UX?
- Will TypeScript support be equivalent to Python or lag behind for the most agentic features?
- How portable are Manifests across providers once workflows need credentials, private networks, browsers, or persistent volumes?

## References

- OpenAI. (2026, April 15). *[The next evolution of the Agents SDK](https://openai.com/index/the-next-evolution-of-the-agents-sdk/).* OpenAI.
- OpenAI. (n.d.). *[Agents SDK](https://developers.openai.com/api/docs/guides/agents).* OpenAI API.

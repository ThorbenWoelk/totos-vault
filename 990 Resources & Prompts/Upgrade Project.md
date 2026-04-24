---
created: 2025-12-19
last_edited: 2026-04-24
tags:
- upgrade
- ai-sdk
- typescript
- bun
- testing
- zod
- openrouter
- type-safety
connections:
- '[[Common AI Development Prompts]]'
- '[[AI Benchmark - Programming Task]]'
- '[[Log Analysis]]'
ai_generated: false
human_approved: false
category:
- Prompt library
---
Upgrade this project to use the latest version of the AI SDK (version 5). I have attached a guide on how to do the upgrade below. Please make sure to review the guide before starting your implementation [https://ai-sdk.dev/docs/migration-guides/migration-guide-5-0](https://ai-sdk.dev/docs/migration-guides/migration-guide-5-0)

Make sure that you use the latest version of the AI SDK package, as well as Zod and the OpenRouter provider.

Test your implementation by running the following command:

'bun run dry-run'

Each test should have a result written to the /results/dry-run directory. Make sure you validate these outputs and that the tool calls that they have do include the parameters that were passed by the model. I've had a problem where the output ends up being empty objects instead of the expected output of what the model submitted in the tool call.

Also make sure that all of the TypeScript in the project is validated. We don't want to be shipping any unsafe code in this project. Please avoid using any and any other things that might break the type safety of the system, and make sure you validate the changes that you make using the 'bun run tsc' command.


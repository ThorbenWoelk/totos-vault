---
created: 2025-09-23
last_edited: 2026-04-24
tags:
  - ai-coding
  - best-practices
  - human-in-loop
  - agent-architecture
  - iterative-task-management
  - monitoring
  - templates
  - workflow
  - code-review
  - todo-pattern
connections:
  - "[[AI Agents - Engineering Guidelines]]"
ai_generated: false
human_approved: false
category:
  - AI
---
What does it mean to be good at AI-based coding? This is a collection of things you should learn and do as a software engineer in the age of AI. 

## Start with the task 

A good agent task:
- is scoped so that agent doesn't need to read entire codebase
- is a closed loop. I.e. the agent has a way to evaluate its own work
- is not mission critical - boring stuff, side projects, rubber-ducking

[Mario Zechner (2026), YouTube](https://www.youtube.com/watch?v=RjfbvDXpFls&list=WL&index=2)

## Choosing the right model for the right task

It's the job of the developer to decide how complicated an issue is. When letting AI analyze and fix a critical but, it might be that only the highest reasoning models go a particular route and find the issue in the depth of the dependency tree. Choosing a lower reasoning model would then be a waste of tokens and energy in the best case and hallucinating new bugs in the worst case.

## Codebase agent readiness
#2026-01 https://factory.ai/news/agent-readiness

## Basic workflow

#2026-03
- There are four distinct "species" of agents in production use, each suited to different problems:
	- Coding Harnesses (individual or project-scale), 
	- Dark Factories (fully autonomous with eval gates), 
	- Auto Research (metric optimization), and 
	- Orchestration Frameworks (workflow routing with specialized roles
- I use Coding Harnesses. They use skills and guidelines I and their devs wrote. 
- Basically still:
	- creating specs or minimal prompt
	- implementing + auto testing + manually testing 
	- iterate...
- **Yolo projects**: I don't spawn subagents. Sometimes the harness does. I don't run multiple worktrees, it's too much overhead. Most of the time I run multiple agents on different parts of codebase in same project and branch. Messy git log but fast. I push to main. Things break but tests help. I try to avoid multi-project work. The context switch is too taxing. 
- **More control**: use gt. One change at a time. 

#2025-12
- Create a plan. A prompt can create a plan. Once verified, the plan can then be used for the implementation. 
- A verification harness. Tests or app to run, scripts to run that create an artifact from which the model can learn what it did wrong. 
- Context. "Here is how I like to do things". Nowadays, the models get actually *better* once they see a larger codebase containing good practice examples. 
- Instead of letting model fix errors after an unsuccessful run, it might be better to inspect where it went wrong, adapt the plan, and run again. 
https://www.youtube.com/watch?v=-g1yKRo5XtY&t=653s (2025-12)

## Iterative task management

**Key insight from Manus**: let agent update a `todo.md` file. 

### Keep Human in the Loop - Always!
Humans are the most critical for providing context awareness that the models lack:
- **Environment**: E.g. poor internet connection and connection errors might make AI assume the code needs fixing
- **Implementation awareness**: AI may rebuild code or libraries that already exist like an overconfident coder who doesn't know how to do their research
- **Sanity checks**: Obvious mistakes to humans, not necessarily to AI

### Guidelines, Rules, and Context

Establish:
- `AGENTS.md` /`GEMINI.md` /`WARP.md` / `.cursorrules` / `CLAUDE.md` / `guidelines.md`  ... files in repo root and subfolders
- For some IDEs, you also set rules in settings
- `llms-full.txt` files exist for many libraries. You may also dump your repos docs fully into this format to provide it as additional context to agents

### Combat Code Base Explosion
Vibe coding is prone to exploding codebase fast:
- Integrate AI review and refactoring sessions often
- Use AI in PR review
- Use pre-commit hooks, linting, testing (commitizen, ruff, black, vitetest) to e.g. catch unused variables, legacy code, premature commits
- **Peer review**: 
	- For non-critical code... skip?
	- For critical code: Treat AI changes like any developer's work and peer review every line... yourself!

### Learning vs Speed Trade-off
The more time you take to 
- read and review the code
- let AI explain what it did and why
- chat about things you don't understand

The more you will learn and improve. You'll not learn anything if you only sit around with a blank stare, watching the results appear. 

The risk of AI coding:
- Forcing speed and productivity, sacrificing knowledge gain. Learning tends toward 0 the more vibe-style we code
- Productivity dividend on the time spent coding decreases
- Junior developers miss fundamental software engineering concepts
- Basic concepts still needed when vibe-coding - often to guide the AI
### Context Truncation Strategy
Based on Manus insights:
- Prefer file-based memory over context compression
- Keep references (URLs, file paths) even when dropping content
- Make all compression reversible
- Use filesystem as unlimited persistent context


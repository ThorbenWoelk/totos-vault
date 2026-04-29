---
created: 2025-12-19
last_edited: 2026-04-29
tags:
  - ai-models
  - ai-harnesses
  - security-approval
  - provider-variability
  - model-quality
connections:
  - "[[001 Knowledge Base/AI/_index|AI Index]]"
  - "[[2026-03]]"
ai_generated: false
human_approved: false
category:
  - Tools & Setup
  - AI Tools
---
# Current Setup

#2026-04 

For a month now, I've been using Codex app with a $200 subscription (that I got for free on an [AI Beavers](https://ai-beavers.com/) Meetup - shoutout to them). Before, I've been switching between several harnesses to max out smaller subscriptions: Antigravity, Warp, Cursor, Claude Code. 

Editor: Cursor
Shell: Warp

I manage my SKILLS.md setup in a central space and symlink all harnesses' skill locations to them. 

## Codex App Workflow

I work on 1-4 chats and projects at the same time. I heavily use queuing to push the time-till-next-input up and reduce the mental tax that comes with context-switching.

## Most important skills - here to stay

**Skill Management Skill** - it tells AI how to manage my skills so that it can set up a new one or edit an existing one without messing up symlink structure or templating approach.

**Ship it!** - How we ship

**Openclaw Setup** - The skillI use for AI to manage my openclaw server and setup. 

## Questionable

**tskills CLI** - CLI to help templating skills and skill-execution (set up with a similar idea to what gstack does). 

**Agent TOMLs** - A set of what Codex calls "prompts". As codex doesnt use them by it's own and they need to be explicitly invoked, I'm not sure if I'll keep them.

## Quality

The harness you use to run AI models matters a lot for the quality of results of your agents. For instance, Anthropic models perform differently when run inside Claude Code or Cursor. 

Harnesses - as well as models - don't necessarily get better all the time. Sometimes, like in early 2025 (Claude Code) or early 2026 (Codex), they decline in quality. For instance when a new model comes out and the AI company has a hard time managing the rush on their hardware. After a while, they might cook again (e.g. at the end of 2025 when ralph wiggum pattern made Claude Code taking over competition entirely). 

## List of Harnesses I know enough about to list them

Codex App & CLI, Claude Code, Antigravity & gemini CLI, Warp, Kilo, Crush, Windsurf, Mistral vibe CLI.


Frequently changing providers and harnesses makes it all the more challenging for companies that usually have a long security and approval process to allow the use of a new technology.


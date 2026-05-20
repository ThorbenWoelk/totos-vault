---
created: 2025-07-22
last_edited: 2026-04-29
tags:
- obsidian
- git
- mcp
- configuration
- plugins
- infrastructure
- version-control
- setup
connections:
- '[[OpenAI Agents SDK]]'
ai_generated: false
human_approved: false
category:
- AI Memory
- Procedures
tagging_processed_count: 10
tagging_last_processed: 2026-05-15
---
Configuration documentation for the AI memory vault infrastructure.

## Git Versioning
- **Plugin**: Obsidian Git
- **Purpose**: Version control for vault contents

## MCP Integration

**Purpose**: Enables AI to read/write vault content via Model Context Protocol

## Required Plugins

Shared across all three vaults (`totos-vault`, `totos-journal`, `totos-job-vault`). Sync script: `/Users/thorben.woelk/repos/_obsidian/scripts/sync-obsidian-setup.sh`.

- Obsidian Git — version control
- Local REST API — MCP connectivity
- Local GPT and AI Providers — in-vault AI
- Dataview and Meta Bind — queries and forms (Skill Map in job vault)
- Virtual Linker — glossary-style links
- Open in Terminal — dev workflow

---
*Configuration enables seamless AI-human knowledge collaboration*

## References

- coddingtonbear. (n.d.). [obsidian-local-rest-api](https://github.com/coddingtonbear/obsidian-local-rest-api). GitHub.
- Dickson, P. (2025, April 13). [Automate note generation in Obsidian with AI Claude and MCP servers](https://www.youtube.com/watch?v=NnwwpASG1_4&t=125s). YouTube video.
- MarkusPfundstein. (n.d.). [mcp-obsidian](https://github.com/MarkusPfundstein/mcp-obsidian). GitHub.
- Obsidian Git. (n.d.). [Start here](https://publish.obsidian.md/git-doc/Start+here). Git Documentation.

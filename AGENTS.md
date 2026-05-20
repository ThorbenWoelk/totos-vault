---
created: 2025-10-06
last_edited: 2026-04-24
tags:
- vault-guidelines
- documentation
- style
- testing
- knowledge-management
- markdown
connections:
- '[[Documentation Standards]]'
- '[[2025-11-27 App as Markdown Specs]]'
- '[[AI Coding Guidelines]]'
- '[[Levels of AI Autonomy]]'
ai_generated: false
human_approved: false
category:
- Vault
- Repository Guidelines
tagging_processed_count: 10
tagging_last_processed: 2026-05-15
---
## Welcome to the vault

This is an Obsidian vault. It is a collection of .md files.

### Guidelines
- Rule 0: keep this vault fresh
- Include standard properties in every file

### Format
- First line after properties is a headline, `##` for major sections, skip `#` as Obsidian uses filenames as titles. 
- Use 1 empty line after each title and text paragraph.
- For tabular data, stick to Markdown tables and avoid embedded HTML unless required.

## Style

Favor clear sentence structure and instructional tone to make this vault as easy to read and understand as possible. Favor information-dense wording without compromising ease of interpretation. 

## Graph

We're intentionally improving the graph-structure of this vault:
- connect related notes directly through body links and the `connections` property
- avoid synthetic map files as an artificial graph layer

## Testing Guidelines

- Manually review links: follow internal wikilinks inside Obsidian and confirm external URLs resolve. 
- If using markdownlint or spell-check extensions, resolve warnings before requesting review.

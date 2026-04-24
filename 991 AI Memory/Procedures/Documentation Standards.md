---
created: 2025-07-22
last_edited: 2026-04-24
tags:
- documentation-standards
- frontmatter
- tagging
- title-standards
- file-structure
- metadata
connections:
- '[[Tagging System]]'
- '[[Learning Protocol]]'
- '[[Local Speaker Discovery and Control]]'
ai_generated: false
human_approved: false
category:
- AI Memory
- Procedures
---
Guidelines for creating consistent, readable, and searchable notes across the Obsidian vaults.

## Frontmatter Standard

Every Markdown note keeps the same seven standard properties. Order matters because the standardizer writes fields in this order.

```yaml
---
created: YYYY-MM-DD
last_edited: YYYY-MM-DD
tags:
- tag-one
- tag-two
connections: []
ai_generated: false
human_approved: false
category:
- Folder
- Subfolder
---
```

### Protocol for Frontmatter Fields

1. **`created`**: Date of creation in `YYYY-MM-DD` format.
2. **`last_edited`**: Date of last modification in `YYYY-MM-DD` format.
3. **`tags`**: Short list of relevant type and topic keywords.
4. **`connections`**: Explicit wiki-links such as `[[Note Name]]`. Use `[]` if none.
5. **`ai_generated`**: `true` when AI created the note body or meaningfully rewrote it.
6. **`human_approved`**: `true` only after explicit human approval.
7. **`category`**: Folder path from the vault root to the note's parent folder.

The standardizer fills missing fields, normalizes timestamp values to date-only strings, and derives `category` from the note path.

## Tagging Strategy

1. **Format**: All tags are lowercase and kebab-case, for example `neural-network` and `machine-learning`.
2. **Signal**: Prefer a few useful tags over a long loose list.
3. **Shape**: Use type tags such as `concept`, `guide`, `resource`, `snippet`, `procedure`, or `reference`.
4. **Topic**: Add domain tags such as `python`, `pandas`, `psychology`, `finance`, `obsidian`, or `metadata`.
5. **Composite Tags**: Keep composite tags when splitting loses meaning, such as `machine-learning`.
6. **No Hash Symbols**: Store tags without `#` in frontmatter.

For vault-wide cleanup, use deterministic property and formatting passes. Tag notes directly from their content. Do not delegate vault tagging to Ollama.

## Title Standards

### No Redundant Headlines

- Do not start a note with a headline that duplicates the filename.
- The filename is the title in Obsidian.
- Start content with a purpose sentence or the first useful section.

### Concise Conceptual Titles

- Good: `Software Design Principles`
- Good: `DevOps Principles`
- Bad: `Software Design Principles - Comprehensive Guide`
- Bad: `Complete Guide to DevOps Best Practices`

Titles name concepts. They do not sell the note.

### Examples

- Bad: `DevOps Principles.md` starting with `# DevOps Principles`
- Good: `DevOps Principles.md` starting with `Core practices for...`
- Bad: Any document where tab title and first line are identical.
- Good: Content flows without repetition.

## Document Architecture

### Opening Structure
```markdown
Brief description or purpose statement

## First Section
```

### File Boundaries

- Start text immediately after frontmatter. Do not leave an empty line between the closing `---` and the first content line.
- End every document with one empty line.
- Do not use importance score lines.

### Consistent Sections

- Start with a brief purpose sentence.
- Use parallel section headers.
- Keep section headings scannable.
- Add a timestamp or relevance note only when it helps future use.

## Pre-Creation Checklist

- [ ] No headline duplication (filename ≠ first line)
- [ ] Concise conceptual title (not descriptive marketing copy)
- [ ] Content starts meaningfully
- [ ] No importance score line
- [ ] No empty line before the first body line
- [ ] Empty line at the end of the document
- [ ] Clear category placement
- [ ] Proper metadata included (all 7 fields)
- [ ] Consistent formatting applied
- [ ] Tags are lowercase and kebab-case

## User Experience Principles

- The tab and breadcrumb show the filename as the title.
- The first content line adds information.
- The note scans cleanly.
- The hierarchy is visible at a glance.


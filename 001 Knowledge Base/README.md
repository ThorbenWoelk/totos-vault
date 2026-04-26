---
created: 2025-07-22
last_edited: 2026-04-24
tags:
- knowledge-base
- concepts
- guidelines
- technical-documentation
connections: []
ai_generated: false
human_approved: false
category:
- Knowledge Base
---
Structured repository of concepts, references, guidelines, and technical documentation.

## Start Here

Use [[001 Knowledge Base/_index]] and the generated category indexes as inventories. Use note-level `connections` and body links for navigation.

## Generated Indexes

The generated `*_index.md` files are maintained by `wiki-compiler`. Use them as inventories.

## Graphify

Use Graphify on the Knowledge Base as a structural audit:

- target: `<obsidian-root>/totos-vault/001 Knowledge Base`
- outputs:
  - `<graphify-output-dir>/graph.html`
  - `<graphify-output-dir>/graph.json`
  - `<graphify-output-dir>/GRAPH_REPORT.md`
Use it to find weak bridges and noisy note families. Fix those notes directly. Do not add synthetic wrapper notes just to improve the rendered graph.

## Purpose

Build a comprehensive knowledge repository for learning, retrieval, and reuse. The shape should stay simple: generated indexes list notes; individual notes create the graph through body links and `connections`.

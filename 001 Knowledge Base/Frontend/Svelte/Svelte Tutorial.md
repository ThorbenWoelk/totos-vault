---
created: 2026-04-24
last_edited: 2026-04-29
tags:
- svelte
- frontend
- svelte-tutorial
- react-comparison
- component-lifecycle
connections:
- '[[001 Knowledge Base/_index|Knowledge Base Index]]'
- '[[UI Design Vocabulary]]'
ai_generated: false
human_approved: false
category:
- 001 Knowledge Base
- Frontend
- Svelte
tagging_processed_count: 1
---
## Component Rendering

In React the entire component is re-rendering when state changes. 
Svelte works differently: the component 'runs' once, and subsequent updates are 'fine-grained'. This makes things faster and gives you more control but might have unintended consequences if not careful with component constants.

## References

- Svelte. (n.d.). *[Keyed each blocks](https://svelte.dev/tutorial/svelte/keyed-each-blocks).* Svelte Tutorial.

---
created: 2026-04-24
last_edited: 2026-04-24
tags:
- learning-session
- rust
- programming
connections:
- '[[raw/_index|Raw Source Index]]'
- '[[Rust]]'
- '[[Technical Vocabulary Ledger]]'
ai_generated: true
human_approved: false
category:
- programming
type: learning-session
topic: Rust method call syntax
---
## Rust Method Call Syntax

## Session Summary

- 2026-04-24 20:29 - Compared Rust method-call syntax with Python method calls while learning iterator pipelines.

## Condensed Facts

- 2026-04-24 20:29 - In Rust, `receiver.method(arg)` and `Type::method(receiver, arg)` can call the same method. The receiver becomes the first argument in the associated-function form.
- 2026-04-24 20:29 - `str::trim` is a function item referring to the `trim` method on `str`. It can be passed to iterator adapters such as `.map(str::trim)` when the input item type matches.
- 2026-04-24 20:29 - `.map(str::trim)` and `.map(|line| line.trim())` are equivalent when the iterator yields string slices compatible with `str::trim`.

## Mental Models

- 2026-04-24 20:29 - Rust method syntax is call sugar resolved at compile time: `line.trim()` can be understood as `str::trim(line)`.
- 2026-04-24 20:29 - Python has a surface analogy with `line.strip()` and `str.strip(line)`, but Python uses its runtime object model and descriptor binding.

## Examples

```rust
let text = "  hello  ";

let a = text.trim();
let b = str::trim(text);

assert_eq!(a, b);
```

```rust
input.lines().map(str::trim)
input.lines().map(|line| line.trim())
```

## Open Questions

- 2026-04-24 20:29 - How Rust resolves method calls when multiple traits provide methods with the same name.

## Source Thread

- Started from Codex learning-session on 2026-04-24.


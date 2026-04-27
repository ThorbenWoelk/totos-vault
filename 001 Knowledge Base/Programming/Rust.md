---
created: 2026-01-07
last_edited: 2026-04-24
tags:
  - rust
  - axum
  - serde
  - declarative-macro
  - procedural-macro
  - literals
connections:
  - "[[001 Knowledge Base/Programming/_index|Programming Index]]"
  - "[[2026-04]]"
  - "[[001 Knowledge Base/Tech/Vocabs/AGENTS.md]]"
  - "[[UI Design Vocabulary]]"
ai_generated: false
human_approved: false
category:
  - Knowledge Base
  - Programming
---
### literal
A literal is a piece of source code that directly represents a value, without any function call or variable lookup.
```
{
    "name": "Alice",
    "age": 7,
    "online": true,
}
```

### declarative macro
A macro is code that writes other code (like `json!` in the example).

implements `macro_rules!`

```
let v = json!({
    "name": "Alice",
    "age": 7,
    "online": true,
});
```

### procedural macro

```
(input: TokenStream) -> TokenStream
```

A function annotated with `#[proc_macro]`, `#[proc_macro_derive]`, or `#[proc_macro_attribute]` that lives in a `proc-macro = true` crate.

```
use proc_macro::TokenStream;


#[proc_macro]
pub fn repeat_expr(input: TokenStream) -> TokenStream {
```

## Some important crates

`Axum` for http
`serde` for (de-)serialization


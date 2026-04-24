---
created: 2025-12-19
last_edited: 2026-04-24
tags:
- tagging-system
- automation
- metadata
- standardization
- procedure
- ai-memory
- obsidian-standardizer
connections:
- '[[Documentation Standards]]'
- '[[Local Speaker Discovery and Control]]'
- '[[991 AI Memory/Procedures/AGENTS|AGENTS]]'
ai_generated: false
human_approved: false
category:
- AI Memory
- Procedures
---
## Tagging System

The vault uses one Rust service to keep note properties complete and tags useful. The service scans Markdown notes, fills missing standard frontmatter, normalizes date fields, derives folder categories, and asks an Ollama-backed Rig agent to maintain the `tags` list.

Primary implementation:

- `<vault-agent-root>/packages/obsidian-standardizer`

Normal lifecycle entrypoints:

- `<vault-agent-root>/start_app.sh`
- `<vault-agent-root>/end_app.sh`

Compatibility shim:

- `apps/vault_tagger.py` prints migration guidance and does no tagging work.

## Standard Frontmatter

Every note keeps these properties:

```yaml
created: <YYYY-MM-DD>
last_edited: <YYYY-MM-DD>
tags: []
connections: []
ai_generated: false
human_approved: false
category: []
```

The standardizer applies these rules:

- Missing keys are added automatically.
- Existing values are preserved.
- `created` and `last_edited` are normalized to date-only (`YYYY-MM-DD`).
- Non-string date values are replaced with filesystem-derived dates.
- `category` is derived from vault-relative parent folder names.
- Frontmatter is written with the seven standard keys first.

Example category:

```yaml
category:
- Tools & Setup
```

## Scanned Files

The scanner processes vault Markdown files under `<obsidian-root>/totos-vault`.

Skipped paths:

- Hidden files and hidden directories.
- `node_modules`.
- `target`.
- `.venv`.
- Vault-relative `apps/data` paths.

## AI Tagging

AI tagging runs after metadata normalization unless the command uses `--dry-run` or `--metadata-only`.

The tagging agent:

- Uses Rig with Ollama.
- Uses model `glm-4.7-flash:latest` by default.
- Reads the note with `read_note`.
- Replaces the `tags` property with `update_tags`.
- May add, replace, or delete tags when the note content supports it.
- Can be limited to untagged notes with `--tag-empty-only`.

Constraints:

- Maximum `10` tags per note.
- Tags are sanitized to lowercase kebab-case.
- Leading `#` is removed.
- Single-word tags are preferred when meaning stays clear.
- Composite tags stay when splitting would lose meaning, such as `machine-learning`.
- Generic two-part composites may be split, such as `ai-tools` into `ai` and `tools`.
- Empty or duplicate tags are removed.

AI processing is tracked in frontmatter:

```yaml
tagging_processed_count: 1
tagging_last_processed: 2026-04-24
```

`tagging_processed_count` increments on each AI run and stops at `10`. Notes at `10` are skipped by the AI tagging pass.

## Tagging Conventions

- Tags are lowercase and kebab-case.
- Use tags for note type and topic.
- Prefer high-signal tags over broad filler.
- Keep the tag list short enough to scan.
- Use type tags such as `concept`, `person`, `resource`, and `definition`.
- Use topic tags such as `misophonia`, `adhd`, and `productivity`.
- Store tags without `#` in frontmatter.

## Run The Service

Use the lifecycle scripts for regular operation.

Start the metadata and tagging service:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
cd "$VAULT_AGENT_ROOT"
./start_app.sh --standardizer
```

Start it in the background:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
cd "$VAULT_AGENT_ROOT"
./start_app.sh --standardizer --detach
```

Stop services started through the lifecycle script:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
cd "$VAULT_AGENT_ROOT"
./end_app.sh
```

The lifecycle script builds the release binary, starts local Ollama on `127.0.0.1:11435` when `OLLAMA_API_BASE_URL` is unset, runs the standardizer against `totos-vault`, and writes logs under:

```text
<vault-agent-root>/.run/logs
```

Useful environment overrides:

| Variable | Used for |
| --- | --- |
| `VAULT_ROOT` | Vault path scanned by the standardizer. |
| `STANDARDIZER_TAG_MODEL` | Ollama model passed to the standardizer. |
| `OLLAMA_API_BASE_URL` | Existing Ollama endpoint. |
| `LOCAL_OLLAMA_HOST` | Local Ollama host and port started by `start_app.sh`. |

## Manual Commands

Use direct Cargo commands for one-off checks and maintenance.

Run one full pass:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
VAULT_ROOT="<obsidian-root>/totos-vault"
cd "$VAULT_AGENT_ROOT/packages/obsidian-standardizer"
OLLAMA_API_BASE_URL=http://127.0.0.1:11435 cargo run -- --vault-root "$VAULT_ROOT" --once
```

Run continuously:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
VAULT_ROOT="<obsidian-root>/totos-vault"
cd "$VAULT_AGENT_ROOT/packages/obsidian-standardizer"
OLLAMA_API_BASE_URL=http://127.0.0.1:11435 cargo run -- --vault-root "$VAULT_ROOT"
```

Preview metadata changes without writing files or running AI tagging:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
VAULT_ROOT="<obsidian-root>/totos-vault"
cd "$VAULT_AGENT_ROOT/packages/obsidian-standardizer"
cargo run -- --vault-root "$VAULT_ROOT" --once --dry-run
```

Write metadata normalization without AI tagging:

```bash
VAULT_AGENT_ROOT="<vault-agent-root>"
VAULT_ROOT="<obsidian-root>/totos-vault"
cd "$VAULT_AGENT_ROOT/packages/obsidian-standardizer"
cargo run -- --vault-root "$VAULT_ROOT" --once --metadata-only
```

Tune AI request behavior:

```bash
VAULT_ROOT="<obsidian-root>/totos-vault"
cargo run -- \
  --vault-root "$VAULT_ROOT" \
  --once \
  --tag-empty-only \
  --tag-model glm-4.7-flash:latest \
  --max-tags 10 \
  --agent-max-retries 5 \
  --rate-limit-base-seconds 15 \
  --agent-pause-ms 300 \
  --agent-read-chars 12000
```

## Maintenance

- Run `./start_app.sh --standardizer --detach` to keep metadata and tags fresh.
- Use `--dry-run` before broad cleanups.
- Use `--metadata-only` when only required frontmatter should be normalized.
- Review changed notes after a full AI tagging pass.
- Investigate files reported as YAML errors; invalid frontmatter is skipped.
- Use the Rust standardizer for tagging work.

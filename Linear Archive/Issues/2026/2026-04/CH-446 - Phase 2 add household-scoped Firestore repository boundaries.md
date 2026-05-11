---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- linear
- linear-archive
- done-issue
connections: []
ai_generated: true
human_approved: false
category:
- Linear Archive
- Issues
- "Linear Archive/Putz & Munter"
---

# CH-446 - Phase 2: add household-scoped Firestore repository boundaries

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-446/phase-2-add-household-scoped-firestore-repository-boundaries
- **State:** Done (completed)
- **Priority:** Low
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:31:57.562+00:00
- **Completed:** 2026-04-28T13:37:23.898+00:00
- **Updated:** 2026-04-28T13:37:23.910+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Add household-scoped Firestore repository boundaries for rooms, tasks, logs, members, pets, subscriptions, and cache keys.
* Make household scope explicit in every repository method signature.
* Move data access toward paths like households/{householdId}/rooms/{roomId} or enforced household_id filters.
* Keep migration execution and broad tenancy test matrix in separate follow-up issues.

Acceptance criteria:

* Backend read/write methods require household context.
* Cache and websocket/update flows include household scope in keys or channels where touched.
* No new global chore-domain query is introduced.

Validation:

* cargo fmt --check
* cargo test
* Focused repository tests or compile-time proof for household context.

Blocker rules:

* Depends on CH-444 model decisions.
* Do not run destructive migration against Firestore.
* If current data shape blocks full path migration, add compatibility adapters and file a migration follow-up.

## Attachments

- [CH-446: scope Firestore room listeners by household](https://github.com/ThorbenWoelk/putz-munter/pull/6) (github)

## Comments

### 2026-04-28T13:33:22.255+00:00 - thorben

Coordinator fix: Firestore listener and repository parents now use the full documents path (`projects/.../documents/households/{householdId}`) instead of relative `households/{householdId}`. Local validation passed: `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo test state::db::tests`, and full `cargo test`.

### 2026-04-28T13:26:00.556+00:00 - thorben

Unblocked: CH-445 is merged on main. Resume CH-446 from current origin/main as verification and gap-fill for repository boundaries. First inspect the landed CH-445 Firestore scoping before editing; keep any remaining changes narrow.

### 2026-04-28T13:06:21.196+00:00 - thorben

Correction to the coordination note: CH-446 is blocked because CH-445 is actively changing backend/src/state/db.rs, backend/src/state/store.rs, and related auth/route files. Resume CH-446 after CH-445 lands or is explicitly split, then rebase from current origin/main.

### 2026-04-28T13:06:14.611+00:00 - thorben

Coordination update: blocked for now because CH-445 is actively changing the same Firestore/store files (, ) plus auth route paths. Resume CH-446 after CH-445 lands or is explicitly split, then rebase from current .

### 2026-04-28T13:04:26.495+00:00 - thorben

## Codex Workpad

Plan:
- Branch from current `origin/main` after CH-445 landed.
- Inspect active Putz & Munter issues and open PRs for overlap.
- Preserve CH-445 access-context routing and cache shape.
- Fill the remaining repository boundary by making Firestore room listeners require household context and emit scoped room updates.
- Add focused repository tests for household path and stable path-safe IDs.

Acceptance criteria:
- Backend read/write methods require household context: met. CH-445 already threaded `household_id` through rooms, tasks, logs, members, pets, and subscriptions; CH-446 now also scopes `listen_rooms(household_id)`.
- Cache and websocket/update flows include household scope where touched: met. Existing cache and websocket update paths remain household-keyed; listener updates now carry `RoomUpdate { household_id, room }`.
- No new global chore-domain query is introduced: met. The room listener now targets `households/{householdId}/rooms` instead of root `rooms`.

Validation:
- Passed: `cargo fmt --check`
- Passed: `cargo test` (36 tests)
- Passed: `cargo test state::db::tests` (focused repository tests)

Blockers:
- None. CH-445 is merged on `origin/main`; no open PRs overlapped this file set.

Commit:
- `45624d0` - `fix: scope Firestore room listeners by household`

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/6

### 2026-04-28T12:56:40.230+00:00 - thorben

Queued as the backend repository lane for Symphony after CH-444. Make household context explicit in Firestore access. If CH-445 is still changing the same auth, route, or store files, record the overlap as a blocker instead of racing the branch.

### 2026-04-28T11:03:48.991+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-446 - Phase 2 add household-scoped Firestore repository boundaries.json`

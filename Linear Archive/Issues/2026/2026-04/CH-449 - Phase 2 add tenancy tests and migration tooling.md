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

# CH-449 - Phase 2: add tenancy tests and migration tooling

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-449/phase-2-add-tenancy-tests-and-migration-tooling
- **State:** Done (completed)
- **Priority:** Low
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:48:30.571+00:00
- **Completed:** 2026-04-28T13:47:45.560+00:00
- **Updated:** 2026-04-28T13:47:45.588+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Add dry-run migration tooling for moving global rooms, tasks, logs, members, pets, and subscriptions into household-scoped Firestore data.
* Add tenancy tests proving reads, writes, cache entries, and websocket/update paths cannot cross household boundaries.
* Document rollback and verification steps.

Acceptance criteria:

* Migration command supports dry-run output before writing.
* Tenancy tests cover two households with overlapping room/task IDs or member IDs.
* Rollback notes identify affected collections and safe restore path.

Validation:

* cargo fmt --check
* cargo test
* Migration dry-run proof using fixture data.

Blocker rules:

* Depends on CH-444 and CH-446.
* Do not run migration against production Firestore without explicit issue scope and required credentials.
* Do not print household data or secrets in logs.

## Attachments

- [CH-449: Add tenancy tests and migration dry run](https://github.com/ThorbenWoelk/putz-munter/pull/8) (github)

## Comments

### 2026-04-28T13:40:43.353+00:00 - thorben

## Codex Workpad

Plan:
- [x] Start from current `origin/main` on branch `thorben/ch-449-phase-2-add-tenancy-tests-and-migration-tooling`.
- [x] Inspect CH-444/CH-446 household model and repository boundaries.
- [x] Add fixture-backed dry-run migration tooling for global data to household-scoped Firestore paths.
- [x] Add tenancy tests for read/write/cache/websocket/update boundaries across two households with overlapping IDs.
- [x] Document rollback and verification steps.

Acceptance criteria:
- [x] Migration command supports dry-run output before writing.
- [x] Tenancy tests cover two households with overlapping room/task IDs or member IDs.
- [x] Rollback notes identify affected collections and safe restore path.

Validation:
- [x] `cargo fmt --check`
- [x] `cargo test`
- [x] Migration dry-run proof using fixture data: `cargo run --bin putz-munter-migrate -- --input fixtures/migration/global-to-household.json --household-id household-fixture --dry-run --output json`

Blockers:
- None. CH-444 and CH-446 are Done. Open PR overlap check found no open PRs. Active CH-448 is scheduler/notification endpoint work; CH-449 avoided those files.

Commit:
- `50b15f7` Add household tenancy migration dry run

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/8

### 2026-04-28T11:03:50.179+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.

### 2026-04-28T11:00:27.508+00:00 - thorben

Clean-start note: the legacy SurrealDB migration package has been removed. When this issue starts, create fresh fixture-backed migration tooling from the CH-444/CH-446 household-scoped model; do not resurrect old scripts or source credentials.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-449 - Phase 2 add tenancy tests and migration tooling.json`

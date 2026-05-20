---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- testing
- e2e
- unit-test
- quality
- putz-munter
- ci
- smoke-test
connections: []
ai_generated: true
human_approved: false
category:
- Linear Archive
- Issues
- Linear Archive/Putz & Munter
tagging_processed_count: 10
tagging_last_processed: 2026-05-15
---

# CH-460 - Quality: add unit and E2E scenario test foundation

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-460/quality-add-unit-and-e2e-scenario-test-foundation
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T11:16:58.719+00:00
- **Completed:** 2026-04-28T13:37:41.638+00:00
- **Updated:** 2026-04-28T13:37:41.650+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Establish the test foundation for product-quality autonomous development.
* Add or document backend unit tests, frontend/native UI unit or component tests, and native E2E scenario tests for critical app behavior.
* Choose the right E2E layer for the native app path once CH-457 selects it, such as Playwright only for web-shell checks, Detox/Appium/Maestro/native platform UI tests for mobile, or another maintained native scenario runner.
* Cover the core user scenarios as test specs or automated flows: unauthenticated access, login, household creation, room/task setup, completing a task, assignment/role behavior, household switching, logout, notifications where feasible, and protected API behavior.
* Make tests runnable locally and in CI where practical.

Acceptance criteria:

* Repo has a documented test strategy for backend unit tests, frontend/native unit or component tests, and native E2E scenario tests.
* At least one automated smoke scenario runs against the selected app surface with deterministic test data or mocks.
* Test commands exist for the stable automated layers.
* CI runs the stable automated test subset or documents why a test stays local/manual for now.
* Future feature issues can point to the test foundation and add scenario coverage incrementally.

Validation:

* cargo test
* frontend/native unit/typecheck command from the chosen setup
* E2E/native smoke command from a clean local build or documented blocker if app/auth/native wrapper foundation is not ready
* GitHub Actions validation if CI is changed

Blocker rules:

* Do not make E2E tests depend on production Firebase or real household data.
* Do not commit test credentials, device signing material, or screenshots with private data.
* If auth/onboarding or native wrapper is not ready yet, add scaffolding and mark concrete scenarios blocked on CH-445/CH-447/CH-457.

## Attachments

- [CH-460: add test foundation](https://github.com/ThorbenWoelk/putz-munter/pull/7) (github)

## Comments

### 2026-04-28T13:27:12.953+00:00 - thorben

## Codex Workpad

Plan:
- Done: inspected repo test/build structure from current origin/main.
- Done: added documented backend/frontend/E2E test strategy scoped to CH-460.
- Done: added deterministic frontend smoke scenario with mocked household data.
- Done: wired stable frontend test commands into local scripts and CI.

Acceptance criteria:
- Done: repo has documented test strategy in `docs/test-strategy.md` for backend unit tests, frontend unit/component direction, current web/API smoke, and future native E2E.
- Done: automated smoke scenario runs with deterministic mocked household data in `frontend/src/e2e/core-smoke.test.js`.
- Done: frontend test commands exist: `bun run test`, `bun run test:unit`, `bun run test:smoke`.
- Done: CI runs the stable automated frontend subset with `bun run test`.
- Done: future feature issues can point to `docs/test-strategy.md` and add scenario coverage incrementally.

Validation:
- Passed: `cargo fmt --check`
- Passed: `cargo clippy -- -D warnings`
- Passed: `cargo build`
- Passed: `cargo test`
- Passed: `bun run lint`
- Passed: `bun run typecheck`
- Passed: `bun run test`
- Passed: `NEXT_PUBLIC_FIREBASE_API_KEY=ci-firebase-placeholder NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=putz-munter-ci.firebaseapp.com NEXT_PUBLIC_FIREBASE_PROJECT_ID=putz-munter-ci bun run build`
- Passed: GitHub Actions on PR #7: Repo Hygiene, Backend Checks, Frontend Checks.

Blockers / coordination:
- CH-446 is In Progress with PR #6 touching only `backend/src/state/db.rs`; no file overlap with this PR.
- Native E2E remains blocked until CH-457 selects the native wrapper/installable app surface.
- Household creation/switching, onboarding, and role-aware UI scenarios remain blocked until CH-447 provides those app surfaces.
- No production Firebase, real household data, credentials, signing material, or private screenshots were used.

Commit / PR:
- Commit: `5774c48` (`Add CH-460 test foundation`)
- PR: https://github.com/ThorbenWoelk/putz-munter/pull/7

### 2026-04-28T13:26:10.161+00:00 - thorben

Queued as the parallel quality lane. Establish test strategy and stable automated foundation now. Keep native E2E runner selection conditional on CH-457; do not depend on production Firebase or private data. Avoid auth/Firestore implementation edits while CH-446 is active.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-460 - Quality add unit and E2E scenario test foundation.json`

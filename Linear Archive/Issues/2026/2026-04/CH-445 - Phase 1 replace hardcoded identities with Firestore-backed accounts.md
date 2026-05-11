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

# CH-445 - Phase 1: replace hardcoded identities with Firestore-backed accounts

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-445/phase-1-replace-hardcoded-identities-with-firestore-backed-accounts
- **State:** Done (completed)
- **Priority:** Low
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:31:57.349+00:00
- **Completed:** 2026-04-28T13:25:36.992+00:00
- **Updated:** 2026-04-28T13:25:37.014+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Replace hardcoded prototype identity selection with persisted users or invite-backed onboarding.
* Back auth users by the Firestore/Firebase-compatible account model from CH-444.
* Use dAstIll as implementation reference for Firebase Auth mechanics: client sign-in, emulator support, backend token verification, and server-side access context.
* Preserve a documented local development login path.
* Move browser auth toward secure cookie sessions unless a Linear issue explicitly chooses direct Firebase bearer-token calls.

Acceptance criteria:

* Login/onboarding is driven by persisted users or invites.
* Frontend auth UI no longer exposes a fixed identity picker.
* Session verification does not depend on hardcoded personal names or emails.
* Backend resolves user, active household, membership, and role before returning household data.
* Local development setup remains documented.

Validation:

* cargo fmt --check
* cargo test
* frontend lint or typecheck for touched auth UI
* Manual login/onboarding notes for local development.

Blocker rules:

* Depends on CH-444 data model decisions.
* Do not use client-only Firebase as the app authority.
* Do not require production Firebase credentials for local tests.
* If copying dAstIll behavior conflicts with household privacy, preserve household privacy and document the difference.

## Attachments

- [[codex\] Replace prototype identity picker](https://github.com/ThorbenWoelk/putz-munter/pull/4) (github)

## Comments

### 2026-04-28T13:19:55.857+00:00 - thorben

Coordinator security fix: pending household invites now require a verified Firebase email before auto-claiming membership. Local backend validation passed: `cargo fmt --check`, `cargo clippy -- -D warnings`, and `cargo test`.

### 2026-04-28T13:15:06.780+00:00 - thorben

Coordinator update: merged current main into PR #4 branch after CH-451/CH-452 landed. Push d1cfe27 includes the release docs baseline plus the CI placeholder fix. Waiting for the new GitHub checks.

### 2026-04-28T13:14:17.562+00:00 - thorben

Coordinator update: PR #4 frontend CI failed because Next production build had no public Firebase env values. Added CI-only placeholder NEXT_PUBLIC_FIREBASE_* values to the frontend build step in commit 79b669c. Local `bun run build` passed with those placeholders. Waiting for GitHub checks again.

### 2026-04-28T12:54:29.287+00:00 - thorben

## Codex Workpad

Plan:
- Done: Confirmed CH-445 was Todo and moved it to In Progress before implementation.
- Done: Confirmed CH-444 is Done and used its household auth model decisions.
- Done: Replaced the frontend fixed identity picker with Firebase Auth sign-in and backend session exchange.
- Done: Added backend Firebase ID-token verification, Firestore user/invite/membership resolution, and backend-owned cookie session payloads.
- Done: Scoped product Firestore reads, writes, cache entries, websocket updates, and notification checks to the active household.
- Done: Persisted household invites and documented local Firebase Auth emulator bootstrap.
- Done: Ran required validation.
- Done: Created branch, committed, pushed, and opened draft PR.

Acceptance criteria:
- Done: Login/onboarding is driven by Firebase-authenticated persisted users, pending invites, or documented local bootstrap.
- Done: Frontend auth UI no longer exposes a fixed identity picker.
- Done: Session verification no longer depends on hardcoded personal names or emails.
- Done: Backend resolves user, active household, membership, and role before returning household data.
- Done: Local development setup remains documented in `docs/setup.md` and `backend/.env.example`.

Validation:
- Passed: `cargo fmt --check`.
- Passed: `cargo test` (34 backend tests).
- Passed: `bun run typecheck`.
- Passed: `bun run lint`.
- Passed: `git diff --check`.
- Manual login/onboarding notes: local development uses Firebase Auth emulator settings in untracked env files. With `AUTH_LOCAL_BOOTSTRAP=true`, the first Firebase-authenticated local user is persisted as a backend user plus owner membership for the configured local household. Production users require Firebase Auth plus Firestore memberships or pending household invites.

Blockers:
- None.

Commit:
- `ed317af` Replace prototype identity picker.

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/4

### 2026-04-28T11:03:50.774+00:00 - thorben

Auth decision note: replace prototype identities with Firebase-backed users and backend-owned household memberships. Do not introduce anonymous household data access.

### 2026-04-28T11:03:48.683+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-445 - Phase 1 replace hardcoded identities with Firestore-backed accounts.json`

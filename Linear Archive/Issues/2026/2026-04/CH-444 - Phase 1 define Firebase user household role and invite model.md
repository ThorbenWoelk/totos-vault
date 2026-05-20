---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- done-issue
- firebase
- auth
- firestore
- household
- roles
- backend
- invites
- audit
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

# CH-444 - Phase 1: define Firebase user, household, role, and invite model

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-444/phase-1-define-firebase-user-household-role-and-invite-model
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:31:57.127+00:00
- **Completed:** 2026-04-28T12:53:20.706+00:00
- **Updated:** 2026-04-28T12:53:20.714+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Define backend model/types and repo documentation for users, households, memberships, invites, roles, and audit_events.
* Use dAstIll as the Firebase Auth reference: Firebase client login, emulator support, backend ID-token verification, access-context resolution, and server-side role mapping.
* Adapt the dAstIll pattern to household-scoped memberships and roles: owner, member, viewer.
* Document Firestore paths and migration strategy from global prototype collections to household-scoped data.
* Keep runtime behavior minimal unless a compile/test boundary needs model code.

Acceptance criteria:

* Repo docs describe collections, required fields, indexes or query patterns, and ownership rules.
* Backend types represent users, households, memberships, invites, roles, and audit events.
* Chore domain ownership is specified as household-scoped.
* Audit events include actor, household, action, target, timestamp, and metadata shape.
* Auth model doc explains how Firebase identity maps to backend user, membership, active household, and role.

Validation:

* cargo fmt --check
* cargo test or cargo check for touched backend code
* Documentation review against current route/state/store code and the dAstIll auth reference.

Blocker rules:

* Do not migrate production data in this issue.
* Do not introduce client-only Firebase access.
* Direct Firebase bearer-token browser transport vs backend cookie session is a decision point; document the tradeoff and keep cookie sessions as the default unless explicitly changed.

## Attachments

- [CH-444 Define Firebase household auth model](https://github.com/ThorbenWoelk/putz-munter/pull/2) (github)

## Comments

### 2026-04-28T11:31:45.940+00:00 - thorben

## Codex Workpad

Plan:
- Done: Confirmed CH-444 is in the Putz & Munter project and moved it from Todo to In Progress before implementation.
- Done: Confirmed current backend/frontend auth, route, state, websocket, and store shape.
- Done: Added backend model/types for Firebase identity, backend users, households, memberships, invites, roles, access context, and audit events with minimal runtime impact.
- Done: Updated repo documentation for Firestore collections, ownership rules, auth mapping, Firebase Auth emulator awareness, migration strategy, and cookie-session default.
- Done: Ran required validation.
- Done: Created branch, committed, pushed, and opened draft PR.

Acceptance criteria:
- Done: Repo docs describe collections, required fields, query/index patterns, and ownership rules in `docs/firestore-model.md`.
- Done: Backend types represent users, households, memberships, invites, roles, and audit events in `backend/src/auth_model.rs`.
- Done: Chore domain ownership is specified as household-scoped under `households/{householdId}/rooms/{roomId}` and related paths.
- Done: Audit events include actor, household, action, target, timestamp, and metadata shape.
- Done: Auth docs explain Firebase identity to backend user, active household, membership, and role mapping.

Validation:
- Passed: `cargo fmt --check` in `backend/`.
- Passed: `cargo test` in `backend/` (30 tests).
- Passed: Documentation review against current `backend/src/routes/auth.rs`, `backend/src/routes/ws.rs`, `backend/src/state/db.rs`, `backend/src/state/store.rs`, frontend-facing setup docs, and the dAstIll-pattern auth reference from the issue: Firebase client login, emulator support, backend ID-token verification, server-side access-context and role resolution, cookie-session default.
- Passed: `git diff --check`.

Blockers:
- None.

Commit:
- `e5f3738` Define household auth model.

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/2

### 2026-04-28T11:03:50.490+00:00 - thorben

Auth decision note: first target is Firebase Auth login, backend Firebase identity verification following dAstIll, backend-minted secure cookie session, and backend household role mapping before any household data access. Cookie sessions remain the default browser transport.

### 2026-04-28T11:03:48.328+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.

### 2026-04-28T10:54:43.984+00:00 - thorben

Production home update: model docs and Firebase Auth configuration should target Firebase/GCP project putz-munter-prod unless a later Linear issue explicitly changes the production project.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-444 - Phase 1 define Firebase user household role and invite model.json`

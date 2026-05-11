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

# CH-466 - Backend: add household onboarding, switching, and invite role endpoints

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-466/backend-add-household-onboarding-switching-and-invite-role-endpoints
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T13:56:58.847+00:00
- **Completed:** 2026-04-28T14:11:37.388+00:00
- **Updated:** 2026-04-28T14:11:37.407+00:00
- **Labels:** -

## Description

Created from CH-447.

Scope:

* Add backend-owned endpoints for `GET /api/households`, `POST /api/households`, and `POST /api/auth/switch-household`.
* Create households, memberships, starter rooms, and starter tasks server-side in Firestore.
* Return a refreshed backend session token after household creation or switch.
* Extend household invite handling to accept and persist selected roles, including viewer.
* Keep Firebase ID-token verification and server-side household access resolution as the authority.

Acceptance criteria:

* No client-only Firestore writes.
* A Firebase-authenticated user without an existing household can create one through the backend.
* An authenticated user with multiple memberships can switch active household through the backend.
* Invite role selection persists as owner/member/viewer according to backend authorization rules.
* Tests cover no cross-household membership access and viewer role behavior.

## Attachments

- [CH-466: add backend household onboarding endpoints](https://github.com/ThorbenWoelk/putz-munter/pull/13) (github)

## Comments

### 2026-04-28T14:01:15.264+00:00 - thorben

## Codex Workpad

Plan:
- Inspect active project issues and open PRs for overlapping ownership. Done: CH-447 owns frontend onboarding/API client work; CH-462 owned deployment docs and landed during this work. This change stayed backend-only.
- Start an issue branch from current origin/main. Done: `thorben/ch-466-backend-add-household-onboarding-switching-and-invite-role`.
- Implement backend-owned household onboarding, switching, and invite role persistence in Firestore. Done.
- Add focused tests for cross-household membership access and viewer role behavior. Done.
- Run validation, then commit, push, open PR. Done.

Acceptance criteria:
- No client-only Firestore writes. Backend endpoints now own household creation, membership creation, starter rooms/tasks, household switching, and invite role persistence.
- Firebase-authenticated user without a household can create one through `POST /api/households` using a Firebase ID token.
- Authenticated user with multiple memberships can switch active household through `POST /api/auth/switch-household`.
- Invite role selection persists as owner/member/viewer via `POST /api/household/invite`.
- Tests cover requested household rejection without membership and viewer mutation denial.

Validation:
- Rebased on latest `origin/main` after CH-462 landed.
- `cd backend && cargo fmt --check` passed.
- `cd backend && cargo clippy -- -D warnings` passed.
- `cd backend && cargo build` passed.
- `cd backend && cargo test` passed: 54 tests.
- `git diff --check` passed.

Blockers:
- None.

Commit:
- `12ce42a` CH-466 add backend household onboarding endpoints.

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/13


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-466 - Backend add household onboarding switching and invite role endpoints.json`

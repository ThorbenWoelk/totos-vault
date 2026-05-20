---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- onboarding
- household-switcher
- role-aware-ui
- frontend
- e2e
- authentication
- putz-munter
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

# CH-447 - Phase 3: add onboarding, household switcher, and role-aware UI

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-447/phase-3-add-onboarding-household-switcher-and-role-aware-ui
- **State:** Done (completed)
- **Priority:** Low
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:31:57.788+00:00
- **Completed:** 2026-04-28T14:17:42.484+00:00
- **Updated:** 2026-04-28T14:17:42.500+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Add generic onboarding screens for creating a household, inviting members, choosing roles, and configuring starter rooms/tasks.
* Add household switcher for accounts with multiple memberships.
* Hide or disable mutation controls for viewer role.
* Replace identity-specific copy with concise German product copy.
* Add or extend E2E scenario coverage for the main user journeys touched by this issue.

Acceptance criteria:

* Unauthenticated users see onboarding/login and cannot view household data pages.
* A user can create or select a household from the UI.
* A user can invite members and choose roles.
* Viewer role cannot mutate tasks from the UI.
* E2E or scenario tests cover login/onboarding, household creation or selection, role-aware viewer state, and logout/login behavior where backend support exists.

Validation:

* frontend lint or typecheck
* E2E scenario command for onboarding and household switching, or documented blocker tied to missing backend endpoints
* Manual browser notes for onboarding, household switch, role-aware viewer state, and logout/login

Blocker rules:

* Depends on CH-444, CH-445, and CH-446.
* Coordinate with the quality test foundation issue if E2E tooling is not in place.
* Do not invent a client-only Firebase authority.
* If backend endpoints are missing, add UI behind typed API boundaries and file backend follow-up issues.

## Attachments

- [CH-447: add onboarding and role-aware UI](https://github.com/ThorbenWoelk/putz-munter/pull/11) (github)

## Comments

### 2026-04-28T14:14:58.029+00:00 - thorben

Coordinator update after CH-466 landed:

- Rebased the frontend PR on main after backend household endpoints merged.
- Added a first-household flow for Firebase-authenticated users without an existing backend session. The frontend now keeps the Firebase ID token in memory after session exchange fails and passes it to POST /api/households.
- Validation passed locally after the patch: npm run typecheck, npm run lint, bun run test.

Commit added to PR #11: ff5d7a8 fix: wire first-household onboarding token.

### 2026-04-28T13:50:15.773+00:00 - thorben

## Codex Workpad

Plan:
- Completed: started from current `origin/main` on branch `thorben/ch-447-phase-3-add-onboarding-household-switcher-and-role-aware-ui`.
- Completed: inspected existing Firebase session, household, invite, role, and E2E smoke surfaces.
- Completed: implemented frontend auth gates, household onboarding/switcher API-boundary UI, invite role selection, and viewer-safe task UI.
- Completed: extended smoke scenario coverage for login/onboarding API boundaries, household creation/switching calls, invite role payload, viewer role helpers, protected reads, and logout.

Acceptance criteria:
- Unauthenticated users see login/onboarding surfaces before household data hooks mount.
- Household creation and switching UI exists behind typed backend API boundaries.
- Member invite UI includes role choice and sends the role payload.
- Viewer role cannot mutate tasks from task cards, FAB, task creation, or task edit UI.
- E2E smoke scenario covers login/onboarding boundaries, household creation/switching requests, viewer role behavior, protected read rejection, and logout/login API flow.

Validation:
- Passed: `npm run typecheck`.
- Passed: `npm run lint`.
- Passed: `bun run test`.
- Passed: `bun run test:smoke`.
- Manual browser: blocked by missing local backend runtime config. `./start_app.sh` compiled the backend, then stopped with `PROJECT_ID or FIRESTORE_PROJECT_ID must be set` before frontend startup. `./end_app.sh` was run afterward and ports were clear.

Blockers:
- Backend endpoints for `GET /api/households`, `POST /api/households`, and `POST /api/auth/switch-household` are missing on this base.
- Invite role persistence needs backend support; frontend sends the typed role payload.
- Filed follow-up `CH-466` in Backlog for backend household onboarding, switching, starter setup, and invite role persistence.

Commit:
- `e872bb2` CH-447 add onboarding and role-aware UI.

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/11

### 2026-04-28T11:03:49.313+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-447 - Phase 3 add onboarding household switcher and role-aware UI.json`

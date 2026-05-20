---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- linear
- done-issue
- authentication
- backend
- security
- middleware
- rust
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

# CH-443 - Phase 0: require auth for chores and notification APIs

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-443/phase-0-require-auth-for-chores-and-notification-apis
- **State:** Done (completed)
- **Priority:** High
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:31:56.891+00:00
- **Completed:** 2026-04-28T12:38:45.352+00:00
- **Updated:** 2026-04-28T12:38:45.374+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Add backend auth/session middleware or route guards for all chores and notification read/write endpoints.
* Keep /health and explicitly public app config available without auth.
* Return consistent 401 responses for unauthenticated API access.
* Keep secure cookie sessions as the browser path; do not expand localStorage bearer-token behavior.
* Leave Firebase Auth user-model work to CH-444 and CH-445.

Acceptance criteria:

* /api/chores and notification endpoints reject unauthenticated requests.
* Auth/session endpoints and health checks keep their intended public behavior.
* Frontend can open the login path without leaking household data.
* Backend tests cover at least one protected read path and one protected mutation path.

Validation:

* cargo fmt --check
* cargo test
* Local smoke check: unauthenticated /api/chores returns 401 and /health returns success.

Blocker rules:

* Do not require Firebase Auth credentials in this issue.
* If route ownership is unclear, protect the narrower route group and file a Backlog follow-up for remaining endpoints.
* Do not weaken notification or scheduler protection to make tests pass.

## Attachments

- [CH-443: Require auth for chores and notifications](https://github.com/ThorbenWoelk/putz-munter/pull/1) (github)

## Comments

### 2026-04-28T11:26:47.266+00:00 - thorben

## Codex Workpad

Plan:
- Done: moved CH-443 from Todo to In Progress before implementation.
- Done: inspected backend route layout and existing auth/session helpers.
- Done: added route-level auth/session protection to chores and notification API routes while keeping /health, /api/config, and auth/session routes public.
- Done: updated the local CLI to send its existing auth token for protected chores reads and notification commands.
- Done: fixed start_app.sh to run the explicit backend binary now that the backend crate has multiple binaries.
- Done: added backend tests for one protected read path and one protected mutation path.
- Done: created branch, committed, pushed, and opened a draft PR.

Acceptance criteria:
- Done: /api/chores and notification endpoints reject unauthenticated requests through the protected Axum route layer.
- Done: auth/session endpoints, /api/config, and /health remain outside the protected route group.
- Done by route shape and smoke proof: frontend login path can open without protected API data being exposed before auth.
- Done: backend tests cover protected read and mutation paths returning 401.

Validation:
- Passed: cargo fmt --check
- Passed: cargo test
- Passed: local smoke with placeholder local env: PROJECT_ID=local-smoke AUTH_SESSION_SECRET=local-smoke-secret SMTP_PASSWORD=local-smoke SMTP_USER=local-smoke SMTP_FROM=local-smoke@example.com ./start_app.sh
- Passed smoke probes: GET /health -> 200; unauthenticated GET /api/chores -> 401.
- Note: the smoke run logged expected Firestore PermissionDenied messages from the background listener for the placeholder local-smoke project after the endpoint checks. No real secrets were used.

Blockers:
- None.

Commit:
- ca70f97 Protect chore and notification APIs

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/1

### 2026-04-28T11:03:48.009+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-443 - Phase 0 require auth for chores and notification APIs.json`

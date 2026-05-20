---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- security
- scheduler
- notifications
- authentication
- backend
- rust
- oidc
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

# CH-448 - Phase 1: protect scheduler and notification service endpoints

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-448/phase-1-protect-scheduler-and-notification-service-endpoints
- **State:** Done (completed)
- **Priority:** Low
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:48:29.488+00:00
- **Completed:** 2026-04-28T13:51:07.699+00:00
- **Updated:** 2026-04-28T13:51:07.708+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Protect notification and scheduler endpoints with Cloud Scheduler OIDC, service account identity, or equivalent backend-only authorization.
* Keep public endpoints limited to health and explicitly public config.
* Document local development bypass or test identity without production secrets.
* Keep deployment automation out of scope unless a separate release issue has selected and implemented a fresh deployment path.

Acceptance criteria:

* Scheduler-triggered jobs reject unauthenticated public requests.
* Deployment or operations docs describe the required service identity for the selected release path.
* Tests or smoke checks prove public rejection and authorized service path behavior.

Validation:

* cargo fmt --check
* cargo test
* Deployment config validation only if deployment files are intentionally added by a scoped release issue.
* Local or documented production smoke proof for the selected release path.

Blocker rules:

* Depends on CH-443 route protection shape.
* Do not expose scheduler secrets or make shared static tokens product defaults.
* If production credentials or deployment path are missing, validate locally and document the production smoke check as blocked.

## Attachments

- [CH-448: protect scheduler notification endpoint](https://github.com/ThorbenWoelk/putz-munter/pull/9) (github)

## Comments

### 2026-04-28T13:48:58.655+00:00 - thorben

Coordinator note: recovered the Symphony branch after it stopped in Blocked with a local commit. Rebasing onto main after CH-449 caused one conflict in backend/src/main.rs. Resolved by keeping CH-449's backend library layout and adding service_auth to backend/src/lib.rs.

Validation after rebase:
- cargo fmt --check
- cargo test
- cargo clippy -- -D warnings

PR: https://github.com/ThorbenWoelk/putz-munter/pull/9

### 2026-04-28T13:40:46.056+00:00 - thorben

## Codex Workpad

Plan:
- Done: confirmed CH-448 was Todo and moved it to In Progress.
- Done: checked active Putz & Munter issues and open PRs for overlapping ownership.
- In progress: inspect backend scheduler, notification, auth/session, and docs surfaces from origin/main.
- Pending: implement backend-only scheduler/service authorization with local test path.
- Pending: document required service identity and local bypass/test identity without secrets.
- Pending: run required validation and create PR.

Acceptance criteria:
- Pending: scheduler-triggered jobs reject unauthenticated public requests.
- Pending: deployment or operations docs describe the required service identity for the selected release path.
- Pending: tests or smoke checks prove public rejection and authorized service path behavior.

Validation:
- Pending: cargo fmt --check
- Pending: cargo test
- Pending: local or documented production smoke proof for the selected release path.
- Not applicable unless scoped release files are added: deployment config validation.

Blockers:
- None currently. Production smoke may be blocked if credentials or deployment path are unavailable.

Commit:
- Pending.

PR:
- Pending.

### 2026-04-28T11:03:49.643+00:00 - thorben

Production safety checklist for this issue:

- No unauthenticated household reads.
- No client-only Firestore writes.
- No cross-household reads, writes, cache entries, websocket updates, or notifications.
- No migration against production without explicit Linear scope and dry-run evidence.
- No secrets in comments, commits, logs, PRs, screenshots, or docs.
- No production deployment automation unless the Linear issue explicitly scopes fresh setup for putz-munter-prod.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-448 - Phase 1 protect scheduler and notification service endpoints.json`

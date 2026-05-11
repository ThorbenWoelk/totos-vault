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

# CH-456 - Release: add observability, backups, audit, and operations runbooks

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-456/release-add-observability-backups-audit-and-operations-runbooks
- **State:** Done (completed)
- **Priority:** Low
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T11:15:28.257+00:00
- **Completed:** 2026-04-28T15:00:23.493+00:00
- **Updated:** 2026-04-28T15:00:23.510+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Add production observability and operations documentation for backend, frontend, Firestore, auth, notifications, and scheduler jobs.
* Configure or document logs, metrics, alerts, audit-event inspection, Firestore backup/export strategy, and incident response.
* Keep cost controls visible for low-traffic household usage.

Acceptance criteria:

* Runbook covers deploy verification, rollback, common failures, secret rotation, and support triage.
* Logs and metrics identify auth failures, tenant-boundary errors, notification failures, and backend 5xxs.
* Firestore backup/export or recovery strategy is documented.
* Budget/alert state for putz-munter-prod is documented.

Validation:

* Documentation review against production deployment path.
* If monitoring config is added, validate with provider CLI or console evidence without secrets.

Blocker rules:

* Depends on selected deployment path.
* Do not export or print household data while proving observability.

## Attachments

- [CH-456: Add production operations runbook](https://github.com/ThorbenWoelk/putz-munter/pull/18) (github)

## Comments

### 2026-04-28T14:54:27.025+00:00 - thorben

## Codex Workpad

Plan:
- Completed: inspected production deployment workflow, backend/frontend config, Firestore rules/indexes, auth/service-auth/notification code, and existing production docs.
- Completed: added a dedicated production operations runbook and linked it from deployment/setup docs.
- Completed: filed Backlog follow-up CH-468 for backend audit-event writes because the repo has audit models/indexes but no write paths.
- Completed: committed, pushed, and opened PR.

Acceptance criteria:
- Runbook covers deploy verification, rollback, common failures, secret rotation, and support triage.
- Logs and metrics identify auth failures, tenant-boundary errors, notification failures, and backend 5xxs.
- Firestore backup/export and recovery strategy is documented.
- Budget/alert state for putz-munter-prod is documented.

Validation:
- Passed: documentation review against production deployment path using `.github/workflows/deploy-production.yml`, `docs/production-deployment.md`, `docs/production-deployment-plan.md`, `docs/firestore-model.md`, `firestore.rules`, `firestore.indexes.json`, `backend/src/main.rs`, `backend/src/service_auth.rs`, `backend/src/notifications.rs`, and `frontend/apphosting.yaml`.
- Passed: local linked evidence paths exist.
- Passed: `git diff --check`.
- Monitoring config added: none. Provider CLI validation not required.

Blockers:
- None for CH-456.
- Follow-up: CH-468 covers missing backend audit-event writes outside this docs ticket.

Commit:
- `9cf188d` docs: add production operations runbook

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/18

### 2026-04-28T14:53:28.288+00:00 - thorben

Coordination handoff for Symphony:

CH-453 production deployment pipeline, CH-454 Firebase rules/indexes, CH-448 service endpoint protection, and CH-465 Android TWA setup are Done. Add operations/runbook material for Cloud Run backend, Firebase App Hosting frontend, Firestore, Firebase Auth, scheduler/service-only endpoints, notifications, audit events, backups/exports, alerts, incident response, rollback, support triage, and cost controls.

Production deploy currently has ENABLE_PRODUCTION_DEPLOY gating automatic runs. Document this as an operator control. A prior production workflow run failed before the gate because App Hosting/SMTP variables were absent; later gated runs skipped as intended. Do not export/print household data and do not paste secrets. Active parallel issue CH-464 owns store readiness docs; avoid touching store listing/compliance docs unless directly relevant.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-456 - Release add observability backups audit and operations runbooks.json`

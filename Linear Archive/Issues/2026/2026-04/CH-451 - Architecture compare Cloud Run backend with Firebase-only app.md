---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- architecture
- cloud-run
- firebase
- rust
- cost-analysis
- security
- decision
- firestore
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

# CH-451 - Architecture: compare Cloud Run backend with Firebase-only app

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-451/architecture-compare-cloud-run-backend-with-firebase-only-app
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:52:41.716+00:00
- **Completed:** 2026-04-28T13:03:58.650+00:00
- **Updated:** 2026-04-28T13:03:58.661+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Compare long-run cost and operational tradeoffs for Cloud Run backend plus Firestore versus a Firebase-only app.
* Include auth, authorization, household tenancy, notifications, scheduler/service jobs, migrations, audit logging, local development, and CI/CD.
* Decide whether Firebase-only is viable for Putz & Munter or whether the Rust backend remains the cost/security sweet spot.

Acceptance criteria:

* Cost model includes expected low-traffic household usage and growth scenario.
* Security model covers household data isolation and role enforcement.
* Service-job alternatives are documented for notifications, scheduling, migrations, and audit logging.
* Recommendation is explicit: keep backend, move parts to Firebase, or create a staged migration issue.

Validation:

* Architecture note committed to repo docs.
* Decision references current code paths and Firebase/GCP services.

Blocker rules:

* Do not migrate implementation in this issue.
* Do not choose client-only Firebase without Firestore Security Rules, service-job alternative, and rollback plan.
* If pricing inputs are missing, document assumptions and keep the issue in Backlog.

## Attachments

- [CH-451: Compare backend and Firebase-only architecture](https://github.com/ThorbenWoelk/putz-munter/pull/3) (github)

## Comments

### 2026-04-28T12:57:24.969+00:00 - thorben

## Codex Workpad

Plan:
- Done: started from current `origin/main` on `thorben/ch-451-architecture-compare-cloud-run-backend-with-firebase-only`.
- Done: inspected current Rust backend, Next.js frontend, Firestore/Firebase usage, service-job paths, auth/session paths, CI, and docs.
- Done: checked current official Google/Firebase pricing sources for Cloud Run, Firestore, Firebase Auth / Identity Platform, Cloud Scheduler, Secret Manager, Firebase Hosting, and Cloud Run functions.
- Done: added `docs/cloud-run-vs-firebase-only.md` comparing Cloud Run backend plus Firestore with Firebase-only for `putz-munter-prod`.
- Done: created Backlog follow-up `CH-462` for fresh production deployment design.
- Done: committed, pushed, and opened PR.

Acceptance criteria:
- Done: cost model includes expected low-traffic household usage and growth scenario.
- Done: security model covers household data isolation and role enforcement.
- Done: service-job alternatives documented for notifications, scheduling, migrations, and audit logging.
- Done: recommendation is explicit: keep the Rust Axum backend plus Firestore; move Firebase Auth/client identity and household-scoped Firestore model forward; do not choose client-only Firebase from this issue.

Validation:
- Passed: architecture note committed to repo docs in `docs/cloud-run-vs-firebase-only.md`.
- Passed: decision references current code paths, including `backend/src/main.rs`, `backend/src/auth.rs`, `backend/src/routes/auth.rs`, `backend/src/routes/chores.rs`, `backend/src/routes/ws.rs`, `backend/src/state/db.rs`, `backend/src/auth_model.rs`, `backend/src/notifications.rs`, `frontend/src/lib/api.ts`, `frontend/src/components/auth/AuthProvider.tsx`, and `.github/workflows/ci.yml`.
- Passed: decision references Firebase/GCP services: Firebase Auth / Identity Platform, Cloud Run, Firestore, Secret Manager, Cloud Scheduler, Firebase Hosting, and Cloud Run functions.
- Passed: `git diff --check`.
- Passed: `test -f docs/cloud-run-vs-firebase-only.md && rg -n "Cost model|Security model|Service-Job Alternatives|Recommendation|CH-462|backend/src/main.rs|backend/src/auth.rs|backend/src/state/db.rs|backend/src/notifications.rs|frontend/src/lib/api.ts|Firebase Auth|Cloud Run|Firestore|Secret Manager|Cloud Scheduler|Cloud Run functions" docs/cloud-run-vs-firebase-only.md`.

Blockers:
- None.
- Active Linear work `CH-445` and `CH-446` touches auth/Firestore implementation; this issue stayed docs-only and did not edit shared implementation files.

Commit:
- `37a8746` docs: compare backend and firebase-only architecture

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/3

Notes:
- Open PRs were clear before implementation and before PR creation.
- No Terraform, Cloud Run service, Docker image, scheduler, workload identity, or deployment automation was added.

### 2026-04-28T12:56:40.271+00:00 - thorben

Queued as the release architecture lane for Symphony. Keep this docs-only: compare Cloud Run backend, Firebase-only, and staged options for putz-munter-prod; create or link implementation follow-ups; do not add deployment code in this issue.

### 2026-04-28T10:58:47.685+00:00 - thorben

Clean-start note: old toto-chores deployment setup has been removed from the repo. Cost comparison should assume fresh deployment design for putz-munter-prod, with no inherited Terraform, Docker image repository, Cloud Run service, scheduler, or WIF binding.

### 2026-04-28T10:54:44.009+00:00 - thorben

Scope note: compare Cloud Run backend vs Firebase-only using putz-munter-prod as the target Firebase/GCP project. Include expected low-traffic cost assumptions for that production home.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-451 - Architecture compare Cloud Run backend with Firebase-only app.json`

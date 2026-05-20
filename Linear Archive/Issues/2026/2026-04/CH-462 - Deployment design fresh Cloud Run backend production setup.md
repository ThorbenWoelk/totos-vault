---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- deployment
- cloud-run
- gcp
- backend
- infrastructure
- rust
- axum
- firebase
- production
- design
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

# CH-462 - Deployment: design fresh Cloud Run backend production setup

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-462/deployment-design-fresh-cloud-run-backend-production-setup
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T13:00:41.452+00:00
- **Completed:** 2026-04-28T14:04:13.177+00:00
- **Updated:** 2026-04-28T14:04:13.196+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Follow-up from CH-451.

Scope:

* Design fresh production deployment for `putz-munter-prod` after auth and household tenancy are accepted.
* Cover Firebase Auth, frontend hosting, Rust Axum backend on Cloud Run, Firestore, Secret Manager, service-only Scheduler or Cloud Run Job path, IAM/service identity, CI/CD, budget alerts, logging, and rollback.
* Keep the design fresh. Do not reuse old toto-chores Terraform state, Cloud Run services, Docker image repositories, scheduler jobs, or workload identity bindings.

Acceptance criteria:

* Architecture/deployment plan identifies runtime services, secrets, service identities, and least-privilege access.
* Cost assumptions use current Google/Firebase pricing for expected household usage and a growth scenario.
* CI/CD path is documented without adding production deployment automation unless this issue explicitly scopes implementation.
* Rollback and production safety checklist are documented.

Validation:

* Docs reference current code paths and Firebase/GCP services.
* No deployment resources are created unless implementation is explicitly added to this issue.

Blockers:

* Wait for CH-445 and CH-446 or equivalent auth/tenancy decisions before implementation.

## Attachments

- [CH-462: Detail fresh production deployment plan](https://github.com/ThorbenWoelk/putz-munter/pull/12) (github)

## Comments

### 2026-04-28T13:57:10.053+00:00 - thorben

## Codex Workpad

Plan:
- Done: Confirmed CH-462 was Todo and moved it to In Progress before implementation.
- Done: Confirmed CH-445 and CH-446 are Done, clearing the issue blocker.
- Done: Checked active project issue and open PR overlap. CH-447 is active in the UI/onboarding lane; CH-462 stayed on deployment design docs.
- Done: Inspected current backend, frontend, Firestore, auth, service-auth, notification, migration, and CI paths.
- Done: Documented fresh production deployment design for `putz-munter-prod` without creating deployment resources.
- Done: Validated docs against current code paths and issue acceptance criteria.
- Done: Committed, pushed, and opened PR #12.

Acceptance criteria:
- Met: Architecture/deployment plan identifies runtime services, secrets, service identities, and least-privilege access.
- Met: Cost assumptions use current Google/Firebase pricing for expected household usage and a 500-household growth scenario.
- Met: CI/CD path is documented without adding production deployment automation.
- Met: Rollback and production safety checklist are documented.

Validation:
- Passed: `git diff --check`.
- Passed: docs-only changed-file check: only `docs/production-deployment-plan.md` and `docs/cloud-run-vs-firebase-only.md` changed.
- Passed: stale deployment/auth wording check for `AUTH_ALLOWED_USERS`, old flat collection wording, old Firebase Hosting pricing wording, and old blocker wording.
- Passed: current referenced code-path existence check for backend auth, service auth, Firestore, notifications, migration, frontend Firebase/proxy/WebSocket, and CI files.
- Passed: tracked deployment-artifact absence check for Terraform, Dockerfile, deploy YAML, tfstate, and tfplan paths.
- Confirmed: No deployment resources were created.

Blockers:
- None.

Commit:
- `2739a1e` docs: detail fresh production deployment plan

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/12


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-462 - Deployment design fresh Cloud Run backend production setup.json`

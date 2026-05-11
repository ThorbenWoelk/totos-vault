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

# CH-452 - Release: choose production architecture and deployment plan

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-452/release-choose-production-architecture-and-deployment-plan
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T11:15:23.363+00:00
- **Completed:** 2026-04-28T13:14:37.705+00:00
- **Updated:** 2026-04-28T13:14:37.714+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Turn CH-451's cost/security recommendation into a concrete production deployment plan for putz-munter-prod.
* Decide the first release path: Cloud Run backend + hosted Next.js frontend, Firebase-only, Firebase App Hosting, or another documented GCP/Firebase path.
* Include domain, HTTPS, cookie/session behavior, CI/CD trigger model, rollback approach, and expected monthly cost.
* Keep old toto-chores deployment setup deleted. Design the release path fresh.

Acceptance criteria:

* Repo doc describes the selected production topology and why it fits Putz & Munter's cost, privacy, and tenancy needs.
* Deployment plan names required GCP/Firebase services, service accounts, secrets, domains, and CI/CD responsibilities.
* Rollback and failure modes are documented.
* Follow-up implementation issues are created or linked for the selected path.

Validation:

* Documentation review against CH-451, docs/product-direction.md, and current repo structure.
* No production deployment code is added unless explicitly scoped in this issue.

Blocker rules:

* Depends on CH-451 if the architecture recommendation is still open.
* Do not reintroduce Terraform, Dockerfiles, Cloud Run, App Hosting, or Firebase-only implementation unless this issue explicitly chooses and scopes that path.
* Do not paste secrets into Git, Linear, logs, or PRs.

## Attachments

- [CH-452: choose production deployment path](https://github.com/ThorbenWoelk/putz-munter/pull/5) (github)

## Comments

### 2026-04-28T13:07:35.573+00:00 - thorben

## Codex Workpad

Plan:
- Done: moved issue from Todo to In Progress.
- Done: inspected CH-451 output, docs/product-direction.md, docs/auth-decision.md, docs/firestore-model.md, docs/production-safety.md, docs/setup.md, CI, and current backend/frontend structure.
- Done: added docs-only production topology and deployment plan for putz-munter-prod.
- Done: linked follow-up implementation issues CH-453, CH-454, CH-455, CH-456, CH-459, and CH-462 as related issues.
- Done: ran issue validation.
- Done: committed, pushed, and opened PR.
- Done: moved issue to In Review.

Acceptance criteria:
- Done: repo doc describes selected production topology: Firebase App Hosting for Next.js frontend, Cloud Run for Rust Axum backend, Firestore, Firebase Auth, Secret Manager, Scheduler/Jobs.
- Done: doc explains fit for cost, privacy, and tenancy.
- Done: deployment plan names required GCP/Firebase services, service accounts, secrets, domains, HTTPS, cookie/session behavior, and CI/CD responsibilities.
- Done: rollback and failure modes are documented.
- Done: follow-up implementation issues are linked.

Validation:
- Passed: documentation review against CH-451, docs/product-direction.md, and current repo structure.
- Passed: `git diff --check`.
- Passed: `test -f docs/production-deployment-plan.md && rg -n "Selected release path|Firebase App Hosting|Cloud Run|Firestore|Firebase Auth|Secret Manager|Domains And HTTPS|Service Accounts|Secrets And Runtime Config|CI/CD Responsibilities|Rollback|Failure Modes|CH-453|CH-454|CH-455|CH-456|CH-459|CH-462|cloud-run-vs-firebase-only|product-direction|Current repo structure" docs/production-deployment-plan.md`.
- Passed: changed-file scope check showed only `docs/production-deployment-plan.md` and `docs/setup.md` changed.
- Passed: deployment artifact scan found no Terraform, Dockerfile, deploy YAML, tfplan, or tfstate files tracked.
- Passed: no production deployment code was added.

Blockers:
- None. CH-451 is Done. Active CH-445/CH-446 work owns auth/Firestore implementation, so CH-452 stayed docs-only.

Commit:
- `e86ebfb` docs: choose production deployment path

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/5

Notes:
- Branch: `thorben/ch-452-release-choose-production-architecture-and-deployment-plan`.
- PR is draft per publish workflow.
- Open PRs were clear before implementation.
- No Terraform, Dockerfile, Cloud Run service, Scheduler job, workload identity binding, or production deployment automation was added.

### 2026-04-28T13:06:33.909+00:00 - thorben

Queued as the release deployment-planning lane after CH-451. Keep this issue docs-only unless its acceptance criteria explicitly require a small config change. Avoid auth, Firestore repository, route, and frontend login files while CH-445 is active.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-452 - Release choose production architecture and deployment plan.json`

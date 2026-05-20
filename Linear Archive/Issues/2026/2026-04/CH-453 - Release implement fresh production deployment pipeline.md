---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- linear
- linear-archive
- done-issue
- deployment
- ci-cd
- production
- cloud-run
- firebase
- github-actions
- infrastructure
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

# CH-453 - Release: implement fresh production deployment pipeline

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-453/release-implement-fresh-production-deployment-pipeline
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T11:15:24.678+00:00
- **Completed:** 2026-04-28T14:40:15.714+00:00
- **Updated:** 2026-04-28T14:40:15.725+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Implement the production deployment path selected by the release architecture issue.
* Configure build, deploy, runtime env, service identity, health checks, and rollback for putz-munter-prod.
* Wire deployment to GitHub Actions or the chosen managed platform from a clean baseline.
* Keep production secrets in Secret Manager or platform secret storage.

Acceptance criteria:

* A fresh deploy pipeline can publish backend/frontend or the selected app shape to putz-munter-prod.
* Deployment uses GitHub OIDC or equivalent keyless auth.
* Runtime config is documented and does not require tracked secrets.
* Health check and smoke-test commands are documented.
* Rollback path is tested or dry-run documented.

Validation:

* CI passes.
* Deploy dry-run or real staging deploy proof, depending on the selected path.
* Production smoke proof only after explicit approval and required secrets exist.

Blocker rules:

* Depends on auth/session baseline and release architecture decision.
* Do not deploy with placeholder secrets.
* Do not use old toto-chores service names, Terraform state, image repositories, or scheduler jobs.

## Attachments

- [CH-453: Add production deploy pipeline](https://github.com/ThorbenWoelk/putz-munter/pull/15) (github)

## Comments

### 2026-04-28T14:26:02.336+00:00 - thorben

## Codex Workpad

Plan:
- Started from current `origin/main` on `thorben/ch-453-release-implement-fresh-production-deployment-pipeline`.
- Inspected release architecture docs, CI, runtime config, active project issues, and open PRs.
- Implemented fresh `putz-munter-prod` deployment workflow and deployable backend/frontend config.
- Documented runtime config, one-time setup, health/smoke checks, rollback, and dry-run blockers.
- Pushed PR and waited for GitHub CI.

Acceptance criteria:
- Fresh deploy pipeline: implemented in `.github/workflows/deploy-production.yml` for Cloud Run backend and Firebase App Hosting frontend.
- Keyless auth: workflow uses GitHub OIDC through `google-github-actions/auth` with WIF provider/service account inputs.
- Runtime config: documented in `docs/production-deployment.md`; no tracked secret values.
- Health/smoke: documented curl checks plus optional workflow `run_smoke` input.
- Rollback: documented Cloud Run revision traffic rollback and Firebase App Hosting commit rollback.

Validation:
- GitHub CI passed on PR #15: Repo Hygiene, Frontend Checks, Backend Checks.
- Local YAML parse passed for `.github/workflows/ci.yml`, `.github/workflows/deploy-production.yml`, and `frontend/apphosting.yaml`.
- Firebase CLI help confirmed `apphosting:rollouts:create --git-commit` and `--force` flags.
- gcloud help confirmed `run deploy` flags used by the workflow: `--source`, `--set-secrets`, `--set-env-vars`, `--service-account`.
- Local backend checks passed: `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo build`, `cargo test`.
- Local frontend checks passed: `bun run lint`, `bun x tsc --noEmit`, `bun run test`, production build with CI Firebase placeholder env.
- `git diff --check` passed.

Deploy dry-run / blocker notes:
- Real deploy dry-run was not run from this workstation because GitHub repo variables required by the workflow are absent: `FIREBASE_APP_HOSTING_BACKEND_ID`, `PROD_SMTP_HOST`, `PROD_SMTP_PORT`, `PROD_SMTP_FROM`, `PROD_SMTP_FROM_NAME`.
- `putz-munter-prod` Secret Manager contains the backend secret IDs used by the workflow: `auth-session-secret`, `smtp-user`, `smtp-password`, `vapid-public-key`, `vapid-private-key`.
- `putz-munter-prod` Secret Manager does not contain `next-public-firebase-api-key`, which App Hosting needs for `NEXT_PUBLIC_FIREBASE_API_KEY`.
- GitHub secrets already include WIF fallback names `GCP_WIF_PROVIDER` and `GCP_WIF_SA_EMAIL`; the workflow accepts those or the documented variable names.
- Docker daemon is unavailable locally, so the backend Dockerfile was not built locally. Cloud Run source deploy flag support was verified with local `gcloud` help.

Parallel safety:
- No open GitHub PRs existed before work began.
- Active parallel issue CH-465 owns mobile wrapper work; this PR does not touch mobile wrapper files.

Commit:
- `b9f0e98 feat: add production deploy pipeline`

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/15

### 2026-04-28T14:24:51.329+00:00 - thorben

Coordination handoff for Symphony:

CH-462, CH-448, CH-454, and CH-466 are Done. Implement a fresh putz-munter-prod deployment pipeline aligned with docs/production-deployment-plan.md: Firebase App Hosting frontend, Cloud Run backend, Firestore, Firebase Auth, Secret Manager, service-only scheduler/job identity, keyless GitHub OIDC where possible. Dry-run or document blockers when live project/IAM/secret values are unavailable. Do not deploy with placeholder secrets and do not reuse old toto-chores services, Terraform state, image repositories, scheduler jobs, or workload identity.

Active parallel issue: CH-465 owns Android TWA/native wrapper. Avoid touching mobile wrapper files unless CH-453 explicitly needs public app domain metadata.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-453 - Release implement fresh production deployment pipeline.json`

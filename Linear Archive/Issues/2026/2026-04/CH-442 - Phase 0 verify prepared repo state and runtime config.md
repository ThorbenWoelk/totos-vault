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

# CH-442 - Phase 0: verify prepared repo state and runtime config

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-442/phase-0-verify-prepared-repo-state-and-runtime-config
- **State:** Done (completed)
- **Priority:** Urgent
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:31:56.602+00:00
- **Completed:** 2026-04-28T12:40:54.911+00:00
- **Updated:** 2026-04-28T12:40:54.920+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Verify the prepared fresh product tree after the cleanup commit lands on main.
* Confirm old toto-chores deployment setup, legacy migration scripts, temp artifacts, personal assets, and sensitive values are absent from the current tree.
* Confirm safe env examples and setup docs cover Firebase Auth, Firestore, SMTP, auth sessions, local development, future production secret handling, auth decision, and production safety.
* Confirm GitHub Actions are CI-only and include repo hygiene, backend, and frontend checks.
* Confirm Linear rotation notes remain available for the exposed legacy SurrealDB credential and removed deployment artifacts.

Acceptance criteria:

* Main branch contains the prepared repo state.
* No tracked .env, tfplan/tfstate, service-account, screenshot, personal avatar, hardcoded password, private email, private deployment URL, legacy migration package, generated build-info file, empty output file, or unused starter SVG remains in the tree.
* No old toto-chores deployment machinery remains tracked: no deploy workflow, Terraform stack, Dockerfile-based Cloud Run setup, Vercel starter deploy artifact, old scheduler setup, or old workload identity binding config.
* Runtime setup docs explain required Firebase/GCP/SMTP/session/VAPID variables without real values.
* Docs state that production deployment is intentionally unconfigured and must be added fresh through a scoped Linear issue.
* Docs state that future migrations are written fresh from the finalized household-scoped model.

Validation:

* Run git status and report relevant dirty files.
* Run targeted rg scans for common secret names, committed .env files, personal emails, known private endpoints, known project IDs, deployment setup terms, SurrealDB terms, and deleted temp paths.
* Run cargo fmt --check and cargo test.
* Run frontend typecheck.
* Validate workflow YAML parses.
* Verify no tracked Terraform, Dockerfile, deploy workflow, Vercel starter deployment artifact, legacy migration package, empty output file, or unused create-next starter SVG remains.

Blocker rules:

* Do not rewrite Git history.
* Do not print secret values.
* Do not recreate deployment automation or legacy migration scripts in this issue.
* If a real secret is found, remove it from the tree, document exact rotation scope, and leave rotation as a human-owned blocker.

## Attachments

_No attachments._

## Comments

### 2026-04-28T11:23:00.617+00:00 - thorben

## Codex Workpad

Plan:
- Done: verified `main` contains the prepared repo state.
- Done: ran issue-required hygiene scans and artifact checks.
- Done: ran backend format/tests, frontend typecheck, and workflow YAML parse validation.
- Done: documented findings, commit/PR status, and handoff notes here.

Acceptance criteria:
- Source of truth is Linear CH-442.
- Verified `HEAD` matches `origin/main` at `26fa9ad8ab21234419e54d5b91dbbd284ddc07c6` (`chore: initial product baseline`).
- Verified no tracked deployment automation, legacy migration package, Terraform/tfstate/tfplan, Dockerfile, deploy workflow, Vercel starter artifact, scheduler/workload identity setup, known private project markers, private key marker, tracked non-example `.env`, output.txt, tsbuildinfo, or unused create-next starter SVG remains.
- Verified runtime docs cover Firebase/GCP, Firestore, SMTP, auth sessions, VAPID/notification secret handling, local development, production safety, production deployment absence, and fresh future migrations.
- Linear rotation notes for the legacy SurrealDB credential and removed deployment artifacts remain in existing CH-442 comments.

Validation:
- Passed: `git status --short --branch` -> clean `main...origin/main`.
- Passed: `git rev-parse HEAD` equals `origin/main`.
- Passed: filename-only targeted scans for common secret names, env files, personal/private markers, known private endpoint/project patterns, deployment setup terms, SurrealDB terms, and deleted temp paths. Only expected safe references remained in docs/env examples or dependency lockfiles; no secret values printed.
- Passed: `cargo fmt --check`.
- Passed: `cargo test` (28 backend tests passed).
- Passed: `npm run typecheck` in `frontend/`.
- Passed: workflow YAML parse via Ruby YAML parser for `.github/workflows/ci.yml`.
- Passed: tracked artifact absence checks for Terraform, Dockerfile, deploy workflow, Vercel starter deployment artifact, legacy migration package, output.txt, tsbuildinfo, and create-next starter SVGs.

Blockers:
- None for CH-442 acceptance. Local GitHub publishing is unavailable in this environment (`gh` token invalid; outbound git lookup failed), but no code or docs changed, so no commit or PR is required for this verification-only handoff.

Commit:
- Not applicable. Working tree has no diff.

PR:
- Not applicable. No code or docs changed.

### 2026-04-28T11:11:36.882+00:00 - thorben

CI passed on fresh root commit 26fa9ad8ab21234419e54d5b91dbbd284ddc07c6: Repo Hygiene, Backend Checks, and Frontend Checks all completed successfully in GitHub Actions run 25049305584.

### 2026-04-28T11:08:14.825+00:00 - thorben

Prepared repo state landed on main as fresh root commit 26fa9ad8ab21234419e54d5b91dbbd284ddc07c6. CH-442 is now verification work: confirm cleanup, docs, CI hygiene, and runtime config. Symphony should not redo the cleanup or recreate deleted deployment/migration artifacts.

### 2026-04-28T11:08:14.790+00:00 - thorben

Production bootstrap done: putz-munter-prod exists and is Firebase-enabled; Firestore Native is present in europe-west3; required APIs are enabled; monthly budget exists; GitHub OIDC provider is active for ThorbenWoelk/putz-munter; GitHub repo secrets GCP_PROJECT_ID, GCP_WIF_PROVIDER, and GCP_WIF_SA_EMAIL are set; Secret Manager containers exist for auth-session-secret, smtp-user, smtp-password, vapid-public-key, and vapid-private-key. Secret values were not pasted into Git or Linear.

### 2026-04-28T10:58:47.684+00:00 - thorben

Deployment cleanup note: current prep removes old toto-chores deploy workflow, Terraform stack, Dockerfiles, Vercel starter artifact, and local .terraform provider cache. Production deployment is intentionally unconfigured until a fresh Linear issue scopes it.

### 2026-04-28T10:54:43.995+00:00 - thorben

Production home update: use putz-munter-prod as the Firebase/GCP project in safe env examples, Terraform defaults, setup docs, and Cloud Run/GitHub configuration. Keep all secret values in Secret Manager or untracked local env files.

### 2026-04-28T10:48:29.174+00:00 - thorben

Rotation notes from repository prep:

- SurrealDB credential: a real password was committed in scripts/migration/*.ts. Rotate the SurrealDB admin/user password and treat the old endpoint credentials as exposed.
- GCP project identifiers and Cloud Run URLs: private project/service identifiers were committed. No secret value was present, but review IAM/WIF access and remove any obsolete deployment targets.
- Terraform plan artifacts: tracked binary tfplan files were removed from the tree. Treat any embedded prior infra values as exposed to repo readers. Rotate only if those plans contained secret material outside Secret Manager references.
- Personal auth emails: hardcoded prototype login emails were removed. No password rotation follows from those emails alone.

Do not paste replacement secret values into Linear or Git.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-442 - Phase 0 verify prepared repo state and runtime config.json`

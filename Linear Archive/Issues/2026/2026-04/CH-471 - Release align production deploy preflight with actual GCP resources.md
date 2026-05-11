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

# CH-471 - Release: align production deploy preflight with actual GCP resources

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-471/release-align-production-deploy-preflight-with-actual-gcp-resources
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T15:34:18.818+00:00
- **Completed:** 2026-04-28T15:44:08.147+00:00
- **Updated:** 2026-04-28T15:44:08.160+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Context from production preflight on 2026-04-28:

* `putz-munter-prod` exists and is active.
* Firestore `(default)` is Native mode in `europe-west3`.
* Required APIs for Run, Artifact Registry, Secret Manager, Scheduler, Firestore, Firebase, Identity Toolkit, IAM, Cloud Build, logging, and monitoring are enabled.
* Existing service accounts are `putz-munter-runtime-sa@putz-munter-prod.iam.gserviceaccount.com`, `putz-munter-scheduler-sa@putz-munter-prod.iam.gserviceaccount.com`, and `putz-munter-github-sa@putz-munter-prod.iam.gserviceaccount.com`.
* The deploy workflow currently references non-existent service accounts without the `-sa` suffix.
* Secret Manager secret containers exist, but versions are intentionally empty until human secret entry.
* Firebase App Hosting has no backend yet.

Scope:

* Align repo deployment workflow and docs with the actual existing production service account names.
* Add or update a safe production preflight checklist/script/docs so future agents can verify names, enabled APIs, Firestore mode, Secret Manager container presence, and App Hosting backend state without printing secret values.
* Keep production deploy gated. Do not enable production deploy.
* Do not add secret values, OAuth secrets, service account keys, keystores, or private household data.

Acceptance criteria:

* `.github/workflows/deploy-production.yml` uses the existing runtime and scheduler service account emails, or documents a deliberate replacement plan.
* Deployment docs no longer instruct operators to use service accounts that do not exist in `putz-munter-prod`.
* A repeatable preflight command or documented checklist verifies production readiness inputs without printing secret values.
* Remaining human blockers are explicit: secret versions, SMTP sender, VAPID values, Firebase Auth provider/OAuth setup, App Hosting backend/domain, and native QA.

Validation:

* CI passes.
* YAML/workflow syntax is valid.
* Run the new or documented preflight in read-only mode if it is a script.
* Confirm no secret values are printed in validation output, Git, PR, or Linear.

Blocker rules:

* Do not set `ENABLE_PRODUCTION_DEPLOY=true`.
* Do not create or rotate production secrets.
* Do not deploy to Cloud Run or Firebase App Hosting.
* Do not change IAM bindings unless the issue is explicitly expanded.

## Attachments

- [[codex\] Align production deploy preflight](https://github.com/ThorbenWoelk/putz-munter/pull/23) (github)

## Comments

### 2026-04-28T15:42:04.936+00:00 - thorben

Coordinator correction after PR review:

Patched the CH-471 branch to use the correct Firebase App Hosting API name: `firebaseapphosting.googleapis.com`. Reran `./scripts/production_preflight.sh --project putz-munter-prod --region europe-west3 --allow-incomplete`; it printed no secret values and confirmed the App Hosting API is enabled. Remaining blockers are now 7: no enabled versions for `auth-session-secret`, `smtp-user`, `smtp-password`, `vapid-public-key`, `vapid-private-key`; missing `next-public-firebase-api-key`; and no Firebase App Hosting backend.

### 2026-04-28T15:38:39.275+00:00 - thorben

Review note while CH-471 is in progress:

Read-only validation of `scripts/production_preflight.sh --allow-incomplete` was safe and did not print secret values. It found one repo-side correction before PR: the available Firebase App Hosting API is `firebaseapphosting.googleapis.com`, not `apphosting.googleapis.com`. Please update the preflight required API list and docs if needed, then rerun the preflight. Keep the expected blockers for missing secret versions, missing `next-public-firebase-api-key`, missing App Hosting backend, Firebase Auth/OAuth setup, and native QA.

### 2026-04-28T15:35:27.244+00:00 - thorben

## Codex Workpad

Plan:
- Completed: inspected deploy workflow and production docs for service-account references and preflight guidance.
- Completed: aligned repository workflow/docs with actual `putz-munter-prod` resource names from CH-471.
- Completed: added a read-only production preflight command that does not print secret values.
- Completed: ran required validation.
- Completed: committed, pushed, and opened a draft PR.

Acceptance criteria:
- `.github/workflows/deploy-production.yml` now uses `putz-munter-runtime-sa@putz-munter-prod.iam.gserviceaccount.com` and `putz-munter-scheduler-sa@putz-munter-prod.iam.gserviceaccount.com`.
- Workflow validation also requires `GCP_DEPLOY_SERVICE_ACCOUNT=putz-munter-github-sa@putz-munter-prod.iam.gserviceaccount.com`.
- Deployment docs and setup guidance no longer instruct production operators to use the old `putz-munter-backend`, `putz-munter-scheduler`, or `github-deploy` service account emails.
- `scripts/production_preflight.sh` verifies APIs, Firestore metadata, expected service accounts, Secret Manager container/enabled-version presence, and App Hosting backend state without reading or printing secret values.
- Human blockers are explicit in docs, script output, and PR notes.

Validation:
- Passed: repo hygiene scan from `.github/workflows/ci.yml`.
- Passed: `bash -n scripts/production_preflight.sh`.
- Passed: Ruby YAML parse for `.github/workflows/deploy-production.yml`.
- Passed: `./scripts/production_preflight.sh --project putz-munter-prod --region europe-west3 --allow-incomplete`.
- Passed: `bun install --frozen-lockfile`.
- Passed: `bun run lint`.
- Passed: `bun x tsc --noEmit`.
- Passed: `bun run test`.
- Passed: `NEXT_PUBLIC_FIREBASE_API_KEY=ci-firebase-placeholder NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=putz-munter-ci.firebaseapp.com NEXT_PUBLIC_FIREBASE_PROJECT_ID=putz-munter-ci bun run build`.
- Passed: `cargo fmt --check`.
- Passed: `cargo clippy -- -D warnings`.
- Passed: `cargo build`.
- Passed: `cargo test`.
- Note: `actionlint` is not installed locally, so workflow syntax validation used Ruby YAML parsing.

Read-only preflight findings:
- Confirmed: project `putz-munter-prod` exists.
- Confirmed: Firestore `(default)` is Native in `europe-west3`.
- Confirmed: `putz-munter-runtime-sa`, `putz-munter-scheduler-sa`, and `putz-munter-github-sa` service accounts exist.
- Blocked: `apphosting.googleapis.com` is not enabled or not visible.
- Blocked: backend Secret Manager containers exist but have no enabled versions for `auth-session-secret`, `smtp-user`, `smtp-password`, `vapid-public-key`, and `vapid-private-key`.
- Blocked: `next-public-firebase-api-key` is missing or not visible.
- Blocked: Firebase App Hosting has no backend.
- Still human readiness work: SMTP sender, VAPID values, Firebase Auth provider/OAuth setup, App Hosting backend/domain, and native QA.

Blockers:
- No coding blocker remains for CH-471.
- Production deploy must remain disabled until the readiness blockers above are cleared.

Commit:
- `f5303c1` Align production deploy preflight

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/23

### 2026-04-28T15:34:29.899+00:00 - thorben

Coordination handoff for Symphony:

Use the actual read-only production facts discovered today: `putz-munter-prod` is active; Firestore Native `(default)` is in `europe-west3`; existing service accounts are `putz-munter-runtime-sa@putz-munter-prod.iam.gserviceaccount.com`, `putz-munter-scheduler-sa@putz-munter-prod.iam.gserviceaccount.com`, and `putz-munter-github-sa@putz-munter-prod.iam.gserviceaccount.com`; Secret Manager containers exist but have no versions; Firebase App Hosting has no backend yet. Fix only repo workflow/docs/preflight evidence. Keep deploy gated. Do not print or create secrets. Do not deploy or mutate IAM.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-471 - Release align production deploy preflight with actual GCP resources.json`

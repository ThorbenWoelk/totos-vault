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

# CH-454 - Release: configure Firebase Auth, Firestore rules, and indexes

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-454/release-configure-firebase-auth-firestore-rules-and-indexes
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T11:15:25.979+00:00
- **Completed:** 2026-04-28T14:22:35.093+00:00
- **Updated:** 2026-04-28T14:22:35.106+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Configure Firebase Auth for the selected sign-in providers and local emulator flow.
* Add Firestore Security Rules and indexes for the finalized household-scoped model.
* Keep backend-owned authorization as the source of truth.
* Deny direct client writes to household data unless a Linear issue explicitly changes the authority model.

Acceptance criteria:

* Firebase Auth provider setup is documented for local, staging if used, and putz-munter-prod.
* Firestore rules prevent unauthenticated household reads and client-only writes.
* Required Firestore indexes are committed or documented.
* Emulator validation path exists for rules and auth flows.

Validation:

* Firebase emulator or rules-unit validation where practical.
* Backend tests still pass.
* Manual setup checklist confirms putz-munter-prod provider state without printing secrets.

Blocker rules:

* Depends on CH-444 and CH-445.
* Do not relax rules to satisfy frontend behavior.
* Do not store Firebase service credentials in Git.

## Attachments

- [CH-454: configure Firebase rules and indexes](https://github.com/ThorbenWoelk/putz-munter/pull/14) (github)

## Comments

### 2026-04-28T14:22:05.488+00:00 - thorben

Coordinator decision: PR #14 is CI-green and repo-safe, so I am landing the committed Firebase rules/index/config files. The live Firebase Auth provider gap is split to CH-467, because enabling production Google sign-in requires human-owned OAuth/provider material and must not print or commit secrets.

CH-454 can close after the repo changes land; CH-467 tracks the remaining production console/provider setup.

### 2026-04-28T14:05:41.814+00:00 - thorben

## Codex Workpad

Plan:
- Done: started from current `origin/main`, rebased after `origin/main` moved, and worked on `thorben/ch-454-release-configure-firebase-auth-firestore-rules-and-indexes`.
- Done: inspected existing Firebase/Auth/Firestore model, active project issues, and open PR overlap.
- Done: added Firebase config, Firestore rules, indexes, emulator rules-unit validation, and release/setup docs within CH-454 scope.
- Done: committed, pushed, opened a draft PR, and passed PR CI.
- Done: deployed Firestore rules and indexes to staging and production.
- Done: created Firebase Web app entries for staging and production.
- Blocked: production Google sign-in provider still needs OAuth client material.

Acceptance criteria:
- Firebase Auth provider setup is documented for local, staging if used, and `putz-munter-prod`: done in `docs/setup.md`.
- Firestore rules prevent unauthenticated household reads and client-only writes: done in `firestore.rules`, validated by rules-unit tests, deployed to staging and production.
- Required Firestore indexes are committed or documented: done in `firestore.indexes.json` and `docs/firestore-model.md`, deployed to staging and production.
- Emulator validation path exists for rules and auth flows: done in `firebase.json`, `frontend/package.json`, `frontend/src/firebase/firestore.rules.test.mjs`, `docs/setup.md`, and `docs/test-strategy.md`.

Live Firebase state:
- `putz-munter-staging`: Firestore rules/indexes deployed. Firebase Web app `Putz Munter Web Staging` exists. Redacted Identity Toolkit check shows Google provider configured and enabled.
- `putz-munter-prod`: Firestore rules/indexes deployed. Firebase Web app `Putz Munter Web` exists. Redacted Identity Toolkit check shows authorized domains include `putz-munter-prod.firebaseapp.com`, `putz-munter-prod.web.app`, `app.putzmunter.de`, and `putzmunter.de`.

Validation:
- Passed: `cd frontend && bun install --frozen-lockfile`.
- Passed: `cd frontend && bun run test:firestore-rules`.
- Passed: `cd backend && cargo test`.
- Passed: `cd frontend && bun run lint && bun x tsc --noEmit && bun run test`.
- Passed: GitHub PR checks: Repo Hygiene, Frontend Checks, Backend Checks.
- Passed: `firebase deploy --project putz-munter-staging --config firebase.json --only firestore:rules,firestore:indexes`.
- Passed: `firebase deploy --project putz-munter-prod --config firebase.json --only firestore:rules,firestore:indexes`.
- Partial production checklist: `putz-munter-prod` project and Web app exist, domains are configured, Firestore rules/indexes are deployed. Google provider remains missing.

Blockers:
- `putz-munter-prod` Google sign-in provider is not configured through the Identity Toolkit provider endpoint. The supported endpoint requires OAuth client material and rejects an enabled Google provider without a `client_id`.
- Production Auth config reports email/password as configured. The documented target keeps email/password local-emulator only unless a Linear issue expands production providers. Do not disable it until Google sign-in is configured, or production would have no confirmed sign-in provider.
- CH-444 and CH-445 are Done.
- Active overlap scan: CH-447 has an open frontend UI/auth PR; CH-466 is active for backend onboarding/invite endpoints. This branch avoided those owned files.

Commit:
- `d1beb1d` (`CH-454 configure Firebase rules and indexes`).

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/14.
- Issue moved to Blocked. Do not move to In Review until the production Google provider blocker is resolved and the checklist passes.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-454 - Release configure Firebase Auth Firestore rules and indexes.json`

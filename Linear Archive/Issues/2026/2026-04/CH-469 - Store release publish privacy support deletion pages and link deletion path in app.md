---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- linear
- done-issue
- privacy
- account-deletion
- support
- putz-munter
- store
- settings
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

# CH-469 - Store release: publish privacy/support/deletion pages and link deletion path in app

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-469/store-release-publish-privacysupportdeletion-pages-and-link-deletion
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T15:04:37.880+00:00
- **Completed:** 2026-04-28T15:28:15.550+00:00
- **Updated:** 2026-04-28T15:28:15.561+00:00
- **Labels:** -

## Description

Source: CH-458 Google Play store release package.

Scope:

* Publish public web pages for privacy policy, support, and account deletion using the CH-458 drafts or reviewed replacements.
* Verify `support@putz-munter.de` or the chosen support contact receives mail and is monitored.
* Link privacy, support, and account deletion paths from an account/settings area inside the app.
* Keep account deletion implementation behind the backend authority for auth, sessions, household roles, Firestore data, audit logging, and deletion workflow decisions.

Acceptance criteria:

* `https://putz-munter.de/privacy`, `https://putz-munter.de/support`, and `https://putz-munter.de/account-deletion` are public, active, non-PDF pages or the final chosen URLs are documented.
* The app exposes a readily discoverable account deletion request path for signed-in users.
* The deletion page lets users request account and associated data deletion without reinstalling the app.
* The privacy policy and Data safety answers stay consistent with implemented Firebase Auth, backend sessions, household-scoped Firestore data, notifications, diagnostics/logs, and TWA/browser runtime behavior.

Validation:

* Open each public URL and confirm it loads without authentication.
* Sign in with a test account and confirm the in-app deletion/support/privacy links are reachable.
* Confirm no secrets, credentials, real invite links, or private household data appear in public pages, screenshots, Git, Linear, or PR evidence.

Blocker rules:

* Do not delete production accounts or household data during validation.
* Do not add broad account deletion backend behavior without explicit acceptance criteria for retention, household ownership, audit logs, and rollback.

## Attachments

- [CH-469: add public privacy support deletion pages](https://github.com/ThorbenWoelk/putz-munter/pull/22) (github)

## Comments

### 2026-04-28T15:15:10.470+00:00 - thorben

## Codex Workpad

Plan:
- Done: confirmed issue state, moved CH-469 to In Progress, and checked active Linear/GitHub overlap.
- Done: rebased from current origin/main before PR after main moved.
- Done: added public privacy, support, and account deletion pages from the CH-458 store-release draft direction.
- Done: linked privacy/support/account-deletion paths from the signed-in household settings surface.
- Done: kept deletion request-only through support email; no backend deletion workflow was added.

Acceptance criteria:
- Done locally: `/privacy`, `/support`, and `/account-deletion` exist as public non-PDF App Router pages.
- Done locally: signed-in users see a discoverable `Rechtliches & Support` settings section with account deletion, privacy, and support links.
- Done locally: deletion page supports a request path without reinstalling the app via `mailto:support@putz-munter.de`.
- Done: public page copy matches the implemented Firebase Auth, backend session, household-scoped Firestore, notifications, diagnostics/logs, and TWA/browser runtime direction.

Validation:
- Passed: `bun run lint`.
- Passed: `bun run typecheck`.
- Passed: `NEXT_PUBLIC_FIREBASE_PROJECT_ID=putz-munter-local NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=putz-munter-local.firebaseapp.com NEXT_PUBLIC_FIREBASE_API_KEY=local-firebase-emulator-key NEXT_PUBLIC_FIREBASE_AUTH_EMULATOR_HOST=127.0.0.1:9099 bun run build`.
- Passed: `bun run test`.
- Passed locally: unauthenticated `http://localhost:3100/privacy`, `/support`, and `/account-deletion` returned `200 text/html`.
- Passed locally: signed-in emulator-compatible test session rendered `Rechtliches & Support` with links to `/privacy`, `/support`, and `/account-deletion`.
- Passed: changed public surfaces scanned for secrets, credentials, real invite links, private household data, and production deletion actions.

Blockers:
- Production URL validation for `https://putz-munter.de/privacy`, `https://putz-munter.de/support`, and `https://putz-munter.de/account-deletion` requires this branch to be deployed to the production domain.
- `support@putz-munter.de` mailbox receipt/monitoring could not be verified from this workspace; needs mailbox-owner confirmation.

Commit:
- `f7f2009` `feat: add public privacy support deletion pages`.

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/22

### 2026-04-28T15:14:18.107+00:00 - thorben

Coordination handoff for Symphony:

CH-458 and CH-464 are Done. Add repo-owned public privacy/support/account-deletion pages and link paths from the app account/settings surface where the current UI allows it. Public production URL validation may be blocked until deployment/domain/TLS are ready; if so, land the app/pages/docs safely and record the exact live-domain blocker. Keep backend authority for any actual deletion workflow; do not implement broad deletion behavior in this issue. Do not include secrets, credentials, real invite links, private household data, or production account deletion actions.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-469 - Store release publish privacy support deletion pages and link deletion path in app.json`

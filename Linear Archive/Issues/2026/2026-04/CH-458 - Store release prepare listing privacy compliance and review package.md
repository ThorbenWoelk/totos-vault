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

# CH-458 - Store release: prepare listing, privacy, compliance, and review package

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-458/store-release-prepare-listing-privacy-compliance-and-review-package
- **State:** Done (completed)
- **Priority:** Medium
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T11:15:31.293+00:00
- **Completed:** 2026-04-28T15:10:10.307+00:00
- **Updated:** 2026-04-28T15:10:10.318+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Prepare store listing material and compliance answers for the chosen store target.
* Include app name, short/long descriptions, screenshots, icons, category, support contact, privacy policy, terms if needed, data-safety/privacy nutrition answers, age rating, permissions rationale, and account deletion path.
* Verify current Google Play Console and/or App Store Connect requirements when this issue starts.

Acceptance criteria:

* Store metadata draft exists in repo docs or a linked artifact without secrets.
* Privacy policy and support/contact paths are ready for review.
* Data collection and account deletion answers match the implemented app behavior.
* Screenshot checklist covers logged-out, onboarding, household, rooms/tasks, and notifications where relevant.

Validation:

* Review checklist against the current store console requirements.
* No private household data appears in screenshots or listing copy.

Blocker rules:

* Depends on mobile release target and production auth/data behavior.
* Do not submit to a store from this issue unless explicitly scoped.

## Attachments

- [CH-458 Google Play store release package](https://github.com/ThorbenWoelk/putz-munter/pull/19) (github)

## Comments

### 2026-04-28T15:02:38.387+00:00 - thorben

## Codex Workpad

Plan:
- Done: moved CH-458 to In Progress and worked from current `origin/main` on `thorben/ch-458-store-release-prepare-listing-privacy-compliance-and-review`.
- Done: inspected active Putz & Munter issues and open PRs for overlap. Open PRs were empty. CH-468 owns backend audit-event implementation, so this work avoided backend code.
- Done: verified Google Play listing, Data safety, preview asset, account deletion, User Data, review content, and target API requirements against official sources on 2026-04-28.
- Done: added repo-owned Google Play store release package documentation with listing draft, privacy/support/deletion paths, Data safety answers, age/content notes, screenshot checklist, and reviewer package.
- Done: committed, pushed, and opened a draft PR.

Acceptance criteria:
- Done: store metadata draft exists in `docs/google-play-store-release-package.md` without secrets.
- Done: privacy policy, support/contact, and account deletion path drafts are ready for review in `docs/google-play-store-release-package.md`.
- Done: Data safety and account deletion answers match the implemented Firebase Auth, backend session, household-scoped Firestore, notification, diagnostics/logging, and TWA/browser runtime behavior documented in the repo.
- Done: screenshot checklist covers logged-out, onboarding, household/settings, rooms/tasks, calendar, and notifications where relevant.

Validation:
- Passed: reviewed package against current Google Play official requirements for app setup/store listing, Data safety, User Data/privacy policy, account deletion, preview assets, app review content, and target API.
- Passed: `git diff --check`.
- Passed: listing lengths checked: short description 58/80, full description 1278/4000.
- Passed: Android manifest permission check confirmed only `android.permission.INTERNET`.
- Passed: no screenshots were added; package copy requires fictional test data and rejects private household data, real invite links, tokens, credentials, debug overlays, and localhost URLs.

Blockers:
- None for CH-458.

Follow-up:
- Created CH-469 in Backlog for publishing the privacy/support/account-deletion pages and linking the in-app account deletion path before store submission.

Commit:
- `2fbb273` Add Google Play store release package.

PR:
- Draft PR: https://github.com/ThorbenWoelk/putz-munter/pull/19

### 2026-04-28T15:00:38.348+00:00 - thorben

Coordination handoff for Symphony:

CH-464 Google Play readiness checklist is Done. Build the fuller store release package for Google Play first: listing draft, privacy/support/account deletion paths, data-safety answers, age/content notes, screenshot plan with test data, and review package. Use docs/google-play-readiness.md as the checklist baseline and cite official store sources already listed there. Do not submit to a store. Do not include credentials, signing material, OAuth material, private household data, real invite links, or screenshots with real data. Active parallel issue CH-468 owns backend audit-event implementation; avoid backend code.

### 2026-04-28T13:52:38.707+00:00 - thorben

CH-463 decision input:

Compliance and listing package should target Google Play first for the Android TWA.

Scope to carry here:
- Draft Google Play listing, privacy policy/support/deletion links, age/content answers, screenshot plan, and Data safety answers for the implemented Android TWA behavior.
- Treat Firebase Auth, backend session cookies, household-scoped chore data, notifications, diagnostics/logs, and wrapper SDKs as data-practice inputs.
- Verify current Play Console requirements again when this issue starts.
- Do not submit to a store or paste credentials, signing material, secrets, or private household data.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-458 - Store release prepare listing privacy compliance and review package.json`

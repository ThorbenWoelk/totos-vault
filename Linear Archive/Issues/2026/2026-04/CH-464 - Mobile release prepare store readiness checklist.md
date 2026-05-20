---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- android
- checklist
- documentation
- google-play
- mobile
- putz-munter
- release
- twa
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

# CH-464 - Mobile release: prepare store readiness checklist

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-464/mobile-release-prepare-store-readiness-checklist
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T13:39:18.202+00:00
- **Completed:** 2026-04-28T15:00:16.702+00:00
- **Updated:** 2026-04-28T15:00:16.716+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Prepare the app-store readiness checklist after the native wrapper path exists.
* Cover app metadata, privacy labels, permissions rationale, screenshots with test data, support URL, deletion/account support notes, signing/release tracks, and release smoke checks.
* Identify which steps Thorben must complete in Apple Developer or Google Play Console.

Acceptance criteria:

* Store-readiness doc lists required assets, account actions, privacy/security answers, release-track steps, and final smoke checks.
* Screenshots or screenshot plan use test data only.
* Human-owned account/signing steps are clearly separated from repo automation.

Validation:

* Documentation review against the selected first store target.
* No signing keys, store API credentials, private household data, or secrets appear in Git or Linear.

Blocker rules:

* Depends on CH-463 and the selected native wrapper build issue.
* Verify current store review requirements when this issue starts.
* Do not submit to any store from this issue.

## Attachments

- [CH-464: Add Google Play readiness checklist](https://github.com/ThorbenWoelk/putz-munter/pull/17) (github)

## Comments

### 2026-04-28T14:55:12.930+00:00 - thorben

## Codex Workpad

Plan:
- Done: Confirmed CH-464 state and dependencies. CH-463 and CH-465 are Done.
- Done: Moved CH-464 from Todo to In Progress before implementation.
- Done: Inspected active Putz & Munter work and open PRs for ownership overlap. No open PRs. CH-456 is active on ops/runbook docs, so CH-464 used a separate store-readiness doc.
- Done: Verified Google Play readiness requirements from official Android and Play Console sources on 2026-04-28.
- Done: Added `docs/google-play-readiness.md` for the Android TWA Google Play path.
- Done: Committed, pushed, and opened PR #17.

Acceptance criteria:
- Done: Store-readiness doc lists required assets, account actions, privacy/security answers, release-track steps, and final smoke checks.
- Done: Screenshot plan uses test household data only and excludes private household data, real emails, invite links, localhost/debug surfaces, credentials, and internal tracker content.
- Done: Human-owned Play Console, account, and signing steps are separated from repo-owned readiness checks.

Validation:
- Passed: Documentation review against Google Play first target and CH-464 acceptance criteria.
- Passed: Official sources cited in the doc include Google Play target API, Android App Bundle, Play App Signing, Data safety, User Data policy, account deletion, app setup/store listing, preview assets, release tracks, and Trusted Web Activity docs.
- Passed: `git diff --cached --check`.
- Passed: Targeted scan of `docs/google-play-readiness.md` for API keys, private keys, client secrets, tokens, service-account markers, assignment-style passwords, `.jks` path, and email-shaped strings returned no matches.

Blockers:
- None.

Commit:
- `5314133` docs: add Google Play readiness checklist

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/17

### 2026-04-28T14:53:26.537+00:00 - thorben

Coordination handoff for Symphony:

CH-463 and CH-465 are Done. Target Google Play first for the Android TWA. Verify against current store requirements and cite sources in the doc. Official checks to include:
- Google Play target API requirement: new apps and updates must target Android 15 / API 35 or higher from August 31, 2025: https://developer.android.com/google/play/requirements/target-sdk
- Android App Bundle is required for new Google Play apps and Play generates optimized APKs: https://developer.android.com/guide/app-bundle/
- Play release signing uses an upload key and Play App Signing: https://developer.android.com/guide/publishing/app-signing.html
- Google Play User Data policy requires a public privacy policy, data retention/deletion disclosure, and account deletion paths when accounts can be created: https://support.google.com/googleplay/android-developer/answer/10144311 and https://support.google.com/googleplay/android-developer/answer/13327111

Do not submit to Play. Do not include signing keys, store API credentials, screenshots with private household data, OAuth material, or secrets in Git/Linear. Active parallel issue CH-456 should own ops/runbook docs; avoid editing the same runbook files unless needed.

### 2026-04-28T13:52:38.327+00:00 - thorben

CH-463 decision input:

Store readiness should target Google Play first for an Android Trusted Web Activity.

Checklist scope to carry here:
- Google Play listing metadata, screenshots with test data, category, support URL, privacy policy, and account deletion path.
- Data safety answers for Firebase Auth, backend sessions, household chore data, push notifications, logs/diagnostics, and any SDK behavior in the TWA wrapper.
- Android App Bundle, Play App Signing/upload-key human steps, and release-track smoke checks.
- Digital Asset Links verification checks so the app launches as TWA instead of Custom Tab fallback.
- No store submission, signing material, private household data, or credentials in Git or Linear.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-464 - Mobile release prepare store readiness checklist.json`

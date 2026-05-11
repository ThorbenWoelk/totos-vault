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

# CH-465 - Mobile release: implement selected native wrapper build

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-465/mobile-release-implement-selected-native-wrapper-build
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T13:39:18.458+00:00
- **Completed:** 2026-04-28T14:49:53.375+00:00
- **Updated:** 2026-04-28T14:49:53.386+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Implement the native wrapper/runtime selected by CH-463.
* Add package or bundle identifiers, app icons/splash placeholders or final assets, permissions, Firebase app registration notes if needed, deep links/app links if needed, and production backend configuration.
* Add local Mac build/run commands for the selected first target.
* Keep signing credentials outside Git and document where humans create them.

Acceptance criteria:

* The selected native app can be built and installed or run on at least one target: Android emulator/device, iOS simulator/device, or the first explicitly chosen store target.
* App startup reaches the intended backend/Auth environment without localhost as the primary QA target.
* Unauthenticated users cannot see household data.
* Native E2E/manual QA notes link to CH-460 and CH-461.

Validation:

* Local native build proof for the selected artifact or documented blocker if platform tooling/account access is unavailable.
* Manual install/smoke notes on one simulator, emulator, or device.
* CI path added if practical.

Blocker rules:

* Depends on CH-463 and enough CH-447 auth/onboarding UI to test the real journey.
* Do not commit signing keys, provisioning profiles, keystores, store API credentials, or secrets.
* Do not alter backend authority or move to client-only Firebase without a dedicated decision issue.

## Attachments

- [CH-465 Add Android TWA wrapper](https://github.com/ThorbenWoelk/putz-munter/pull/16) (github)

## Comments

### 2026-04-28T14:26:18.684+00:00 - thorben

## Codex Workpad

Plan:
- Done: Confirmed CH-465 was Todo and moved it to In Progress before implementation.
- Done: Confirmed selected native path from CH-463: Android Trusted Web Activity with Bubblewrap and Digital Asset Links.
- Done: Checked parallel ownership: CH-453 is active on deployment pipeline files; CH-465 stayed in Android TWA wrapper/public metadata/docs lanes and avoided deployment workflow files.
- Done: Started branch from current origin/main: `thorben/ch-465-mobile-release-implement-selected-native-wrapper-build`.
- Done: Added Android TWA wrapper project under `mobile/android-twa` with package id `de.putzmunter.app`, targetSdk 35, TWA launcher metadata, app-link intent filter, placeholder icons/splash, Bubblewrap manifest, and signing-key ignore rules.
- Done: Updated web manifest metadata and added `frontend/public/.well-known/assetlinks.json` for Digital Asset Links plumbing.
- Done: Added `docs/android-twa-release.md` and `mobile/android-twa/README.md` with Mac build/run commands, human-owned signing steps, CH-460/CH-461 native smoke note requirements, and blockers.
- Done: Committed, pushed, and opened PR #16.

Acceptance criteria:
- Native Android TWA wrapper can be built locally: debug APK and release AAB compile from `mobile/android-twa`.
- App startup target is the production app origin `https://app.putz-munter.de` and backend origin `https://api.putz-munter.de`; localhost is not the primary QA target.
- Unauthenticated household data blocking remains covered by existing web/API smoke test: `core product smoke scenario > rejects protected household reads without an accepted session`.
- Native manual QA notes template links to CH-460 and CH-461 in `docs/android-twa-release.md` and `mobile/android-twa/README.md`.
- Signing keys, upload keys, keystores, Play credentials, OAuth secrets, and Firebase Admin credentials are absent from Git and Linear.

Validation:
- Passed: `jq empty frontend/public/manifest.json frontend/public/.well-known/assetlinks.json mobile/android-twa/twa-manifest.json`.
- Passed: `xmllint --noout` for Android XML resources and manifest.
- Passed: `git diff --check` and `git diff --cached --check`.
- Passed: `npm run typecheck` in `frontend/`.
- Passed: `npm run lint` in `frontend/`.
- Passed: frontend production build with CI placeholder Firebase public config.
- Passed: `bun run test` in `frontend/` (39 unit tests, 2 smoke tests).
- Passed: Android debug APK build using temporary JDK 17 and Gradle 8.13: `:app:assembleDebug` produced `mobile/android-twa/app/build/outputs/apk/debug/app-debug.apk`.
- Passed: Android release App Bundle build using temporary JDK 17 and Gradle 8.13: `:app:bundleRelease` produced `mobile/android-twa/app/build/outputs/bundle/release/app-release.aab`.
- Passed: staged secret scan for private-key/API-token patterns; only the CI regex itself matched before excluding `.github/workflows/ci.yml`.
- Passed: no tracked `.gradle`, `local.properties`, keystore, or Android build output files.

Blockers / smoke notes:
- Production-like HTTPS app domain is not ready: `curl -I https://app.putz-munter.de/manifest.json` fails TLS validation because the certificate does not match `app.putz-munter.de`; `curl -k` reaches a 403. TWA validation and real startup smoke should wait for the domain.
- `frontend/public/.well-known/assetlinks.json` contains a placeholder SHA-256 fingerprint. Replace it with the Google Play App Signing fingerprint, or an exact QA signing fingerprint, before expecting Digital Asset Links to verify.
- Manual install smoke is blocked locally: `adb devices` showed no attached devices, and `avdmanager list avd` showed `Medium_Phone_API_36.1` cannot load because its Google Play arm64-v8a system image is missing.
- Native CI was not added because CH-453 is active on deployment/workflow ownership; docs record this as a follow-up lane.

Commit:
- `62ded12` CH-465 add Android TWA wrapper

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/16

### 2026-04-28T14:24:52.456+00:00 - thorben

Coordination handoff for Symphony:

CH-463 and CH-447 are Done. Implement the Android Trusted Web Activity path first, using Bubblewrap/Digital Asset Links and production-like HTTPS domains from docs/mobile-release-decision.md. Keep signing keys, upload keys, keystores, Play credentials, OAuth secrets, and Firebase Admin credentials out of Git and Linear. If Android tooling/account material is missing, document the exact blocker and still land safe repo setup, manifest, assetlinks, build docs, and validation notes where possible.

Active parallel issue: CH-453 may touch deployment config and docs. Avoid overlapping deploy workflow files unless needed; coordinate through PR order and rebase before review.

### 2026-04-28T13:52:38.159+00:00 - thorben

CH-463 decision input:

Selected first native path is Android Trusted Web Activity for Google Play, generated with Bubblewrap and verified by Digital Asset Links.

Implementation scope to carry here:
- Add Android TWA wrapper project and package identifier.
- Ensure `https://app.putz-munter.de/manifest.json` and `/.well-known/assetlinks.json` support TWA verification.
- Build a Google Play-ready Android App Bundle path targeting the current Play target API requirement.
- Use production or staging production-like HTTPS app/API domains for Mac QA, not localhost.
- Keep signing keys, upload keys, keystores, Play Console credentials, and secrets outside Git and Linear.
- Prove install/run on Android emulator or physical Android device and record smoke notes for CH-461.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-465 - Mobile release implement selected native wrapper build.json`

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

# CH-463 - Mobile release: decide native wrapper and first store target

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-463/mobile-release-decide-native-wrapper-and-first-store-target
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T13:39:03.340+00:00
- **Completed:** 2026-04-28T13:55:49.996+00:00
- **Updated:** 2026-04-28T13:55:50.022+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Decide the first native publication path for Putz & Munter. Compare Capacitor, native shell, Trusted Web Activity for Android-only, React Native rewrite slice, and any clearly better store-accepted path.
* Verify current Apple App Store and Google Play technical constraints before recommending the path.
* Choose the first store target and explain what can be tested on Thorben's Mac before store submission.
* Define how the native app reaches the production backend and Firebase Auth flow without localhost as the primary QA path.
* Create or update follow-up issues for implementation, signing/assets, native E2E, and store readiness.

Acceptance criteria:

* A repo doc records the chosen wrapper/runtime path, first store target, and rejected alternatives.
* The decision includes current store constraint notes with source links and dates checked.
* The decision names the local Mac QA path: simulator/emulator/device, build command, backend target, and expected smoke journey.
* Follow-up Linear issues are linked or updated with precise implementation scope.

Validation:

* Documentation review only.
* No native build is required in this issue.
* No signing keys, provisioning profiles, store API credentials, or secrets are committed or pasted into Linear.

Blocker rules:

* Do not implement a wrapper before the decision is written.
* Do not choose client-only Firebase unless a dedicated Linear issue explicitly changes the authority model.
* If current store requirements are unclear, stop at a documented blocker with links to the exact missing account/tooling access.

## Attachments

- [CH-463: decide Android TWA mobile release path](https://github.com/ThorbenWoelk/putz-munter/pull/10) (github)

## Comments

### 2026-04-28T13:50:18.796+00:00 - thorben

## Codex Workpad

Plan:
- Done: verified current Apple App Store, Google Play, Capacitor, TWA, and Next.js constraints from primary sources.
- Done: compared Capacitor, native iOS shell, native Android shell, Android TWA, React Native rewrite slice, and PWA-only path.
- Done: added `docs/mobile-release-decision.md` with the selected Android TWA / Google Play first path.
- Done: linked downstream Linear work through comments on CH-465, CH-464, CH-461, and CH-458.
- Done: committed, pushed, and opened PR.

Acceptance criteria:
- Done: repo doc records the chosen wrapper/runtime path, first store target, and rejected alternatives.
- Done: decision includes store constraint notes with source links and date checked: 2026-04-28.
- Done: decision names the local Mac QA path: Android emulator/device, Bubblewrap commands, production HTTPS backend target, and smoke journey.
- Done: follow-up Linear issues were updated with precise implementation, signing/assets, native QA, and store readiness scope.

Validation:
- Passed: documentation review only, per CH-463.
- Passed: `git diff --check origin/main...HEAD`.
- Native build intentionally not run; CH-463 explicitly requires no native build.

Blockers:
- None.

Commit:
- `2a3bb3a` docs: decide Android TWA mobile release path

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/10


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-463 - Mobile release decide native wrapper and first store target.json`

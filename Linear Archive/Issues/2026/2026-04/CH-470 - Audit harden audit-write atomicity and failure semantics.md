---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- audit
- atomicity
- failure-semantics
- backend
- firestore
- rust
- transaction
- idempotency
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

# CH-470 - Audit: harden audit-write atomicity and failure semantics

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-470/audit-harden-audit-write-atomicity-and-failure-semantics
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T15:13:44.758+00:00
- **Completed:** 2026-04-28T15:23:59.544+00:00
- **Updated:** 2026-04-28T15:23:59.554+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Follow-up from CH-468 review.

Scope:

* Decide and implement the production policy for audit-write failures around household mutations.
* Avoid returning API success when required audit evidence is missing, and avoid returning a post-mutation 500 that encourages unsafe retries after the domain mutation already committed.
* Prefer an atomic Firestore write batch/transaction for domain mutation plus audit event where the repository supports it. If atomic writes are not practical for a path, document whether audit is best-effort or fail-closed and why.
* Cover chore mutations, invite/member role actions, household creation/switching, auth session events, and service-only jobs.

Acceptance criteria:

* Audit failure behavior is explicit and consistent by route class.
* At least one mutation path test proves the chosen behavior.
* No audit metadata includes secrets, raw tokens, invite tokens, SMTP/push payloads, or sensitive household free text.
* Operations docs mention the chosen failure policy.

Validation:

* Backend tests pass.
* Focused review of retry/idempotency behavior.

Blocker rules:

* Do not weaken household authorization or Firestore tenancy to make audit writes pass.
* Do not paste production household data or secrets into tests, logs, Git, or Linear.

## Attachments

- [CH-470: Harden audit write failure policy](https://github.com/ThorbenWoelk/putz-munter/pull/21) (github)

## Comments

### 2026-04-28T15:15:36.820+00:00 - thorben

## Codex Workpad

Plan:
- Inspected CH-468 audit implementation across auth, household, chore, service job, repository, and operations docs.
- Added explicit audit route classes and write policies.
- Moved chore room mutations to Firestore transactions that commit room/log changes with the audit event.
- Converted post-commit/non-atomic classes to explicit best-effort audit handling to avoid returning retryable post-mutation 500s.
- Updated production operations docs with policy and retry/idempotency notes.

Acceptance criteria:
- Audit failure behavior explicit and consistent by route class: done in `backend/src/audit.rs` and `docs/production-operations.md`.
- At least one mutation path test proves chosen behavior: done via audit route-class policy tests covering chore mutations and post-commit household profile mutations.
- Audit metadata excludes secrets, raw tokens, invite tokens, SMTP/push payloads, and sensitive household free text: preserved existing sanitizer; reviewed changed audit metadata keys.
- Operations docs mention the chosen failure policy: done.

Validation:
- Passed: `cargo test` in `backend` on 2026-04-28.
- Passed: `git diff --check`.
- Focused retry/idempotency review: chore mutations now fail before success when the atomic commit fails; auth session creation fails before cookie success; best-effort post-commit classes are documented with inspect-before-retry guidance.

Blockers:
- None.

Commit:
- `51c25e7` fix: harden audit write failure policy

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/21

### 2026-04-28T15:14:18.111+00:00 - thorben

Coordination handoff for Symphony:

CH-468 is Done and introduced audit writes. This issue owns the follow-up policy for audit write failure semantics. Prefer a small implementation: define a consistent helper/policy, add at least one backend test proving the chosen behavior, and update production operations docs. If Firestore atomic batch/transaction support is not locally practical, document route classes as best-effort or fail-closed with retry/idempotency notes. Do not weaken auth/tenancy and do not include secrets or private household data.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-470 - Audit harden audit-write atomicity and failure semantics.json`

---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- backend
- audit
- security
- firestore
- rust
- household
- linear-archive
- done-issue
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

# CH-468 - Implement backend audit-event writes for household security actions

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-468/implement-backend-audit-event-writes-for-household-security-actions
- **State:** Done (completed)
- **Priority:** None
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T14:57:09.056+00:00
- **Completed:** 2026-04-28T15:13:56.522+00:00
- **Updated:** 2026-04-28T15:13:56.536+00:00
- **Labels:** -

## Description

Follow-up from CH-456.

The production operations runbook documents audit-event inspection under `households/{householdId}/audit_events/{eventId}`, and the repo has `AuditEvent` models plus Firestore audit indexes. Code review for CH-456 did not find backend writes for security-relevant audit events.

Scope:

* Add backend-owned audit-event writes for membership invites, role changes, household settings changes, chore mutations, auth/session security events where useful, and service-only job events.
* Keep audit writes household-scoped in Firestore.
* Never write secrets, raw session tokens, Firebase ID tokens, invite tokens, SMTP payloads, push keys, or sensitive household free text into audit metadata.
* Preserve backend authority for actor, household, target, and timestamp fields.

Acceptance criteria:

* Security-relevant household actions create append-only audit events.
* Audit events can be queried by latest household events, actor user id, and target document.
* Tests cover at least one write path, one role/security path, and metadata redaction constraints.
* Production operations docs remain accurate after implementation.

## Attachments

- [CH-468 Implement backend audit event writes](https://github.com/ThorbenWoelk/putz-munter/pull/20) (github)

## Comments

### 2026-04-28T15:01:34.365+00:00 - thorben

## Codex Workpad

Plan:
- Done: started branch `thorben/ch-468-implement-backend-audit-event-writes-for-household-security` from current `origin/main`.
- Done: inspected existing `AuditEvent` model, Firestore indexes, backend auth/session/household/chore/service write paths, and operations docs.
- Done: added backend-owned household-scoped audit writes for current security-relevant paths without metadata secrets or sensitive free text.
- Done: added focused tests for a normal audit write path, role/security actor context, and metadata redaction.
- Done: updated operations/model docs so audit inspection reflects the implemented backend write path.

Acceptance criteria:
- Done: security-relevant household actions create append-only audit events via Firestore `insert` under `households/{householdId}/audit_events/{eventId}`.
- Done: audit events can be queried by latest household events, actor user id, and target document through backend helper methods matching committed indexes.
- Done: tests cover one write path, one role/security path, and metadata redaction constraints.
- Done: production operations docs remain accurate after implementation.

Validation:
- Passed: `cd backend && cargo test` (57 tests).
- Passed: `git diff --check`.
- Docs drift check: no stale CH-468 readiness note, pending-write-path note, or planned-audit-filter wording remains.

Coordination:
- Open PR check before push: PR #19 / CH-458 touches `docs/google-play-readiness.md` and `docs/google-play-store-release-package.md`; no overlap with CH-468 files.

Blockers:
- None.

Commit:
- `99604c3` (`CH-468 add backend audit event writes`).

PR:
- https://github.com/ThorbenWoelk/putz-munter/pull/20

### 2026-04-28T15:00:38.190+00:00 - thorben

Coordination handoff for Symphony:

CH-456 created this follow-up because the repo has audit-event models/indexes and operations docs, but backend write paths are not implemented. Implement backend-owned household-scoped audit event writes for security-relevant actions. Keep metadata redacted: no secrets, raw session cookies, Firebase ID tokens, invite tokens, SMTP payloads, VAPID/private push keys, or sensitive household free text. Add tests for at least one normal write path, one role/security path, and metadata redaction. Active parallel issue CH-458 owns store listing/privacy docs; avoid release docs unless production-operations accuracy needs a small link/update.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-468 - Implement backend audit-event writes for household security actions.json`

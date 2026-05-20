---
created: 2026-05-11
last_edited: 2026-05-11
tags:
- git
- history-rewrite
- devops
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

# CH-450 - Repo: reset product history to a fresh initial commit

## Linear Metadata

- **URL:** https://linear.app/code-and-heart/issue/CH-450/repo-reset-product-history-to-a-fresh-initial-commit
- **State:** Done (completed)
- **Priority:** High
- **Project:** Putz & Munter
- **Assignee:** -
- **Created:** 2026-04-28T10:52:41.562+00:00
- **Completed:** 2026-04-28T11:08:15.674+00:00
- **Updated:** 2026-04-28T11:08:15.687+00:00
- **Labels:** -

## Description

Repository: ThorbenWoelk/putz-munter

Scope:

* Prepare a clean product history that replaces inherited toto-chores history with one initial commit of the accepted current tree.
* Confirm exact remote, branch, rollback tag/backup branch, and force-push command before execution.
* Do not change application behavior.

Acceptance criteria:

* Proposed reset plan names source commit, target branch, remote URL, backup ref, and rollback command.
* Secret scan and git status are clean enough for the initial commit.
* Human confirmation is recorded before force-updating any remote branch.

Validation:

* git status
* git remote -v
* targeted secret scan
* dry-run commands or documented no-op verification where available.

Blocker rules:

* Destructive history rewrite. Do not execute unattended.
* Do not force-push unless the issue explicitly says to execute and includes human confirmation.
* Do not delete backup refs until the new product history is accepted.

## Attachments

_No attachments._

## Comments

### 2026-04-28T11:08:15.137+00:00 - thorben

Completed manually: main was replaced with fresh root commit 26fa9ad8ab21234419e54d5b91dbbd284ddc07c6 for ThorbenWoelk/putz-munter. Local backup branch backup/pre-fresh-history preserves the previous local history.


## Raw Snapshot

- JSON snapshot: `Linear Archive/Issues/2026/2026-04/CH-450 - Repo reset product history to a fresh initial commit.json`

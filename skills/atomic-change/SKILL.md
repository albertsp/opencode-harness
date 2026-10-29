---
name: atomic-change
description: Make one logical change touching only the files named in the task, without refactors or reformatting. Use while implementing a task in an existing codebase.
---

# Atomic change

Implement exactly one logical change and nothing more.

## Before editing

- Confirm the task is in scope and the files it names.
- Stop and report if the change needs a file the task does not name, or an architectural decision.
- Read the surrounding code first and match its style, naming, and patterns.

## While editing

- Change only the files named in the task.
- Do not refactor, rename, reformat, or reorganize anything outside the task scope.
- Do not fix unrelated bugs or update unrelated comments.
- Add comments only where the behavior is not obvious.
- Make the smallest change that satisfies the task.

## After editing

- Run the relevant tests; if they fail and the fix needs out-of-scope work, stop and report.
- Leave all changes uncommitted for review. Never commit, push, reset, or delete work.

## Report

- Files changed, with a one-line description each.
- Anything left undone or uncertain.

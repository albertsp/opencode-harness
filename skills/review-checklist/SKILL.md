---
name: review-checklist
description: Audit a change set against a fixed checklist covering scope, unintended changes, tests, edge cases, error handling, and security, then return a verdict. Use for read-only code review.
---

# Review checklist

Review the change set and return a verdict on the first line: `[APPROVED]` or `[CHANGES REQUESTED]`.

## Checklist

- **Scope matches task**: every change serves the stated task.
- **No unintended changes**: no unrelated edits, refactors, renames, or reformatting.
- **Test coverage**: new behavior has tests, and the tests were run.
- **Edge cases**: empty, null, boundary, and concurrent inputs are handled.
- **Error handling**: failures are caught, reported, and do not corrupt state.
- **Security**: no secrets in code or logs; input is validated against injection; authorization is checked on every path.

## Rules

- Never modify files. Read with `git diff`, `git status`, and `git log` only.
- Cite a file path and line number for every finding.
- Order findings by severity.
- Do not suggest unrelated refactors.
- Approve only when there are no blocking findings; minor style notes do not block.

## Output

- Verdict on the first line.
- Findings as bullets, each with `path:line`.
- Test coverage: covered, partially covered, or not covered.
- Any edge cases not handled.

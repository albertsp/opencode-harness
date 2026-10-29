---
name: git-workflow
description: Git rules for this harness: branch naming, atomic conventional commits, approval before commit, PR creation with gh, and what agents must never do with git.
---

## What I do
- Define how branches, commits, and pull requests are named and created.
- Keep each commit atomic: one logical change, with its tests, in a single commit.
- Make sure every commit message follows Conventional Commits.

## Branches
- Never work on `master` or `main` directly.
- Name branches `<type>/<short-kebab-slug>`, for example `feat/token-refresh` or `fix/login-redirect`.
- Create one branch per feature. Use `git worktree` when work must be isolated.

## Conventional commits
- Format: `type(scope): imperative summary`, for example `feat(auth): add token refresh`.
- Types: `feat`, `fix`, `docs`, `test`, `refactor`, `perf`, `build`, `ci`, `chore`.
- Summary is 72 characters or fewer, imperative, with no trailing period.
- Use `!` after the type or scope, and a `BREAKING CHANGE:` footer, for breaking changes.
- Body is optional. Explain why, not what.

## When to commit
- Commit only after the user has approved the diff at the diff checkpoint.
- Commit only the files in scope for the approved step. Check with `git status` and `git diff --cached` first.
- Stage files by path, for example `git add src/auth/token.ts`. Do not use `git add -A` or `git add .`.
- Never commit secrets, credentials, `.env` files, `service.json`, or generated artifacts.

## Pull requests
- Push the branch with `git push -u origin <branch>`.
- Open the PR with `gh pr create`, using a conventional title and a body with a summary and a test plan.
- Never merge a PR. The user merges.

## Never
- `git push --force` or `--force-with-lease` to a shared branch.
- `git commit --amend` or `git rebase` on commits that have been pushed.
- `git reset --hard`, `git clean -fd`, or `git checkout --` on files with uncommitted user work.
- `--no-verify` to skip hooks.
- Commit or push without approval.

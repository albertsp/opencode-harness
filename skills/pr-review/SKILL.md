---
name: pr-review
description: Review a GitHub pull request by number or URL in any of the user's repositories, using explicit owner/repo targeting, the project's AGENTS.md standards, and a secret scan. Returns a reviewer verdict and offers numbered next steps.
---

# PR review

## What I do

- Read a pull request and its checks, then review the diff against the project's own standards.
- Scan the diff for secrets and block on any finding.
- Hand the diff and findings to the `reviewer` agent and report its verdict with file and line references.
- Offer the user numbered options for what to post. I never post, merge, or close on my own.

## When to use me

- The user asks to review a GitHub pull request by number or URL.
- The PR can be in any of the user's repositories, not just the current one.
- The user wants a verdict plus a choice of next action, not an automatic approval.

## Steps

1. Resolve the target as `owner/repo` and the PR number. Run **every** `gh` command with an explicit `-R owner/repo`; never rely on the current directory's remote, because reviews span many repos.
2. Read the PR:
   - `gh pr view <n> -R <repo>` for title, body, author, base, and state.
   - `gh pr diff <n> -R <repo>` for the full patch.
   - `gh pr checks <n> -R <repo>` for CI status.
3. Read the project's standards from its remote: `gh api repos/<owner>/<repo>/contents/AGENTS.md` and decode the `content` field, which is base64. If the file is missing, say so explicitly and review against general standards instead.
4. Scan the diff for secrets: credentials, tokens, private keys, and `.env` content. Any finding is a blocker regardless of other results.
5. Pass the diff, the standards, and the findings to the `reviewer` agent. Report its verdict, `[APPROVED]` or `[CHANGES REQUESTED]`, with a file and line reference for every finding.
6. Present the decision options below as a numbered choice and stop. Post only after the user picks one.

## Decision options

1. Approve with a comment.
2. Request changes with the findings.
3. Leave a comment only.
4. Do nothing.

## Never

- Merge a PR. Merging is always done by the user.
- Use `gh api` with write methods: `-X`, `--method`, `-f`, `-F`, `--field`, `--raw-field`.
- Push to a PR branch unless the user explicitly asks for a fix.
- Close a PR.
- Post anything to GitHub without the user's explicit choice.

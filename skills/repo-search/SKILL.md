---
name: repo-search
description: Search a repository using only grep, find, and cat, with narrow patterns and cited results. Use for read-only lookup of files, symbols, and context.
---

# Repository search

Search with `grep`, `find`, and `cat` only. Do not modify files or run other commands.

## Steps

1. Start from the question and pick the narrowest pattern that can answer it.
   - Search for a symbol before searching for a word.
   - Prefer a path restriction (`grep -r pattern src/`) over the whole repo.
2. Narrow further before broadening. Try a second pattern only when the first returns nothing useful.
3. Read the matching file with `cat` to confirm context around the hit.
4. Stop as soon as you have enough evidence to answer.

## Rules

- Never chain commands with `;`, `&&`, `|`, or redirections.
- Never use `find -exec` or `find -delete`.
- Do not guess. If a search finds nothing, say so and list what you checked.

## Output

- Return concise bullet points in the form `path:line`.
- Label each bullet as a verified fact or an inference. Separate the two groups.
- End with a line listing anything you could not find.

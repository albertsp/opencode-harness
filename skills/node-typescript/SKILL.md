---
name: node-typescript
description: Work in Node.js and TypeScript projects using their existing package manager, lint, test, and type-check scripts. Use when writing, changing, testing, or reviewing JavaScript or TypeScript code.
---

# Node + TypeScript

## What I do

- Target Node.js LTS and TypeScript with `"strict": true`.
- Default to ESM unless the project's `package.json` says otherwise.
- Use the project's existing tooling rather than introducing new tools.

## When to use me

- Adding or changing `.ts`, `.mts`, `.cts`, `.js`, or `.mjs` files.
- Setting up lint, test, or type-check commands for a Node project.
- Reviewing a Node/TypeScript change before reporting it done.

## Conventions

- Read `AGENTS.md` and the `package.json` scripts first, then use the commands they define.
- Detect the package manager from the lockfile: `pnpm-lock.yaml`, `package-lock.json`, `yarn.lock`, or `bun.lockb`.
- Lint with the project's existing lint command. For a new setup, use ESLint 9 flat config (`eslint.config.js`) with neostandard. Do not switch linters in an existing project.
- Test with the project's runner. If none exists, use the built-in `node:test` runner.
- Always await or otherwise handle promises; never leave floating promises.
- Throw `Error` objects, not strings or plain objects.
- Keep imports and formatting consistent with the surrounding files.

## Verification

- Run the single affected test file first, then the full suite.
- Run `tsc --noEmit`, or the project's `typecheck` script, before reporting done.
- Run the lint command and fix what it reports within scope.

## Never

- Add a dependency without asking the user first.
- Change tsconfig strictness.
- Disable lint rules with blanket comments.
- Commit or publish.
- Run `npm publish`, `pnpm publish`, or `yarn npm publish`.
- Pipe remote scripts into a shell, such as `curl ... | sh`.

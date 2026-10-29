# <Project name>

<One-sentence description of what this project does.>

## Stack
- Language: <e.g. TypeScript 5.x, Python 3.12, Go 1.22>
- Framework: <e.g. Next.js 15, FastAPI, none>
- Package manager: <e.g. pnpm, uv, go modules>
- Runtime / target: <e.g. Node 22, Linux container, AWS Lambda>

## Commands
Run these exactly. Use the narrowest command that verifies the change first, then the full suite before a PR.
- Install: `<install command>`
- Test (single file): `<focused test command, e.g. pnpm vitest run path/to/file.test.ts>`
- Test (full): `<full test command>`
- Lint: `<lint command>`
- Type check: `<typecheck command, or "none">`
- Build: `<build command>`
- Dev server: `<dev command, or "none">`

## Layout
- `<path>/` - <what lives here>
- `<path>/` - <what lives here>
- Do not edit: `<generated paths, vendored code, lockfiles>`

## Conventions
- <Naming, error handling, logging, or style rules that differ from defaults>
- <Test location and naming, e.g. tests live next to source as `*.test.ts`>

## Git
- Default branch: `<main or master>`
- Branches and commits follow the `git-workflow` skill.
- Protected: never push to `<default branch>` directly.

## Deploy
- Deploy command: `<script or command, or "none">`
- Target: `<environment name, e.g. staging, production>`
- Deploys always require explicit user approval. Agents must never run this command on their own.
- Pre-deploy checks: `<commands that must pass first>`
- Rollback: `<command or procedure, or "none documented">`

## Secrets
- Never read or write `.env*`, credentials, keys, or `service.json`.
- Required env var names (values are never stored here): `<NAME_1>`, `<NAME_2>`

## Known gotchas
- <Anything that wasted time before, e.g. "codegen must run before tests">

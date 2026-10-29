---
name: run-tests
description: Discover the project test commands from AGENTS.md, run the narrowest test first then the full suite, and report commands and pass/fail counts. Use before reporting back on any code change.
---

# Run tests

## Steps

1. Read the project `AGENTS.md` for the documented test commands. If it is absent, look for the project's standard manifest scripts (for example `package.json`, `Makefile`, `pyproject.toml`).
2. Run the narrowest test that covers the change first (a single file or test name).
3. If it passes, run the full suite.
4. If a test fails, read the failure, fix it within scope, and rerun.

## Rules

- Never change tests to make them pass unless the task requires it.
- If a fix attempt fails, retry once. After two failed attempts, stop and report the exact error so the work can be escalated.
- Do not skip, disable, or delete tests.
- Leave all changes uncommitted.

## Report

- The exact command run for each step.
- Pass/fail counts for the narrow test and the full suite.
- The full error output for anything that failed.

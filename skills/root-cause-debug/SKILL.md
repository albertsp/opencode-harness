---
name: root-cause-debug
description: Diagnose a failure by reading the error and prior diff, testing hypotheses, and fixing the root cause with the smallest change. Use when a normal fix attempt has failed.
---

# Root-cause debugging

## Steps

1. Read the full error output, the previous attempt's diff, and the relevant code.
2. Form a short list of hypotheses for what could cause the failure.
3. Test each hypothesis with the narrowest command or reproduction, and discard the ones that fail.
4. Keep going until you find the root cause, not the symptom.
5. Apply the smallest fix that addresses the root cause.
6. Re-run the relevant tests.

## Rules

- Do not patch symptoms or mask errors.
- Do not refactor unrelated code.
- If the root cause is architectural and needs a decision, stop and describe the options instead of changing the design.
- Leave all changes uncommitted. Never commit, push, reset, or delete work.

## Report

- Root cause, with the evidence that confirmed it.
- Fix applied, with file paths and line numbers.
- Rationale, in exactly two sentences.
- Tests run and their results.

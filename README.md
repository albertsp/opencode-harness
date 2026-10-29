# OpenCode Development Harness

A multi-agent setup for OpenCode. One primary **orchestrator** plans work and asks for approval at each checkpoint. Specialized **subagents** do the exploration, implementation, escalation, and review. Models are chosen to fit the OpenCode Go plan: cheap models do most of the work, and stronger models are used only when needed.

## Layout

```
opencode.json                     Orchestrator config, watcher exclusions, task permissions
.opencodeignore                   Heavy directories and build artifacts to keep out of context
.gitignore                        Keeps credentials and local state out of git
agents/
  researcher.md                   Read-only research, paid-cheap model
  researcher_lite.md              Read-only simple lookups, free model
  coder.md                        Standard implementation, full access
  coder_lite.md                   Low-risk implementation only, free model
  deep_fixer.md                   Escalation for persistent bugs, strong model
  reviewer.md                     Read-only audit, returns [APPROVED] or [CHANGES REQUESTED]
.opencode/
  prompts/orchestrator.txt        Orchestrator system prompt (workflow, checkpoints, tier rules)
```

`service.json` holds a credential. It is git-ignored and must stay out of any shared repository.

## Agents and models

| Agent | Mode | Model | Access |
|---|---|---|---|
| `orchestrator` | primary | `opencode-go/claude-haiku-5-5` | No edits. Bash: `ask` by default; `ls`, `git status/diff/log` allowed. May only invoke the six subagents below. |
| `researcher` | subagent | `opencode-go/mimo-v2.6-flash` | Read-only. Bash limited to `grep`, `find` (no `-exec`/`-delete`), `cat`. |
| `researcher_lite` | subagent | `opencode-go/beam-free` | Same as `researcher`. For single-question lookups. |
| `coder` | subagent | `opencode-go/deepseek-v4.1-flash` | Edit and bash allowed. No push, reset, clean, or `rm -rf`. |
| `coder_lite` | subagent | `opencode-go/iceman-contributor-free` | Same as `coder`. Low-risk tasks only. |
| `deep_fixer` | subagent | `opencode-go/deepseek-v4-pro` | Same as `coder`. Called only after repeated failure or for architectural blockers. |
| `reviewer` | subagent | `opencode-go/glm-5.3-flash` | Read-only. Bash limited to `git diff`, `git status`, `git log`. |

## Workflow

1. **Intake.** The orchestrator restates the goal and asks about any ambiguity.
2. **Research.** `researcher_lite` or `researcher` gathers context with file paths and line numbers.
3. **Plan checkpoint.** Numbered atomic steps, each with files, agent and tier, and risk. Architectural choices go to you.
4. **Implementation.** One step at a time. Before each step the orchestrator states the agent and tier and waits for confirmation.
5. **Diff checkpoint.** You review the `git diff` after each step before the next one starts.
6. **Validation.** `reviewer` audits the full change set. `[CHANGES REQUESTED]` leads to a fix plan that needs your approval.
7. **Close.** Summary of changes, tests, and open risks. Nothing is committed or pushed unless you ask.

### Model tier rules

Free agents (`coder_lite`, `researcher_lite`) are used only when all of these hold:
- One file, or only docs or tests.
- No auth, permissions, personal data, migrations, secrets, or deploy config.
- No public API change.
- Under about 50 changed lines, easy to revert.
- No architectural decision.

If a free agent fails (rate limit, timeout, empty or incoherent output), the orchestrator retries once with the paid equivalent and tells you.

Every change is reviewed by `reviewer`, including free-tier changes.

`deep_fixer` is used only after `coder` has failed the same step twice, or when the cause is unclear or architectural. The orchestrator asks before escalating.

## Configuration notes

- **Agent discovery.** Subagents live in `agents/` (the documented global location, `~/.config/opencode/agents/`). Verify with `opencode debug agents`. Putting them under `.opencode/agents/` did not load them.
- **Prompt file.** `opencode.json` loads the orchestrator prompt via `{file:./.opencode/prompts/orchestrator.txt}`.
- **Delegation.** `permission.task` denies all subagents except the six listed. Denied agents are hidden from the orchestrator's tool list.
- **Reasoning levels.** Not configured. Some models support low/high/max reasoning, but the config key for OpenCode Go has not been verified.
- **Step limits.** Each agent has a `steps` cap to bound cost per run.

## Known limits

- `.opencodeignore` is not documented by OpenCode. Excluding heavy directories from the file watcher relies on `watcher.ignore` in `opencode.json`. Treat `.opencodeignore` as a convention, not a guarantee.
- Bash permission patterns match command text. `grep *` also matches `grep x; rm file`. The prompts forbid chaining, but nothing enforces that. Only the orchestrator's `ask` default is a hard gate.
- Free models (`beam-free`, `iceman-contributor-free`) have unknown quality, rate limits, and data-use policies. Check the policy before sending private code to them.
- Budget tracking is advisory. The config cannot read your 20%-per-5-hours quota, so the orchestrator only counts its own delegations.
- The agents have not yet been exercised end to end. Start with a small, low-risk change to validate the loop.

## Testing the harness

1. Start `opencode` in this directory and select `orchestrator`.
2. Ask for a small documentation change in a scratch project.
3. Confirm that the orchestrator asks for a plan checkpoint before delegating.
4. Confirm that `coder_lite` receives the task only after you approve it.
5. Confirm that `reviewer` returns `[APPROVED]` or `[CHANGES REQUESTED]`.

## Maintenance

- Model IDs come from `opencode models`. Check them when prices or availability change.
- After editing an agent or the orchestrator prompt, run `opencode debug config` to confirm it loads.

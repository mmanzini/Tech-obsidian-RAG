# Claude Code /goal — Autonomous Multi-Turn Completion

**Source:** [Keep Claude working toward a goal](https://code.claude.com/docs/en/goal)

---

## Summary

The `/goal` command sets a completion condition and keeps Claude working across turns automatically until a separate evaluator model confirms the condition is met. It is the session-scoped, condition-driven approach to autonomous work — distinct from `/loop` (time-interval repetition) and Stop hooks (custom evaluation logic). Requires Claude Code v2.1.139 or later.

## How /goal Works

After each turn, a small fast model (default: Haiku) checks whether the condition holds. A "no" sends Claude back for another turn with the evaluator's reason as guidance. A "yes" clears the goal and records an achieved entry. The evaluator runs on the configured provider (source: 2026-05-25-Keep Claude working toward a goal.md).

**Key design property:** completion is decided by a fresh model, not the one doing the work. This prevents the working model from self-declaring success prematurely. Evaluation tokens are negligible compared to main-turn spend (source: 2026-05-25-Keep Claude working toward a goal.md).

## Comparison to Other Autonomous Workflow Approaches

| Approach | Next turn starts when | Stops when |
|---|---|---|
| `/goal` | Previous turn finishes | A model confirms the condition is met |
| `/loop` | A time interval elapses | You stop it, or Claude decides the work is done |
| Stop hook | Previous turn finishes | Your own script or prompt decides |

`/goal` and Stop hooks both fire after every turn. `/goal` is a session-scoped shortcut (active for the current session only). A Stop hook lives in settings files, applies to every session in scope, and can use scripts for deterministic checks or prompts for model-evaluated ones (source: 2026-05-25-Keep Claude working toward a goal.md).

**Auto mode complement:** auto mode approves tool calls within a single turn without prompting; `/goal` removes per-turn prompts. The two compose: auto mode for unattended tool calls, `/goal` for unattended turn-to-turn continuation (source: 2026-05-25-Keep Claude working toward a goal.md).

## Using /goal

**Set a goal:**
```text
/goal all tests in test/auth pass and the lint step is clean
```
Setting a goal starts a turn immediately with the condition as the directive. A `◎ /goal active` indicator shows while the goal is running. One goal can be active per session; setting a new one replaces the current one (source: 2026-05-25-Keep Claude working toward a goal.md).

**Check status:** Run `/goal` with no arguments to see condition, elapsed time, turn count, token spend, and the evaluator's most recent reason.

**Clear early:** Run `/goal clear` (aliases: `stop`, `off`, `reset`, `none`, `cancel`).

**Resume:** A goal still active when a session ends is restored on `--resume` or `--continue`. Turn count, timer, and token baseline reset on resume (source: 2026-05-25-Keep Claude working toward a goal.md).

**Non-interactive (headless):**
```bash
claude -p "/goal CHANGELOG.md has an entry for every PR merged this week"
```

## Writing Effective Conditions

The evaluator judges against what Claude has **surfaced in the conversation** — it does not run commands or read files independently. Write conditions that Claude's own output can demonstrate (source: 2026-05-25-Keep Claude working toward a goal.md):

A good condition has:
- **One measurable end state**: a test result, build exit code, file count, or empty queue
- **A stated check**: how Claude should prove it (`npm test exits 0`, `git status is clean`)
- **Constraints that matter**: what must not change on the way (`no other test file is modified`)

To bound runtime, include a turn or time clause: `or stop after 20 turns`. Claude reports progress against this clause and the evaluator judges it from the conversation (source: 2026-05-25-Keep Claude working toward a goal.md).

Condition max length: 4,000 characters.

## Requirements

`/goal` runs only in workspaces where the trust dialog has been accepted (it is part of the hooks system). Unavailable when `disableAllHooks` is set at any settings level, or when `allowManagedHooksOnly` is set in managed settings. The command explains why rather than silently failing (source: 2026-05-25-Keep Claude working toward a goal.md).

## Use Cases

Best suited for substantial work with a **verifiable end state**:
- Migrating a module to a new API until every call site compiles and tests pass
- Implementing a design doc until all acceptance criteria hold
- Splitting a large file into focused modules until each is under a size budget
- Working through a labeled issue backlog until the queue is empty

(source: 2026-05-25-Keep Claude working toward a goal.md)

## Key Takeaways

- `/goal` is a session-scoped shortcut over the Stop hook system — use a Stop hook when you need the logic to persist across sessions
- The separate evaluator model prevents premature self-declared completion
- Conditions must be demonstrable from Claude's conversation output — the evaluator cannot run commands or read files independently
- Pair with auto mode for fully unattended long-horizon work
- Add a `or stop after N turns` clause to prevent runaway sessions on hard or mis-specified goals

## Related

- [[claude-code-session-management|Claude Code Session Management and 1M Context]] — session continuation options that complement /goal for long-running work
- [[claude-code-routines|Claude Code Routines]] — scheduled autonomous tasks that run independent of any open session
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — hooks as the foundation that /goal is built on
- [[claude-code-agent-view|Claude Code Agent View — Managing Multiple Background Sessions]] — managing multiple autonomous sessions in parallel

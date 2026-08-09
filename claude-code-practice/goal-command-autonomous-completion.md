---
type: synthesis
title: /goal — Condition-Based Autonomous Completion in Claude Code
description: The `/goal` command sets a completion condition and Claude keeps working toward it without the user prompting each step.
bundle: ai-engineering
topic: claude-code-practice
tags:
- claude-code
- long-running-agents
- agent-workflows
- skills-and-hooks
resource: https://code.claude.com/docs/en/goal
sources:
- id: goal
  resource: https://code.claude.com/docs/en/goal
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-25T00:17:36Z'
status: stable
related:
- ai-engineering/agent-infrastructure/agent-view-multi-agent-management.md
- ai-engineering/claude-code-practice/claude-code-routines.md
- ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
- ai-engineering/claude-code-practice/subagents-in-claude-code.md
---

# /goal — Condition-Based Autonomous Completion in Claude Code

**Source:** [Keep Claude working toward a goal](https://code.claude.com/docs/en/goal)

---

## Summary

The `/goal` command sets a completion condition and Claude keeps working toward it without the user prompting each step. After each turn, a separate small fast model (Haiku by default) checks whether the condition holds. If not, Claude starts another turn instead of returning control. The goal clears automatically once the condition is met. This creates a session-scoped autonomous loop with a verifiable end state rather than open-ended iteration.

## How It Differs from Other Autonomous Workflows

Three approaches keep the current session running between prompts (source: goal-command-autonomous-completion.md):

| Approach | Next turn starts when | Stops when |
|---|---|---|
| `/goal` | Previous turn finishes | A model confirms the condition is met |
| `/loop` | A time interval elapses | You stop it, or Claude decides the work is done |
| Stop hook | Previous turn finishes | Your own script or prompt decides |

`/goal` and a Stop hook both fire after every turn. `/goal` is a session-scoped shortcut: type a condition and it's active for the current session only. A Stop hook lives in your settings file and applies to every session in its scope — it can run a script for deterministic checks or a prompt for model-evaluated ones (source: goal-command-autonomous-completion.md).

**Relationship to auto mode**: auto mode approves tool calls within a single turn but doesn't start a new one. `/goal` adds a separate evaluator that checks your condition after every turn. The two are complementary: auto mode removes per-tool prompts, `/goal` removes per-turn prompts (source: goal-command-autonomous-completion.md).

Requires Claude Code v2.1.139 or later (source: goal-command-autonomous-completion.md).

## Setting and Writing Effective Goals

Run `/goal` followed by the condition. Setting a goal starts a turn immediately with the condition as the directive. A `◎ /goal active` indicator shows how long the goal has been running. After each turn, the evaluator returns a short reason explaining why the condition is or isn't met (source: goal-command-autonomous-completion.md).

Good use cases: migrating a module until every call site compiles and tests pass; implementing a design doc until all acceptance criteria hold; splitting a large file until each is under a size budget; working through an issue backlog until the queue is empty (source: goal-command-autonomous-completion.md).

**Effective condition structure** (source: goal-command-autonomous-completion.md):

- **One measurable end state**: a test result, build exit code, file count, empty queue
- **A stated check**: how Claude should prove it — "`npm test` exits 0" or "`git status` is clean"
- **Constraints that matter**: what must not change on the way there — "no other test file is modified"
- Conditions can be up to 4,000 characters
- Include a turn or time clause to bound duration: "or stop after 20 turns"

The evaluator judges your condition against what Claude has surfaced in the conversation — it doesn't run commands or read files independently. Write the condition as something Claude's own output can demonstrate (source: goal-command-autonomous-completion.md).

## Status, Clearing, and Resuming

- **`/goal`** (no args): show current condition, duration, turn count, token spend, and the evaluator's most recent reason
- **`/goal clear`** (aliases: `stop`, `off`, `reset`, `none`, `cancel`): remove active goal before condition is met
- Running `/clear` to start a new conversation also removes any active goal
- A goal still active when a session ended is **restored on resume** (`--resume` or `--continue`) — condition carries over, but turn count, timer, and token spend reset
- An already achieved or cleared goal is not restored on resume (source: goal-command-autonomous-completion.md)

## Non-Interactive and Headless Use

`/goal` works in non-interactive mode, in the desktop app, and through Remote Control. Setting a goal with `-p` runs the loop to completion in a single invocation:

```shellscript
claude -p "/goal CHANGELOG.md has an entry for every PR merged this week"
```

Interrupt with `Ctrl+C` to stop a non-interactive goal before the condition is met (source: goal-command-autonomous-completion.md).

## How Evaluation Works

`/goal` is a wrapper around a session-scoped prompt-based Stop hook. Each time Claude finishes a turn, the condition and the conversation so far are sent to your configured small fast model (defaults to Haiku). The model returns a yes-or-no decision and a short reason:

- **No**: Claude keeps working; the reason is included as guidance for the next turn
- **Yes**: goal clears; an achieved entry is recorded in the transcript

Evaluation tokens are billed on the small fast model and are typically negligible compared to main-turn spend (source: goal-command-autonomous-completion.md).

## Requirements

`/goal` runs only in workspaces where you have accepted the trust dialog, because the evaluator is part of the hooks system. `/goal` is also unavailable when `disableAllHooks` is set at any settings level or when `allowManagedHooksOnly` is set in managed settings (source: goal-command-autonomous-completion.md).

## Key Takeaways

- `/goal` is a session-scoped autonomous loop with a separate evaluator (Haiku by default) verifying completion after every turn
- Write conditions that Claude's own output can demonstrate — not conditions requiring the evaluator to run commands independently
- Combine with auto mode for fully unattended operation (auto mode handles per-tool prompts; `/goal` handles per-turn prompts)
- Goals restore on session resume; cleared or achieved goals do not
- Works in headless/non-interactive mode for scripted autonomous pipelines

## Related

- [[agent-view-multi-agent-management|Agent View — Managing Multiple Claude Code Sessions]] — agent view as the complement for managing many simultaneous goal-driven sessions
- [[claude-code-routines|Claude Code Routines]] — scheduled tasks as an alternative for work that should run on a time basis rather than completion condition
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — the Stop hook mechanism that `/goal` is built on top of
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — delegation pattern that works alongside goal-driven sessions

---
type: synthesis
title: Verification Loops in Claude Code — Encoding Manual Checks as Skills
description: Anthropic's guide to verification loops — turn the manual checks you repeat after every agent change into skills (standalone, embedded, chained, or PR-wide) so Claude closes its own feedback loop.
bundle: ai-engineering
topic: harness-engineering
tags: [skills-and-hooks, claude-code, harness-engineering, agent-workflows]
source: Resources/web-clippings/2026-08-04-Building verification loops in Claude Code with skills.md
resource: https://claude.com/blog/building-verification-loops-in-claude-code-with-skills
timestamp: 2026-08-05T09:00:00Z
status: active
related:
  - ai-engineering/agent-infrastructure/graph-engineering-verification.md
  - ai-engineering/harness-engineering/skill-creator-evals.md
  - ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
  - ai-engineering/harness-engineering/steering-claude-code-instruction-methods.md
  - ai-engineering/ci-integrations/index.md
---

# Verification Loops in Claude Code — Encoding Manual Checks as Skills

**Source:** [Building verification loops in Claude Code with skills](https://claude.com/blog/building-verification-loops-in-claude-code-with-skills) (Anthropic, Claude Code blog)
**Author:** Delba de Oliveira, Claude Code team

---

## Summary

A verification loop is a repeating cycle where the agent checks its own work — tests, linters, or custom checks — and fixes what fails before moving on (source: 2026-08-04-Building verification loops in Claude Code with skills.md). Claude already verifies from deterministic signals (type checkers, linters, tests, runtime errors); everything you still check by hand is a candidate to be encoded as a skill, so every session applies the same checks automatically instead of relying on a human to remember them (source: 2026-08-04-Building verification loops in Claude Code with skills.md). The article's core taxonomy: standalone (invoked deliberately), embedded (fires inside a producing skill), chained (skills invoke each other end-to-end), and PR-wide (the chain becomes team infrastructure).

## Built-in verification loops

Claude Code ships with several verification mechanisms before you write anything custom (source: 2026-08-04-Building verification loops in Claude Code with skills.md):

- **/verify skill** — builds, runs, and observes changes in your application.
- **Toolchain** — Claude acts on error codes and warnings from any tool you provide; listing exact build/test commands in CLAUDE.md saves it inferring them.
- **Code Review (research preview)** — managed multi-agent review pass on PRs; close the loop by commenting `@claude` on a finding when GitHub Actions is configured.
- **GitHub Actions** — a job that invokes Claude with a verification skill runs the same checks on every push or PR.
- **Spec validation** — a skill verifying each change against a markdown spec in the repo.
- **Rubrics in Claude Managed Agents (beta)** — a separate grader agent verifies outcomes against a rubric; failures loop back for rework automatically.

## Writing your own

The trigger for a custom loop: you find yourself making the same small corrections every time Claude implements a feature. Write down everything you repeatedly do, in plain English, the way you'd hand it to a new teammate on day one (source: 2026-08-04-Building verification loops in Claude Code with skills.md). Checks don't have to be qualitative — "reject any migration that drops a column without a backfill step" is a deterministic project-specific rule no generic linter catches (source: 2026-08-04-Building verification loops in Claude Code with skills.md). The fastest path to a skill is the skill-creator plugin (`/skill-creator … Interview me about my workflow.`); the simplest is a hand-written `SKILL.md` in `.claude/skills/` with name, description, allowed-tools, and a body describing the check and the fix (source: 2026-08-04-Building verification loops in Claude Code with skills.md).

## The four invocation modes

| Mode | When it runs | Use for | Watch out |
|---|---|---|---|
| **Standalone** | You invoke it deliberately, after the artifact exists | Cross-cutting checks that don't apply every time: pre-commit security scan, accessibility audit, license headers | Each invocation is a turn you must remember; running it after every change means it has outgrown standalone |
| **Embedded** | Fires automatically inside the producing skill | Checks that belong to one specific workflow (e.g. "run eslint after scaffolding the component") | Only works on skills you can edit; built-in and plugin-managed skills are off-limits — chain instead |
| **Chained** | One skill calls another at its end | End-to-end verified handoffs; also how you add verification to skills you can't modify (wrapper skill) | Trades flexibility for automation; increases token spend — test before deploying broadly |
| **On every PR** | CI applies the chain to every change | Team infrastructure: everyone's changes pass the same gates regardless of author diligence | Hold off while the chain is in flux — every adjustment becomes team-visible |

(source: 2026-08-04-Building verification loops in Claude Code with skills.md)

Anthropic's Claude Code team chains in their day-to-day: `/code-review` hunts for bugs, `/simplify` cleans the diff, `/verify` confirms end-to-end behaviour, and a custom `/design` skill checks against a DESIGN.md when the change touches UI (source: 2026-08-04-Building verification loops in Claude Code with skills.md). Chaining turns a habit ("I always run /verify after /simplify") into a contract ("/simplify always runs /verify when it finishes").

## The process

1. Pick the manual follow-up you did most often this week.
2. Try the built-in `/verify` skill first.
3. Write the procedure in plain English.
4. Hand it to skill-creator, or drop the markdown in `.claude/skills/` yourself.
5. Invoke it on a new task and confirm the check runs; iterate.
6. Experiment with chaining for an end-to-end verification flow.

(source: 2026-08-04-Building verification loops in Claude Code with skills.md)

## Key Takeaways

- Whatever Claude can't infer from deterministic signals becomes your manual check — and every manual check qualifies for capture as a skill.
- Match the check to where it runs: standalone for cross-cutting, embedded for workflow-specific, chained for end-to-end, PR-wide for team enforcement.
- The escalation signal is frequency: running a standalone check after every change means embed or chain it; a solid chain earns promotion to every PR.
- Encoding checks moves verification from personal habit to contract to team infrastructure — the same skills and rubrics at each level.

## Related

- [[graph-engineering-verification]] · [Graph Engineering and Verification](../agent-infrastructure/graph-engineering-verification.md) — why verification decides whether multi-agent graphs work at all; builds directly on this article's skill types
- [[skill-creator-evals]] · [Improving Skill-Creator](../harness-engineering/skill-creator-evals.md) — eval authoring and benchmark mode for testing the verification skills themselves
- [[hooks-for-deterministic-cli-enforcement]] · [Hooks for Deterministic CLI Enforcement](../harness-engineering/hooks-for-deterministic-cli-enforcement.md) — the deterministic sibling: hooks for what must always happen, skills for judgment-based checks
- [[steering-claude-code-instruction-methods]] · [Steering Claude Code](../harness-engineering/steering-claude-code-instruction-methods.md) — where verification skills sit among the seven instruction methods

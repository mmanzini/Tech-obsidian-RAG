---
type: synthesis
title: Execution contracts for headless skill runs
description: "Patterns from making the Atlas dashboard-refresh pipeline run reliably as a scheduled headless Claude Code job: project hooks interfere with automation, and the fix is an inline \"execution contract\" in the skill itself rather than hook configuration (source: session-2026-06-11-1300.md)."
bundle: ai-engineering
topic: harness-engineering
tags: [skills-and-hooks, harness-engineering, claude-code, long-running-agents]
source: session capture 2026-06-11 (Dashboard AI News / dashboard-refresh work
resource:
timestamp: 2026-06-11T20:51:27Z
status: active
related:
  - ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/harness-engineering/excalidraw-plugin-external-edit-gotcha.md
---

# Execution contracts for headless skill runs

**Source:** session capture 2026-06-11 (Dashboard AI News / dashboard-refresh work)
**Author:** Max (working notes, auto-captured)

---

## Summary

Patterns from making the Atlas dashboard-refresh pipeline run reliably as a scheduled headless Claude Code job: project hooks interfere with automation, and the fix is an inline "execution contract" in the skill itself rather than hook configuration (source: session-2026-06-11-1300.md).

## The execution-contract pattern

A vault's SessionStart and Stop hooks (e.g. the task-board reconcile reminder) hijack headless runs — the agent answers the hook instead of the job. The fix is an explicit contract in `SKILL.md`: "do NOT touch the Tasks board, finish all resolvers". Solve interference via inline execution contract, not hook config (source: session-2026-06-11-1300.md). Scheduled-agent prompts need an absolute cwd plus an explicit file path to the `SKILL.md` (no `/` slash commands available headless), and the execution contract repeated inline to prevent short-circuiting (source: session-2026-06-11-1300.md).

## Dashboard AI News architecture notes

- **Dedup ledger pattern:** seen URLs tracked in the data file body, invisible to the renderer; 7-day window max, weekly digest always included, world-reaction recap; entity watchlist expanded from 20 to 40+ (agents, hardware, policy, funding, research/safety) (source: session-2026-06-11-1300.md).
- **Trigger:** refresh initially ran via the `/dashboard-refresh` slash command — a personal command in `~/.claude/commands/`, not vault-committed (source: session-2026-06-11-1300.md). Earlier the same day Max chose to keep the refresh manual and uncoupled from the consolidate auto-chain (source: 2026-06-11-dashboard-refresh-stays-manual.md), then superseded that decision: the refresh moves to a **daily remote scheduled agent**. The agreed prompt pattern: absolute working directory, direct path to `Skills/dashboard-refresh/SKILL.md` (no slash commands headless), the execution contract replicated inline, an explicit prohibition on touching the Tasks board, and a closing commit + push to `main` (source: 2026-06-11-dashboard-refresh-scheduled-agent.md). The refresh stays out of the consolidate → refine → reflect chain in both regimes — automation is by schedule, not by chaining.
- **Git rule:** remote agents push to `main`, inherited from session convention (source: session-2026-06-11-1300.md).

## Codex compatibility: hooks as encoded instructions

Claude Code's `.claude/settings.json` wires SessionStart, Stop, and SessionEnd hooks, but Codex does not execute this file. The solution: the CLAUDE.md/AGENTS.md encodes those hook behaviours as **mandatory encoded instructions** — the same guarantees (snapshot recall at session start, board reconcile at stop, capture note at session end) are written directly into the program file as if they were hard rules (source: 2026-06-11-codex-hooks-encoded.md).

This is the CLAUDE.md-as-execution-contract pattern applied at the whole-vault level: when the runtime can't run the hooks, the instructions absorb them. The tradeoff is that encoded behaviours must be kept in sync with the hook implementations manually; divergence creates two codepaths with different guarantees.

## Key Takeaways

- Hooks are session-scoped UX; headless jobs need their guarantees written into the skill text itself.
- Repeat the contract inline in the scheduled prompt — a single statement gets short-circuited.
- Keep machine-facing state (dedup ledgers) inside data files but outside the rendered surface.

## Related

- [[hooks-for-deterministic-cli-enforcement]] — hooks as enforcement; this article covers their failure mode in headless runs
- [[skill-issue-harness-engineering]] — the harness toolbox these contracts live in
- [[excalidraw-plugin-external-edit-gotcha]] — another agents-writing-into-Obsidian workflow rule (close tabs before external rewrites)

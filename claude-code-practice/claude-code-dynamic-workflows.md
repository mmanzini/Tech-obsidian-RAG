---
type: synthesis
title: Claude Code Dynamic Workflows — Script-Driven Subagent Orchestration at Scale
description: Dynamic workflows move the orchestration plan out of Claude's context and into a rerunnable JavaScript script that a runtime executes in the background — dozens to hundreds of subagents per run, with intermediate results in script variables and only the final answer landing in context.
bundle: ai-engineering
topic: claude-code-practice
tags:
- claude-code
- multi-agent
- agent-workflows
- long-running-agents
- context-engineering
resource: https://code.claude.com/docs/en/workflows
sources:
- id: 2026-07-01-orchestrate-subagents-at-scale-with-dynamic-workflows
  resource: Resources/web-clippings/2026-07-01-Orchestrate subagents at scale with dynamic workflows.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-07-05T07:20:00Z'
status: stable
related:
- ai-engineering/claude-code-practice/subagents-in-claude-code.md
- ai-engineering/claude-code-practice/claude-code-agent-view.md
- ai-engineering/agent-architecture/multi-agent-coordination-patterns.md
---

# Claude Code Dynamic Workflows — Script-Driven Subagent Orchestration at Scale

**Source:** [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows) (Claude Code docs)

---

## Summary

A dynamic workflow is a JavaScript script that orchestrates subagents at scale: Claude writes the script for the task you describe, and a runtime executes it in the background while the session stays responsive (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md). Reach for one when a task needs more agents than a single conversation can coordinate, or when the orchestration itself should be a readable, rerunnable artifact. Canonical use cases: codebase-wide bug sweeps and audits, 500-file migrations, research questions needing sources cross-checked against each other, and drafting a hard plan from several independent angles before committing (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md). Requires Claude Code v2.1.154+ on paid plans, API access, or Bedrock/Vertex/Foundry.

## The plan moves into code

The key distinction from subagents, skills, and agent teams is *who holds the plan*. With those, Claude is the orchestrator — deciding turn by turn what to spawn, with every intermediate result landing in a context window. A workflow script holds the loop, the branching, and the intermediate results itself (in script variables), so Claude's context holds only the final answer (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md). The comparison table:

| | Subagents | Skills | Agent teams | Workflows |
|---|---|---|---|---|
| Who decides what runs next | Claude, turn by turn | Claude, following the prompt | Lead agent, turn by turn | The script |
| Intermediate results live in | Context window | Context window | Shared task list | Script variables |
| What's repeatable | Worker definition | Instructions | Team definition | The orchestration itself |
| Scale | Few tasks per turn | Same | Handful of peers | Dozens to hundreds of agents per run |

Moving the plan into code also enables repeatable *quality patterns*, not just more agents: independent agents adversarially reviewing each other's findings before reporting, or drafting a plan from several angles and weighing them (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).

## Triggering and writing workflows

- **Bundled workflow:** `/deep-research` fans out web searches across angles, fetches and cross-checks sources, votes on each claim, and returns a cited report with refuted claims filtered out (unverifiable claims are listed as unverified as of v2.1.196) (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).
- **Per-prompt opt-in:** include the keyword `ultracode` (or just ask for "a workflow" in natural language) and Claude writes a workflow script instead of working turn by turn (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).
- **Session-wide:** `/effort ultracode` combines `xhigh` reasoning effort with automatic workflow orchestration — Claude plans a workflow for every substantive task, potentially several per request (understand → change → verify) (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).
- **Existing orchestrators port over:** point Claude at a folder of subagent prompts or a fan-out skill and ask for a workflow that does the same thing (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).

## Rerunnable artifacts

Every run writes its script to a file under `~/.claude/projects/`; you can read it, diff it against a previous run's script, or edit and relaunch it (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md). Save a good run via `/workflows` → `s` to either `.claude/workflows/` (project-shared) or `~/.claude/workflows/` (personal); it then runs as a `/<name>` command. Saved workflows accept structured input through an `args` global — a research question, target paths, or a config object — without editing the script per run (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).

## Runtime behaviour and limits

The runtime executes the script in an isolated environment: no mid-run user input, no direct filesystem or shell access from the script itself (agents do the reading/writing; the script coordinates), up to 16 concurrent agents, and 1,000 agents total per run to prevent runaway loops (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md). Spawned subagents always run in `acceptEdits` mode and inherit the tool allowlist regardless of session mode — pre-populate the allowlist before long runs to avoid mid-run prompts. Runs are pausable/resumable within the same session (completed agents return cached results); monitoring happens in `/workflows` (per-phase agent counts, token totals, drill-down into any agent) or the task panel (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md). Cost scales with agent count: gauge spend on a small slice first, and route stages to smaller models where possible (source: 2026-07-01-Orchestrate subagents at scale with dynamic workflows.md).

## Key Takeaways

- Workflows move orchestration from Claude's context into a rerunnable script: the script holds loop, branching, and intermediate results; context gets only the final answer.
- Choose by who should hold the plan: subagents/skills for a few delegated tasks, agent teams for long-running peers, workflows for dozens-to-hundreds of agents with codified orchestration.
- The script is a first-class artifact — readable, diffable, editable, saveable as a `/command` with `args` input; orchestration becomes version-controllable process knowledge.
- Quality patterns (adversarial cross-review, multi-angle drafting) are the deeper win over raw scale.
- Hard limits: 16 concurrent agents, 1,000 per run, no mid-run user input; subagents run in `acceptEdits` with your allowlist — prepare it before launching.

## Related

- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] · [Subagents in Claude Code](../claude-code-practice/subagents-in-claude-code.md) — the worker primitive workflows orchestrate; workflows are the answer when subagent counts outgrow one conversation
- [[claude-code-agent-view|Claude Code Agent View]] · [Claude Code Agent View](../claude-code-practice/claude-code-agent-view.md) — session-level dashboard for background work; workflows add script-level orchestration below it
- [[multi-agent-coordination-patterns|Multi-Agent Coordination Patterns]] · [Multi-Agent Coordination Patterns](../agent-architecture/multi-agent-coordination-patterns.md) — the canonical coordination patterns (generator-verifier, orchestrator-subagent) that workflow scripts codify

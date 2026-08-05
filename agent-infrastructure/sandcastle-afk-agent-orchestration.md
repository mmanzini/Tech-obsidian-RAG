---
type: synthesis
title: Sandcastle — AFK Software Factory
description: Sandcastle is a TypeScript library for orchestrating AI coding agents inside isolated sandboxes.
bundle: ai-engineering
topic: agent-infrastructure
tags: [harness-engineering, long-running-agents, agent-workflows, multi-agent, claude-code]
source: https://www.youtube.com/watch?v=E5-QK3CDVQM
resource: https://www.youtube.com/watch?v=E5-QK3CDVQM
timestamp: 2026-05-03T12:59:31Z
status: active
related:
  - ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
  - ai-engineering/claude-code-practice/claude-code-desktop-parallel.md
  - ai-engineering/claude-code-practice/subagents-in-claude-code.md
  - ai-engineering/harness-engineering/effective-harnesses-long-running.md
---

# Sandcastle — AFK Software Factory

**Source:** [I Open-Sourced My Own AFK Software Factory (YouTube)](https://www.youtube.com/watch?v=E5-QK3CDVQM)
**Author:** Matt Pocock
**Date:** 2026-04-02

---

## Summary

Sandcastle is a TypeScript library for orchestrating AI coding agents inside isolated sandboxes. It exposes a single composable primitive — `sandcastle.run(agent, sandbox, prompt)` — and ships a workflow template that implements a fully autonomous planner → implementer → reviewer → merger pipeline. The goal is an AFK software factory: agents pick up GitHub Issues, implement them in parallel Docker branches, review each other's work, and merge to main, all without human permission interruptions.

## Core Primitive

```ts
sandcastle.run(agent, sandbox, prompt)
```

This single call is the composable unit. Complex multi-agent workflows are assembled by composing it (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## Setup

```bash
npm install @aihero/sandcastle
npx sandcastle init
```

The init wizard picks: agent (Claude Code), sandbox (Docker), backlog manager (GitHub Issues), and workflow template (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## Dockerfile

The generated Dockerfile installs system dependencies, GitHub CLI, and Claude Code. A `.castle/` directory holds workflow configuration. Required environment variables: `ANTHROPIC_API_KEY` and a GitHub token (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## Default Workflow Template

### 1. Planner Agent
Grabs open GitHub Issues tagged with the Sandcastle label, produces a structured plan as JSON inside `<plan>` tags, then spawns one implementer agent per issue (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

### 2. Implementer Agent
Each implementer picks up its assigned issue and implements the solution inside its own isolated Docker container on a dedicated branch (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

### 3. Reviewer Agent
If the implementer produced more than one commit, a reviewer agent inspects the implementation before it proceeds to merge (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

### 4. Merger Agent
A senior merger agent handles merge conflicts and integrates all parallel branches into main (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## Key Feature — `!bash_command` Prompt Injection

Prompt templates support `!<bash_command>` syntax: at resolve-time the shell command executes and its output is inserted inline into the prompt. Example: `!git diff` inserts the actual diff text. This gives agents live environmental context without manual wiring (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## Cross-Model Adversarial Review

`sandcastle.codex` supports adversarial multi-model workflows: Claude and Codex implement the same issue independently, and a reviewer agent picks the superior result. This exploits model diversity for quality without requiring a single best model (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## AFK Design Philosophy

The defining constraint is zero permission interruptions. Sandboxing (Docker isolation) prevents destructive actions that would otherwise require human approval, enabling genuine unattended operation (source: transcript-i-open-sourced-my-own-afk-software-factory.md).

## Key Takeaways

- A single composable primitive (`sandcastle.run`) is sufficient to assemble arbitrarily complex multi-agent pipelines.
- Sandbox isolation (Docker) is what makes AFK operation safe — it removes the class of destructive-action risks that force permission prompts.
- `!bash_command` prompt injection is a lightweight alternative to MCP for injecting live environmental context into agent prompts.
- Adversarial cross-model review (Claude vs. Codex on the same task) is a viable quality strategy when model capabilities overlap.
- GitHub Issues as a backlog manager gives human oversight via a familiar interface without building custom tooling.

## Related

- [[hooks-for-deterministic-cli-enforcement]] — complementary approach: PreToolUse hooks enforce CLI rules deterministically, removing a different class of permission prompts
- [[claude-code-desktop-parallel]] — parallel Claude Code sessions at the IDE level; Sandcastle is the harness-level equivalent
- [[subagents-in-claude-code]] — context firewalls and invocation patterns for sub-agents that Sandcastle's pipeline applies at scale
- [[effective-harnesses-long-running]] — two-agent initializer/coder pattern; Sandcastle generalises this to N parallel agents

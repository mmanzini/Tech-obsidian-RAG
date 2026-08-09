---
type: synthesis
title: Kiro Hooks
description: Agent hooks in Kiro are event-driven automation triggers that execute predefined agent actions (prompts or shell commands) when specific IDE events occur.
bundle: ai-engineering
topic: ai-dev-tools
tags:
- skills-and-hooks
- harness-engineering
- agent-workflows
- claude-code
resource: https://kiro.dev/docs/hooks/
sources:
- id: hooks
  resource: https://kiro.dev/docs/hooks/
generated:
  by: claude-code/atlas-consolidate
  at: '2026-04-29T21:24:51Z'
status: stable
related: []
---

# Kiro Hooks

**Source:** [Kiro Docs](https://kiro.dev/docs/hooks/)

---

## Summary

Agent hooks in Kiro are event-driven automation triggers that execute predefined agent actions (prompts or shell commands) when specific IDE events occur. They eliminate manual requests for routine tasks and enforce consistency across a codebase. Conceptually similar to git hooks, but tied to agent lifecycle and IDE events.

## Trigger Events

- File save, create, delete
- User prompt submission and agent turn completion
- Before/after tool invocations
- Before/after spec task execution
- Manual triggers on demand

## What Hooks Enable

- Maintain consistent code quality
- Prevent security vulnerabilities
- Reduce manual overhead
- Standardize team processes
- Faster development cycles

## How It Works

Two-step process:
1. **Event Detection** — system monitors for IDE events
2. **Automated Action** — when an event fires, an agent prompt or shell command runs

## Creating a Hook

Two methods:

### Ask Kiro to Create a Hook
1. Navigate to **Agent Hooks** in the Kiro panel → click **+**
2. Select **Ask Kiro to create a hook**
3. Describe the workflow in natural language
4. Review generated config and save

### Manually Create a Hook
Form fields:
- **Title** — short name
- **Description** — what it does
- **Event** — trigger type (File Save, Post Tool Use, Pre Task Execution, etc.)
- **Tool name** — for Pre/Post Tool Use hooks
- **File pattern** — for file event hooks
- **Action** — Ask Kiro (agent prompt) or Run Command (shell)
- **Instructions / Command** — the prompt or shell command

Also accessible via Command Palette: `Kiro: Open Kiro Hook UI`.

## Key Takeaways

- Hooks bridge the gap between **manual prompting** and **deterministic automation**
- Two action types: **agent prompt** (model invoked) or **shell command** (deterministic)
- Use natural language to generate hook configs, or fill the form by hand
- Common categories: code quality enforcement, security checks, standardization, lifecycle automation
- Conceptually parallel to Claude Code hooks — see [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] for how hooks fit into the broader harness picture (back-pressure, verification, integrations)

## See Also

- [[kiro-specs|Kiro Specs]] — hooks can fire on Pre/Post task execution within a spec
- [[kiro-steering|Kiro Steering]] — steering provides persistent context; hooks provide event-driven actions
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — broader treatment of hooks as a harness configuration surface

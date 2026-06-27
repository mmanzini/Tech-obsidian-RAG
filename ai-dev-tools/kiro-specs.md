---
type: synthesis
title: Kiro Specs
description: Specs are structured artifacts that formalize the development process for features and bug fixes.
bucket: ai-engineering
topic: ai-dev-tools
tags: [spec-driven-development, agent-workflows, claude-code, harness-engineering]
source: https://kiro.dev/docs/specs/
resource: https://kiro.dev/docs/specs/
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# Kiro Specs

**Source:** [Kiro Docs](https://kiro.dev/docs/specs/)

---

## Summary

Specs are structured artifacts that formalize the development process for features and bug fixes. They transform high-level ideas into detailed implementation plans with clear tracking. Each spec generates three files (`requirements.md`, `design.md`, `tasks.md`) following a three-phase workflow that closely mirrors [[research-plan-implement|Research Plan Implement (RPI)]].

## Core Structure

Every spec generates three files:

- **`requirements.md`** (or **`bugfix.md`**) — User stories with acceptance criteria, or bug analysis with current/expected/unchanged behavior
- **`design.md`** — Technical architecture, sequence diagrams, implementation considerations
- **`tasks.md`** — Detailed implementation plan with discrete trackable tasks

## Three-Phase Workflow

1. **Requirements / Bug Analysis** — Define what to build or fix
2. **Design** — System architecture, sequence diagrams, data flow, error handling, testing strategy
3. **Tasks** — Discrete, executable, trackable tasks with real-time status updates

Tasks can be run individually or all at once. The execution interface displays in-progress / completed status in real time.

## Two Spec Types

### Feature Specs
For new features and capabilities. Two workflow variants:
- **Requirements-First** — start from user stories
- **Design-First** — start from architecture

### Bugfix Specs
For systematically diagnosing and fixing bugs with surgical precision. Helps identify root causes, design fixes, and validate against regressions.

## When to Use Specs vs "Vibe"

**Use Specs when:**
- Building complex features requiring structured planning
- Fixing bugs where regressions are costly
- You need documentation for team collaboration
- Requirements or design need iteration

**Use Vibe when:**
- Quick exploratory coding
- Prototyping without clear goals

## Getting Started

1. Click `+` under **Specs** in the Kiro pane (or choose Spec from chat)
2. Choose Feature or Bug
3. Describe the feature/bug; choose Requirements-First or Design-First for features
4. Follow the workflow through each phase

## Key Takeaways

- Specs formalize the **requirements → design → tasks** flow as first-class IDE artifacts
- Each phase produces a tracked file; tasks have real-time status updates
- Bugfix Specs explicitly model **current vs expected vs unchanged** behavior to prevent regressions
- Closely parallels [[research-plan-implement|Research Plan Implement (RPI)]] — the same three-phase discipline, but built into the IDE rather than invoked via slash commands

## See Also

- [[research-plan-implement|Research Plan Implement (RPI)]] — the workflow pattern Specs implements at the IDE level
- [[kiro-hooks|Kiro Hooks]] — can fire Pre/Post task execution within a spec
- [[kiro-steering|Kiro Steering]] — provides persistent context that informs spec generation
- [[quick-dev|Quick Dev]] — alternative philosophy that minimizes phase boundaries

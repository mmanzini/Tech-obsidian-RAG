---
type: synthesis
title: SDD Overview
description: "SDD inverts the traditional development relationship: the spec — not the code — is the artifact that endures and is maintained under version control."
bundle: ai-engineering
topic: spec-driven-development
tags: [spec-driven-development, agent-workflows, knowledge-management]
source: synthesis — spec-driven-development corpus
resource:
timestamp: 2026-05-09T07:23:23Z
status: active
related:
  - ai-engineering/spec-driven-development/sdd-workflow.md
  - ai-engineering/spec-driven-development/sdd-roles-and-boundaries.md
  - ai-engineering/ai-dev-tools/kiro-specs.md
  - ai-engineering/agent-workflows/research-plan-implement.md
  - ai-engineering/rpi-methodology/vs-other-frameworks.md
---

# SDD Overview

**Source:** (synthesis — spec-driven-development corpus)

**Spec-Driven Development (SDD)** is a tool-agnostic standard for building software with AI agents. The core inversion: the **specification** is the durable, version-controlled artifact, and code becomes a regenerable output of the spec plus the current toolchain.

## Summary

SDD inverts the traditional development relationship: the spec — not the code — is the artifact that endures and is maintained under version control. It is designed to be adopted incrementally across four levels, from a simple one-page brief (Level 1) up to an org-wide standard with shared boundary conventions (Level 4), making it suitable for both solo developers and scaling organisations. The Always/Ask/Never boundary framework makes agent autonomy safe by making intent explicit and reviewable by any compliant agent.

## Why SDD

Vibe-coding with agents produces fast first drafts but fragile systems: intent lives in chat history, regressions are silent, and onboarding new agents (or humans) means re-explaining everything. SDD makes intent explicit and reviewable so that any compliant agent — today's or next year's — can pick up where the last one left off.

## Principles

1. **Spec is the source of truth.** Code is downstream; if they disagree, the spec wins (and gets updated if it was wrong).
2. **Tool agnosticism.** The standard prescribes artifacts and phases, not which IDE, model, or framework you use. A spec written for Kiro should be runnable by Claude Code, Cursor, or a human.
3. **Explicit boundaries.** Every spec declares what the AI may always do, must ask about, or must never do (see [[sdd-roles-and-boundaries]]).
4. **Phased flow, not waterfall.** Phases are checkpoints, not gates — you loop back when you learn something.
5. **Own your prompts and context.** Like [[twelve-factor-agents|Twelve-Factor Agents]], steering documents and spec fragments are committed to the repo, not hidden in tool state.

## Adoption Levels

SDD is designed to be adopted incrementally:

- **Level 1 — Brief only.** Write a one-page brief before any agent task. Cheapest possible win.
- **Level 2 — Spec + Tasks.** Add a structured spec and a task list before implementation.
- **Level 3 — Full workflow.** All 6 phases, validation gates, and steering documents committed alongside code.
- **Level 4 — Org-wide standard.** Multiple teams share spec format and boundary conventions; specs become the interface between teams and agents.

Most teams should start at Level 1 and graduate as the pain of skipping a phase becomes obvious.

## Key Takeaways

- The spec — not the code, not the chat — is the artifact you maintain.
- SDD is a **standard**, not a tool: the same spec must work across agents and IDEs.
- Adopt it in levels; don't try to land the full workflow on day one.
- Boundaries (Always / Ask / Never) are non-optional — they're how you make agent autonomy safe.

## Related

- [[sdd-workflow]] — The phases and spec anatomy
- [[sdd-roles-and-boundaries]] — Who does what, and the boundary framework
- [[kiro-specs|Kiro Specs]] — Reference implementation of the spec format
- [[research-plan-implement|Research Plan Implement (RPI)]] — a lighter-weight sibling workflow. See [[rpi-methodology/vs-other-frameworks|RPI vs Other Frameworks]] for the "orthogonal, not strict superset" framing.

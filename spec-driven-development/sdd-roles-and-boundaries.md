---
type: synthesis
title: SDD Roles and Boundaries
description: SDD defines three roles — Product Owner (owns intent and acceptance criteria), Developer (owns design and validation), and AI Agent (drafts and implements within declared boundaries) — and governs agent behaviour through a per-spec Always/Ask/Never framework.
bucket: ai-engineering
topic: spec-driven-development
tags: []
source: synthesis — spec-driven-development corpus
resource:
timestamp: 2026-05-09T07:23:23Z
status: active
related:
  - ai-engineering/spec-driven-development/sdd-overview.md
  - ai-engineering/spec-driven-development/sdd-workflow.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/agent-architecture/twelve-factor-agents.md
---

# SDD Roles and Boundaries

**Source:** (synthesis — spec-driven-development corpus)

SDD assigns three roles and a boundary framework that together make agent autonomy reviewable.

## Summary

SDD defines three roles — Product Owner (owns intent and acceptance criteria), Developer (owns design and validation), and AI Agent (drafts and implements within declared boundaries) — and governs agent behaviour through a per-spec Always/Ask/Never framework. The key design principle is that boundaries are declared per-spec rather than globally, so the trust envelope precisely matches the risk profile of each piece of work, allowing agents to move autonomously on safe actions without ever silently exceeding dangerous ones.

## Roles

- **Product Owner (PO).** Owns the brief and acceptance criteria. Decides scope and non-goals. Signs off on the spec before Design begins.
- **Developer.** Owns the design, the task decomposition, and the validation. Reviews everything the agent produces. Updates the spec when reality diverges.
- **AI Agent.** Drafts, implements, and tests within the boundaries declared in the spec. Surfaces ambiguity instead of guessing. Never silently exceeds its boundaries.

The roles can collapse — a solo developer wears all three hats — but the artifacts each role owns must still exist.

## The Always / Ask / Never Framework

Every spec declares three lists that govern agent behavior for that piece of work:

- **Always.** Actions the agent may take without confirmation. E.g., *run the test suite*, *format files*, *create new files inside `src/feature-x/`*.
- **Ask.** Actions that require human confirmation. E.g., *modify the database schema*, *add a new dependency*, *touch files outside the spec's scope*.
- **Never.** Actions the agent must refuse. E.g., *push to main*, *delete migrations*, *call paid APIs*, *modify security middleware*.

Boundaries are **per-spec**, not global. A migration spec can grant Always rights that a feature spec would put under Never. This is what makes the framework usable: the trust envelope matches the work.

## Why Boundaries Matter

Without explicit boundaries, every agent action becomes either a blocking confirmation or an unreviewable risk. The Always/Ask/Never split is how SDD lets agents move fast on the safe stuff without ever sneaking past the dangerous stuff.

## Key Takeaways

- Three roles: PO owns intent, Developer owns design/validation, Agent owns drafting within boundaries.
- Boundaries are declared per-spec, not once globally.
- Always = autonomous, Ask = confirm, Never = refuse.
- Roles can collapse onto one person, but the artifacts must still exist.

## Related

- [[sdd-overview]] — The principles behind SDD
- [[sdd-workflow]] — Where boundaries live in the spec
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — Steering docs and sub-agent firewalls implement the same idea at the harness level
- [[twelve-factor-agents|Twelve-Factor Agents]] — "Own your control flow" is the same insight applied to agent loops

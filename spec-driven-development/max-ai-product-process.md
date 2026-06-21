---
type: synthesis
title: Max's AI-First Product Development Process
description: This is Max's opinionated end-to-end process for building AI-first products, covering five phases: Investigation, Product Definition, Product Refinement, Product Development, and Iteration.
bucket: ai-engineering
topic: spec-driven-development
tags: []
source: self-authored — Massimiliano Manzini
resource:
timestamp: 2026-05-09T07:23:23Z
status: active
related:
  - ai-engineering/spec-driven-development/sdd-workflow.md
  - ai-engineering/spec-driven-development/sdd-roles-and-boundaries.md
  - ai-engineering/agent-workflows/research-plan-implement.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/ai-dev-tools/kiro-specs.md
---

# Max's AI-First Product Development Process

**Source:** (self-authored — Massimiliano Manzini)

A simplified, opinionated end-to-end process for building products with AI agents, distilled from extended personal research. Sits on top of [[sdd-overview|SDD]] and borrows from [[skill-issue-harness-engineering|harness engineering]] and the Heist way of working (see Way of Working bucket).

## Summary

This is Max's opinionated end-to-end process for building AI-first products, covering five phases: Investigation, Product Definition, Product Refinement, Product Development, and Iteration. The key differentiator is the Minion Approach in the Iterate phase — spawning context-blind user-impersonator agents to surface gaps that context-saturated teams consistently miss. The spec (per SDD) is the formal hand-off artefact between PM and development.

## The Five Phases

### 1. Investigation
Run product analysis, competitor analysis, business model, and market analysis. Best done **co-working** with an agent (not full delegation).

### 2. Product Definition
1. Draft and refine a **Product Mission**.
2. Define and challenge an **MVP**.
3. Define and challenge the feature list required for the MVP.
4. Define and challenge the **tech stack and code standards**.

### 3. Product Refinement
1. From the roadmap, generate per-feature specs following the [[sdd-workflow|SDD standard]].
2. Refine each spec with specialised agent teams role-playing UX designer, tech architect, marketing, and business analyst.

### 4. Product Development
1. Use [[skill-issue-harness-engineering|harness design principles]] to build a development loop with built-in QA.
2. Test the output personally; feed your own findings back into the loop or open new specs.

### 5. Iterate (Minion Approach)
1. Spawn a team of agents impersonating possible users — **with no access to project context** — to test and report findings as feedback.
2. Filter findings; turn the survivors into new specs.
3. Loop back to Phase 3.

## Hand-Off: PM → Development

The hand-off artefact is the **spec**, per [[sdd-overview|SDD]]. Tooling options Max has scouted:

- **AgentOS** — simple SDD project bootstrap plugin; dev-focused, light on PM/agent collaboration. ([video](https://www.youtube.com/watch?v=4PlVnrliN3Q))
- **Spec-Kit** — GitHub's open framework, becoming an industry standard alongside BMAD and OpenSpec. Model-agnostic but stiff. ([repo](https://github.com/github/spec-kit))
- **OpenSpec** — [openspec.dev](https://openspec.dev)

## IDEs

- **Kiro** — AWS's AI-first IDE built around SDD plus [[kiro-steering|Steering]] and [[kiro-hooks|Hooks]]. ([about](https://kiro.dev/about/))
- **Cursor** — mainstream AI-first IDE. ([cursor.com](https://cursor.com))

## Agent Team Building

- [Claude's agent team talk](https://www.youtube.com/watch?v=uHnLU404fto) — what an agent team can achieve.
- [Multi-agent product development coordination](https://www.youtube.com/watch?v=9d5bzxVsocw) — best practices for coordination.

## Way of Working

- [Obsidian + Claude Code](https://www.youtube.com/watch?v=eRr2rTKriDM) — synergy between Obsidian's markdown structure and Claude Code/co-work.

## Key Takeaways

- Treat the spec as the contract between PM and development — see [[sdd-roles-and-boundaries]].
- Co-work in investigation and definition; delegate in implementation.
- Always close the loop with **context-blind user-impersonator agents** ("minions") to surface gaps a context-saturated team would miss.
- Tooling is in flux: Spec-Kit, BMAD, OpenSpec, and Kiro are the main contenders; pick the one that fits the team's stiffness tolerance.

## Related

- [[sdd-workflow|SDD 6-Phase Workflow]]
- [[sdd-roles-and-boundaries|SDD Roles & Boundaries]]
- [[research-plan-implement|Research Plan Implement (RPI)]]
- [[skill-issue-harness-engineering|Harness Engineering for Coding Agents]]
- [[kiro-specs|Kiro Specs]]

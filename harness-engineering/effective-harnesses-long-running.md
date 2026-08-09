---
type: synthesis
title: Effective Harnesses for Long-Running Agents
description: Long-running AI agents must work across multiple context windows, but each new session starts with no memory.
bundle: ai-engineering
topic: harness-engineering
tags:
- harness-engineering
- long-running-agents
- agent-memory
- agent-architecture
resource: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
sources:
- id: effective-harnesses-for-long-running-agents
  resource: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
generated:
  by: claude-code/atlas-consolidate
  at: '2026-04-29T21:24:51Z'
status: stable
related: []
---

# Effective Harnesses for Long-Running Agents

**Source:** [Anthropic Engineering](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) (2026)
**Author:** Justin Young (Anthropic)

---

## Summary

Long-running AI agents must work across multiple context windows, but each new session starts with no memory. Anthropic solved this with a two-agent pattern: an **initializer agent** that sets up the environment on the first run, and a **coding agent** that makes incremental progress in every subsequent session while leaving structured artifacts for the next one.

## The Problem

Two core failure modes when agents work across many context windows:

- **One-shotting:** Agent tries to build everything at once, runs out of context mid-implementation, leaves half-finished undocumented work
- **Premature completion:** After some features exist, a later agent instance sees progress and declares the job done

## The Solution: Two-Agent Pattern

### Initializer Agent (first session only)
- Expands the user's high-level prompt into a comprehensive **feature list** (200+ features for complex apps)
- Creates an `init.sh` script to run the dev server
- Sets up a `claude-progress.txt` file for inter-session state
- Makes an initial git commit

![[anthropic-puppeteer-testing.gif]]
*Claude testing the claude.ai clone via Puppeteer MCP screenshots.*

### Coding Agent (every subsequent session)
1. Runs `pwd`, reads git logs and progress files
2. Reads feature list, picks highest-priority incomplete feature
3. Implements **one feature at a time** (critical for avoiding one-shotting)
4. Tests end-to-end using browser automation (e.g., Puppeteer MCP)
5. Commits to git with descriptive messages
6. Updates progress file

### Feature List Design
- Stored as **JSON** (model is less likely to inappropriately modify JSON vs Markdown)
- Each feature has a `passes: boolean` field
- Strongly-worded instructions prevent the agent from removing or editing test definitions
- Only the `passes` field should change

## Key Failure Modes & Solutions

| Problem | Initializer Fix | Coding Agent Fix |
|---------|----------------|-------------------|
| Declares victory too early | Feature list with all requirements | Read feature list, pick single feature |
| Leaves buggy/undocumented state | Git repo + progress file | Read progress + git log; write commit + update at end |
| Marks features done prematurely | Structured feature list | Self-verify with browser testing before marking "passing" |
| Wastes time on setup | `init.sh` script | Read `init.sh` at session start |

## Key Takeaways

- **Incremental progress** is the single most important pattern — one feature per session
- **Structured handoff artifacts** (progress file + git history) bridge the gap between context windows
- **JSON feature lists** resist model tampering better than Markdown
- **Browser automation testing** (Puppeteer MCP) catches bugs invisible in code alone
- The initializer/coder split mirrors what effective human engineers do: plan first, then execute incrementally
- Open question: single general-purpose agent vs. specialized [[twelve-factor-agents|multi-agent architecture]] (testing agent, QA agent, cleanup agent)

## See Also

- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — evolution of this work into a planner/generator/evaluator architecture
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — practical harness configuration guide
- [[research-plan-implement|Research Plan Implement (RPI)]] — similar phased approach applied as a developer workflow

---
type: synthesis
title: Skill Issue — Harness Engineering for Coding Agents
description: A comprehensive practitioner's guide to **harness engineering** — the art of leveraging a coding agent's configuration points (CLAUDE.md, MCP servers, skills, sub-agents, hooks, back-pressure) to improve output quality and task success rates.
bundle: ai-engineering
topic: harness-engineering
tags: [harness-engineering, claude-code, skills-and-hooks, context-engineering, multi-agent]
source: https://www.hlyr.dev/blog/skill-issue-harness-engineering-for-coding-agents
resource: https://www.hlyr.dev/blog/skill-issue-harness-engineering-for-coding-agents
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# Skill Issue — Harness Engineering for Coding Agents

**Source:** [hlyr.dev](https://www.hlyr.dev/blog/skill-issue-harness-engineering-for-coding-agents) (2026-03-12)
**Author:** Kyle (HumanLayer)

---

## Summary

A comprehensive practitioner's guide to **harness engineering** — the art of leveraging a coding agent's configuration points (CLAUDE.md, MCP servers, skills, sub-agents, hooks, back-pressure) to improve output quality and task success rates. Core thesis: most agent failures are configuration problems, not model problems.

![[skill-issue-meme.png]]

## What Is Harness Engineering?

```
coding agent = AI model(s) + harness
```

The harness is everything around the model: system prompt, tools, context management, sub-agents, hooks. **Harness engineering** (coined by Viv Trivedy) means systematically configuring these surfaces to improve reliability.

It's a **subset of [[twelve-factor-agents|context engineering]]** — specifically the part that involves leveraging harness configuration to manage context windows of coding agents.

![[harness-anatomy-viv.png]]
*Viv Trivedy's anatomy of an agent harness — the customization levers around the model.*

![[harness-customization-levers.png]]
*The four core levers: system prompt, tools/MCPs, context, sub-agents.*

![[harness-eng-subset-context-eng.png]]
*Harness engineering as a subset of context engineering.*

## Configuration Surfaces

### 1. CLAUDE.md / AGENTS.md
- Markdown files deterministically injected into the agent's system prompt
- The ETH Zurich study (138 agentfiles) found most are useless-or-worse:
  - LLM-generated ones **hurt** performance and cost 20%+ more
  - Human-written ones helped ~4%
  - Codebase overviews and directory listings didn't help at all
- **What works:** concise, universally applicable, hand-crafted instructions (<60 lines)
- Avoid: auto-generation, over-steering toward specific tools, irrelevant context

### 2. MCP Servers (for tools)
- Extend agent capabilities beyond file I/O and bash
- Tool descriptions get injected into system prompt → **too many tools is bad** (fills context, enters "dumb zone")
- If an MCP server duplicates a CLI well-represented in training data (GitHub, Docker, databases), just prompt the agent to use the CLI instead
- HumanLayer replaced the Linear MCP server with a tiny custom CLI + 6 example usages in CLAUDE.md — saved thousands of tokens

### 3. Skills (for reusable knowledge)
- Solve the problem of context overload via **progressive disclosure**
- Only loaded when the agent decides (or is told) it needs them
- Can bundle markdown files, CLIs, templates in a skill directory
- Security warning: skill registries (ClawHub, skills.sh) have distributed malicious skills

![[too-many-tools-dumb-zone.png]]
*Too many MCP tools fills the context window and pushes you into the "dumb zone".*

### 4. Sub-Agents (for context control)
![[sub-agents-context-firewall.png]]
*Sub-agents act as a "context firewall" — only the final result returns to the parent.*

- **The most powerful lever** for maintaining coherence across many sessions
- Act as a **"context firewall"** — parent only sees the prompt it wrote + the sub-agent's final result
- None of the intermediate tool calls pollute the parent's context
- Avoid "context rot" (Chroma research): performance degrades as context length increases, even on simple tasks
- **Don't** use sub-agents for role-playing ("frontend engineer", "backend engineer") — use them for **context isolation**
- Also useful for **cost control**: expensive model (Opus) for parent orchestration, cheaper model (Sonnet/Haiku) for sub-agent tasks

#### Why Bigger Context Windows Don't Help
- Longer context ≠ better retrieval — it just makes the haystack bigger
- If you think you need longer context, you probably need better **context isolation**

### 5. Hooks (for control flow)
- User-defined scripts that execute on agent lifecycle events
- Use cases: notifications (sounds), auto-approve/deny tool calls, integrations (Slack, GitHub PR), verification (typecheck/lint on stop)
- Key pattern: **silent on success, surface errors on failure** (exit code 2 re-engages the agent)

### 6. Back-Pressure (for self-verification)
- Strongly correlated with task success: the agent's ability to verify its own work
- Typechecks, unit/integration tests, code coverage, UI testing (Playwright)
- **Must be context-efficient** — swallow passing output, only surface errors
- Full test suite output (4,000 lines of passing tests) floods context and causes hallucinations

## What Didn't Work

- Designing the ideal harness upfront before hitting real failures
- Installing dozens of skills/MCP servers "just in case"
- Running full test suite (5+ min) after every session
- Micro-optimizing which sub-agents get which tools

## What Did Work

- Starting simple, adding config only when the agent actually failed
- Designing, testing, iterating — and throwing away things that didn't help
- Distributing battle-tested configs via repo-level settings
- Optimizing for **iteration speed**, not one-shot success
- Carefully paring down exposed capabilities once needs were clear

## Key Takeaways

- It's not a model problem — it's a **configuration problem**
- Sub-agents are the single most impactful lever for complex, multi-session work
- Progressive disclosure (skills, conditional loading) prevents context overload
- Back-pressure mechanisms (tests, typechecks) are the highest-leverage investment
- Context efficiency matters everywhere: tool descriptions, test output, MCP responses
- Models can be **over-fitted to their harness** (Opus 4.6 ranks #33 in Claude Code but #5 in a different harness on TerminalBench 2.0)
- Bias toward shipping — only optimize the harness when it's actually causing failures

## See Also

- [[twelve-factor-agents|Twelve-Factor Agents]] — the broader context engineering principles this builds on
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's initializer/coder pattern
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — GAN-inspired multi-agent harness
- [[kiro-hooks|Kiro Hooks]] — IDE-level hook implementation in Kiro
- [[kiro-steering|Kiro Steering]] — Kiro's approach to persistent agent context (similar to CLAUDE.md)

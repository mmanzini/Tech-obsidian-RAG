---
type: synthesis
title: Agentic Engineering — Brendan O'Leary (Kilo Code)
description: Brendan O'Leary (Kilo Code) frames agentic engineering as directing a tireless but judgment-free junior developer — where the primary constraint is context quality, not model capability.
bundle: ai-engineering
topic: agent-workflows
tags:
- agent-workflows
- context-engineering
- harness-engineering
- mcp
- skills-and-hooks
resource: https://www.youtube.com/watch?v=BEKc4P87XKo
sources:
- id: watch-v-bekc4p87xko
  resource: https://www.youtube.com/watch?v=BEKc4P87XKo
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-09T07:03:05Z'
status: stable
related:
- ai-engineering/agent-workflows/research-plan-implement.md
- ai-engineering/harness-engineering/skill-issue-harness-engineering.md
- ai-engineering/agent-workflows/agentic-engineering-approaches-compared.md
- ai-engineering/agent-architecture/twelve-factor-agents.md
---

# Agentic Engineering — Brendan O'Leary (Kilo Code)

**Source:** [YouTube talk](https://www.youtube.com/watch?v=BEKc4P87XKo), AI Engineer, 2026-04-08.

The shift from *using* AI (autocomplete, suggestions) to *working with* AI (collaborator that executes tasks, runs tests, opens PRs). Talk frames the agent as an "energetic, well-read, often confidently wrong junior developer" — fast and tireless, but lacking judgment and business context.

## Summary

Brendan O'Leary (Kilo Code) frames agentic engineering as directing a tireless but judgment-free junior developer — where the primary constraint is context quality, not model capability. The talk's recommended approach is the Research → Plan → Implement loop, which front-loads human thinking into research and planning phases so that implementation can run on cheap models with minimal context; bad output is almost always a context or specification failure by the human, not the agent.

## Mental Model

- Tools are picked up and put down; agents are collaborated with.
- The agent has read everything but knows nothing about *your* decisions from 3 months ago.
- Productivity gains (~30%) come from knowing **what to hand off and what to keep**, not from blindly accepting output.

## Context Engineering (Karpathy)

> "The delicate art and science of filling the context window with just what's needed for the next iteration."

Four practices:

1. **Persist outside the window** — scratchpads, memory files, `agents.md` so context can be reloaded fresh.
2. **Select carefully** — only pull in what's relevant for *this* step; disable unused MCP servers.
3. **Summarize / trim / compress** — after deep debugging, condense findings before moving to the fix.
4. **Isolate** — split work across parallel agents/sessions to prevent accumulation.

Failure modes:
- **Context rot** — quality degrades past ~50% window fill.
- **Poisoned context** — outdated comments, mixed tasks, or steering an agent back from a wrong path leaves residue. Better to start a new session.
- **MCP bloat** — every enabled MCP server burns input tokens on tool descriptions every turn.

## Research → Plan → Implement Loop

The talk's recommended workflow (see [[research-plan-implement|Research Plan Implement (RPI)]] for the canonical write-up):

1. **Research** (Kilo "Ask mode" — read-only): understand the system, files, paradigms, edge cases. Output: a research document the human reviews.
2. **Plan** (Architect mode): explicit step-by-step changes, files touched, test/verification strategy, **in/out of scope**. Output: a plan file in `plans/`. Often clear enough that a smaller/cheaper model can implement it.
3. **Implement** (Code mode, fresh session, low context): execute the plan, commit frequently, treat local git as your first PR review.

> "A bad line of research can become hundreds of lines of bad code." — Dex Horthy
> "AI can't replace thinking. It can only amplify the thinking you've done — or the lack of it."

Highest-leverage human time is in research and planning, not implementation.

## Agent Configuration — Three Bundles

- **Modes** — role-based behaviors (ask / architect / code).
- **`agents.md`** — always-on project README for agents: conventions, build/test commands, requirements. Becoming the de facto standard file.
- **Skills (`skills.md`)** — on-demand reusable playbooks for recurring workflows (e.g. changelog compiling, motion graphics).

Other tunable knobs: auto-approval scope (which tools the agent runs unattended), worktrees for parallel agents, what gets read inside vs. outside the workspace.

## MCP & Internal APIs

- MCP expands the tool surface (GitHub MCP, Context7 for fresh framework docs) but every server costs system-prompt tokens. Disable what you're not using.
- For internal platform APIs: prefer existing OpenAPI/Swagger spec → else convert to markdown in repo → else fetch a reference URL on demand → else build a custom MCP for complex multi-system flows.

## Habits

- One task per session.
- Watch the context meter; when in doubt, start fresh.
- Have the agent summarize the session to hand off to a new one (LLMs are good at writing prompts for LLMs).
- Review agent output as if it were a junior engineer's PR.

## Key Takeaways

- Agentic engineering = directing a fast, tireless, judgment-free collaborator — not autocompleting harder.
- Context is the primary constraint: expensive, easily poisoned, and quality-degrading past ~50% fill.
- The Research → Plan → Implement loop front-loads thinking so implementation can run on cheap models with low context.
- Configure agents through three layers: modes (role), `agents.md` (always-on rules), skills (on-demand playbooks).
- Disable unused MCP servers — they silently bloat every prompt.
- The Comic Sans intern story: bad output is almost always a context/spec failure by the human, not the agent.

## Related

- [[research-plan-implement|Research Plan Implement (RPI)]] — canonical write-up of the loop
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — deeper dive into CLAUDE.md, MCP, skills, sub-agents
- [[agentic-engineering-approaches-compared|Agentic Engineering — Approaches Compared]]
- [[twelve-factor-agents|Twelve-Factor Agents]] — own your prompts and context

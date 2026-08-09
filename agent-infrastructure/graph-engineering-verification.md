---
type: synthesis
title: Graph Engineering — Multi-Agent Graphs and the Verification Problem
description: Graph engineering fans one task out across many agents (nodes and edges, diamond and fan-in-at-barrier shapes); its failure mode is that one broken node corrupts the whole output, which makes self-built verification skills — and the model doing the judging — the decisive design choice.
bundle: ai-engineering
topic: agent-infrastructure
tags:
- multi-agent
- harness-engineering
- skills-and-hooks
- claude-code
resource: https://www.youtube.com/watch?v=H7t3uUp3HVw
sources:
- id: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw
  resource: Resources/transcriptions/transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-05T09:00:00Z'
status: stable
related:
- ai-engineering/harness-engineering/verification-loops-in-claude-code.md
- ai-engineering/claude-code-practice/claude-code-dynamic-workflows.md
- ai-engineering/agent-architecture/multi-agent-coordination-patterns.md
- ai-engineering/harness-engineering/skill-creator-evals.md
---

# Graph Engineering — Multi-Agent Graphs and the Verification Problem

**Source:** [Anthropic Just Fixed Graph Engineering's Greatest Flaw](https://www.youtube.com/watch?v=H7t3uUp3HVw) (AI LABS, YouTube)

---

## Summary

Graph engineering is the successor framing to loop engineering: instead of one agent working a goal in a straight line (do → verify → next step, everything waiting on the step before), a graph splits the main task into smaller parts and gives each part its own agent, running in parallel (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md). This is not the knowledge-graph kind of graph — it is a multi-agent execution topology; Claude Code's dynamic workflows are already graphs by another name (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md). The catch: one error in a small node disturbs the entire output and is hard to trace because all you see is the finished result. The fix the video builds — following Anthropic's verification-loops article — is layered self-built verification skills, with the judge model chosen deliberately, because "the node that does the judging is the one place where saving tokens costs you everything" (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).

## From loops to graphs

- A **loop** is a working cycle handed to the agent: you give the end goal instead of prompting every step, and it gets there on its own, adjusting as it goes. Its problem is shape — everything runs serially, so each step waits on the previous one even when the two are unrelated (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- A **graph** fans the task out: speed from parallelism, plus per-node cost control because you pick which model each node runs on (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- Cost warning: a graph burns far more tokens overall than a single agent — the video advises against running graphs on API pricing at all, and expects subscription limits to arrive much sooner ($20 plans are insufficient) (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).

## Anatomy: nodes, edges, shapes

- **Node** — a single job out of the bigger task; an agent doing its task in its own isolated context window and reporting back (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Edge** — controls how data moves between nodes, so one agent's output lands with the right agent at the right point (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Diamond shape** — one task splits into several sub-agents running side by side, then narrows back into a single agent that pulls everything into one answer (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Fan-in at a barrier** — the same problem goes out to several agents, each looking through a different lens; nothing moves forward until every one has reported back, then the fixes run (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).

## Why verification decides everything

With a fleet of agents, a huge pile of work comes back at once (hard to review) and failures are untraceable — you can't see which node started an error (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md). Agents self-verify by running tests, but that catches only major errors, not how the code is written. Claude Code's built-ins — the Verify skill, tool chaining (put exact project commands in CLAUDE.md), and code review skills — help, but the verification that works best is the one you build yourself with Skill Creator (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).

## The judge model matters most

The video's central empirical lesson: running a review skill on Haiku (cheap, job looked simple) returned a long list of issues; the same review on Opus flagged far fewer — but the Haiku findings were mostly things left in on purpose, which Opus worked out from surrounding code (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md). Inside a graph, a weak judge means agents burning tokens fixing things that were never broken, with no way to tell which node started it. The model you pick for judging decides the quality of the whole graph (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).

## Three kinds of verification skill (plus two patterns)

- **Standalone** — run manually for a deep pass on finished work (e.g. Cursor's "thermonuclear code review" fanning agents across security angles); don't fire it after every run (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Embedded** — fires automatically as part of the workflow (e.g. every new feature checked against component rules before implementation can finish); pre-installed skills can't be made auto-invoking — build your own (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Chained with an orchestrator** — one skill per review angle (stuffing all review types into one skill degrades results), with an orchestrator skill above whose only job is to run the others in parallel and merge findings into one report. Anthropic's own team chains Code Review + Simplify + Verify + a design skill checking against design.md — review from four directions (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Second Opinion** — the agent that built the thing is the worst one to review it, since it judges its own work from the same context it built with. The skill launches a fresh Claude session via the `-p` flag (no inherited context, unlike the built-in advisor which reads the current chat); slow, and worth explicitly running on Opus (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).
- **Chrome Headless Shell** — for visual verification inside workflows, a stripped-down browser that loads pages and takes screenshots far faster than full Chrome/Puppeteer/Playwright loops (source: transcript-anthropic-just-fixed-graph-engineerings-greatest-flaw.md).

## Key Takeaways

- Graph engineering = loop engineering gone wide: nodes (isolated agents) + edges (data routing), arranged in shapes like the diamond and fan-in-at-a-barrier.
- Every shape rests on verification — without proper checks, every downstream agent builds on top of a mistake.
- Never economise on the judging node: cheap review models produce false findings that cost more than they save.
- Build one skill per review angle and chain them under an orchestrator skill, rather than one bloated review skill.
- Unbiased review requires fresh context: a `-p` background session on a strong model, not the built-in advisor.
- The frame is tool-agnostic — demonstrated in Claude Code but applicable to Codex or any agent runtime.

## Related

- [[verification-loops-in-claude-code]] · [Verification Loops in Claude Code](../harness-engineering/verification-loops-in-claude-code.md) — the Anthropic article this video operationalises: standalone/embedded/chained taxonomy and the /code-review → /simplify → /verify → /design chain
- [[claude-code-dynamic-workflows]] · [Claude Code Dynamic Workflows](../claude-code-practice/claude-code-dynamic-workflows.md) — dynamic workflows are the graph you have already run without calling it one
- [[multi-agent-coordination-patterns]] · [Multi-Agent Coordination Patterns](../agent-architecture/multi-agent-coordination-patterns.md) — the canonical coordination patterns (generator-verifier, orchestrator-subagent) the diamond and barrier shapes instantiate
- [[skill-creator-evals]] · [Improving Skill-Creator](../harness-engineering/skill-creator-evals.md) — Skill Creator is the recommended build path because output comes tested with references and scripts

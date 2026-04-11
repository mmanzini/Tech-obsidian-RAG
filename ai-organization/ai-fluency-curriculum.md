# Getting Good at Claude — A Research-Backed Curriculum

**Source:** [claude.com/resources/tutorials/getting-good-at-claude-a-research-backed-curriculum](https://claude.com/resources/tutorials/getting-good-at-claude-a-research-backed-curriculum) (Anthropic, 2026-04-11)

Anthropic's AI Fluency Index now covers >50,000 conversations across Claude.ai, Claude Code, and Claude Cowork, measured against 11 behavioral fluency indicators. Two-track finding: some skills grow naturally with exposure, others require deliberate teaching. Training programs that ignore the distinction waste effort on the wrong dimension.

## The three tracks of fluency

1. **Signature move** — the gateway behavior that unlocks everything else. Product-specific.
2. **Description spectrum** — how users shape Claude's output, organized by **durability** (one-shot → persistent config). Grows organically with tenure.
3. **Discernment** — evaluating what Claude gives back. Does **not** grow with tenure, feature familiarity, or time. Must be explicitly taught.

## Signature moves (per product)

| Product | Signature move | Why |
|---|---|---|
| **Claude.ai (Chat)** | **Iterate** — refine through follow-up turns | Users who send one message and leave show almost no critical evaluation; iteration creates the space where other skills develop |
| **Claude Code** | **Clarify the goal** — state what you want before Claude starts working | Clusters with format specification, interaction style, task decomposition |
| **Claude Cowork** | **Clarify the goal** — write a brief that names what you need | Same clustering as Claude Code; iteration doesn't cluster the same way on agentic surfaces |

A Chat curriculum that doesn't teach iteration first builds on sand. A Code/Cowork curriculum that doesn't teach goal clarity produces users who hand off vague requests and wonder why output misses the mark.

## Description spectrum — grows with practice

Organized by durability: how long the feature affects your interactions.

- **Basic** — in-the-moment, single response. Iterate, add context, upload a file, pick a model.
- **Middle** — repeatable capability. Artifacts, thinking, connectors; slash commands, skills, MCPs; embedded questions, plugins.
- **Lasting** — configuration that affects every response that follows. Projects / custom instructions / memory; `CLAUDE.md`, hooks, sub-agents; scheduled workflows, multi-step automations.

Good news for trainers: tenured users find their way to the lasting end on their own. **If training time is limited, just expose people to features and let the spectrum grow organically.**

## Discernment — does not grow on its own

- Doesn't transfer from feature familiarity.
- Observational verification (skim diff, run test, read output) catches errors you can **see**, but misses wrong assumptions, missing context, and plausible-but-false claims.
- As entry-level work automates, intentional programs to teach "what good looks like" become mandatory — otherwise the junior-level calibration never gets built.

**If training time is limited, concentrate it on Discernment.** It's the one thing that must be taught.

## Curriculum sequence

1. Teach the signature move first.
2. Advance along the Description spectrum (exposure is enough).
3. **Revisit Discernment at every step.** Every module, every session — introduce a feature, then close with a "now question it" check.

## Per-product teaching tables

### Claude.ai (Chat)

| Durability | Features | Teaching approach | Discernment check |
|---|---|---|---|
| Basic | Iteration, model selection, uploads, web search | Show, then let people explore with their own tasks | Is this response actually usable, or does it need another turn? |
| Middle | Artifacts, extended thinking, connectors | Pair creation with format spec; position thinking as "when you want Claude to think harder" | Read the thinking. Does the reasoning hold, or did Claude just sound confident? |
| Lasting | Projects, custom instructions, memory | Show how a well-set-up Project changes every conversation in it | Is your Project feeding Claude the right context, or just more context? |

### Claude Code

| Durability | Features | Teaching approach | Discernment check |
|---|---|---|---|
| Basic | Prompt clarity, file refs, model selection | One clear goal, one agentic run, then review | Before you accept the diff, ask Claude what it assumed |
| Middle | Skills, slash commands, MCPs | Shape a repeatable capability | Test the capability on a case it might get wrong |
| Lasting | `CLAUDE.md`, hooks, sub-agents | Shape every session in the repo | Audit your configuration — is Claude using it the way you expect? |

### Claude Cowork

| Durability | Features | Teaching approach | Discernment check |
|---|---|---|---|
| Basic | Initial prompt, model selection, embedded questions, output review | Write a self-contained prompt; check what you got back | Before you use this, ask: what would make it wrong? |
| Middle | Connectors, plugins, skills | Shape a repeatable fetch-or-produce pattern | Did the connector pull what you needed, or what was easy to find? |
| Lasting | Scheduled workflows, multi-step automations | Shape work that runs without you | When did you last verify this workflow still produces good work? |

## Key Takeaways

- Two-track fluency: **Description** grows organically with exposure; **Discernment** has to be explicitly taught.
- Each product has a signature move — Chat rewards iteration; Claude Code and Cowork reward goal-clarity. Teach it first or everything downstream collapses.
- The Description spectrum is organized by **durability** — from in-the-moment shaping up to `CLAUDE.md`/hooks/scheduled workflows. Users find the advanced end on their own given enough exposure.
- Observational verification (reading diffs, running tests) is real but only catches **visible** errors; wrong assumptions and plausible-but-false claims require taught discernment.
- Curriculum rhythm: signature move → spectrum exposure → **"now question it"** Discernment check at every step.
- Ties to [[ai-organization/claude-code-productivity-takeaways|claude-code-productivity-takeaways]] (adoption) and [[harness-engineering/skill-issue-harness-engineering|harness engineering]] (the "lasting" end of the Claude Code spectrum).

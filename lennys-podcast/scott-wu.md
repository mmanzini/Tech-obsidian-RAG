# Scott Wu — Devin Is a Named Junior Engineer

**Source:** Lenny's Podcast, Scott Wu (co-founder and CEO, Cognition / Devin)

Scott's bet: the right unit of coding AI isn't a copilot or a tool, it's a *named autonomous engineer* called Devin who joins your team as a junior and gets better. Cognition pivoted 8 times before landing there. Today internal engineers each run 5 Devins in parallel, Devin writes 25%→50%+ of PRs, and the team's advantage rides on high-compute RL and Jevons Paradox.

## Named, not anonymous
- Most coding agents are features inside an IDE; Devin is a *persona* — you say "Devin did this PR" the way you'd say a colleague's name.
- The anthropomorphic frame is deliberate: it changes how humans review the work, how onboarding is structured, and how tasks are assigned.
- Consequence: Devin has a title trajectory — high-schooler → intern → junior engineer — and each model release is a "promotion."

## Jagged intelligence
- Devin (and all agents) are *jagged* — superhuman at some subtasks, confidently wrong at others.
- Managing jagged intelligence is the core skill: know which tasks to delegate vs. which to babysit.
- This is why the senior-engineer role persists — someone has to read the jagged edges of what agents produce.
- See [[agent-workflows/_index|Agent Workflows]] on adversarial review.

## 25%→50%+ PRs written by Devin
- Cognition's internal team of ~15 engineers operates each with 5 Devins running in parallel.
- Share of PRs authored by Devin crossed 25% early in 2025 and is now above 50%.
- The human role shifts from bricklayer (writing lines) to architect (framing problems, reviewing output, owning the spec).

## 8 pivots to get to "autonomous engineer"
- Cognition (founded Nov 2023) pivoted 8 times through varieties of coding AI before landing on the autonomous-engineer framing.
- Failed experiments included IDE plugins, PR-review bots, and issue-triagers — all ran into the ceiling of "helpful but not responsible."
- Lesson: the wedge is not a feature, it's an owner. An agent that *owns* a task end-to-end behaves differently from one that *assists* with a step.

## High-compute RL as the moat
- Cognition's training bet: long-horizon reinforcement learning on real engineering tasks, at very high compute per run.
- Short-horizon SFT / imitation learning caps at "looks right" behavior; RL pushes past to "actually works on CI."
- This is why coding agents got good at RL's natural fit (code passes tests or not) before they got good at essay writing.

## Jevons Paradox in software
- Cheaper code does not mean less code — it means *more* code, more projects, more ambition (Jevons Paradox).
- Scott's forecast: demand for software will rise faster than agent productivity for at least the next decade, which is why Cloudflare/Shopify/etc. are *hiring* engineers, not firing.
- Software becomes more like electricity — consumption expands to fill capacity.

## Bricklayer to architect
- The human engineer's role compresses away from typing syntax and expands into: framing problems, decomposing, reviewing, approving, integrating.
- The best engineers are already operating at architect level with agent swarms; juniors need to skip the bricklayer phase entirely.
- Connects to Simon Willison's "mid-career squeeze" — the people who only know how to lay bricks get displaced; the people who always operated at system-level are amplified.

## Key Takeaways
- Anthropomorphize the agent — "Devin" as a named junior engineer changes how humans interact with it.
- Jagged intelligence is the defining management challenge: great in patches, catastrophically wrong in others.
- The real wedge is *ownership* of a task, not assistance with a step.
- High-compute RL on real engineering tasks is the current frontier moat.
- Jevons Paradox: cheaper code = more demand for code, not fewer engineers.
- Human role compresses from bricklayer to architect — the bricklayer tier is the one that goes away.

## Related
- [[agent-architecture/_index|Agent Architecture]] — autonomous engineer pattern, ownership
- [[agent-workflows/_index|Agent Workflows]] — reviewing jagged output
- [[ai-organization/_index|AI & Organisation Design]] — 15 engineers × 5 agents, bricklayer→architect
- [[harness-engineering/_index|Harness Engineering]] — the substrate under the persona
- [[ai-product-development/_index|AI Product Development]] — Jevons Paradox, rising ambition
- [[lennys-podcast/_index|Lenny's Podcast]]

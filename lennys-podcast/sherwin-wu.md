# Sherwin Wu — 100% of PRs Reviewed by Codex

**Source:** Lenny's Podcast, Sherwin Wu (Head of Engineering, OpenAI API)

Sherwin's report from inside OpenAI API: 95% of engineers use Codex, 100% of PRs are reviewed by Codex, and Codex users open 70% more PRs than non-users. He frames the shift through SICP's wizard metaphor — we're becoming sorcerers — and warns of Sorcerer's Apprentice risk. His operating moves: a 100%-Codex code-base experiment team, context-engineering via .md/skills files, top-performer time allocation rules, and a surgeon-team org model. Also: "SV is a bubble" — most enterprise AI deployments today have negative ROI.

## The three numbers
- **95%** of OpenAI API engineers actively use Codex.
- **100%** of PRs are reviewed by Codex before human merge.
- **Codex users open 70% more PRs** than non-users, at comparable quality.
- These are the internal baselines OpenAI is using to size the shift.

## SICP wizards and the Sorcerer's Apprentice
- Sherwin's metaphor: the classic SICP textbook opens with "computation as magic, programmer as wizard." In the agent era we've become **sorcerers** — we command forces we don't fully understand.
- Warning: Fantasia's *Sorcerer's Apprentice* is the failure mode — brooms multiply faster than you can stop them.
- Operating implication: the *stop button* matters more than the *start button*; reviewability and shutdown must be first-class.

## 100%-Codex code-base experiment team
- OpenAI ran an internal team that committed to **no human-typed production code** for a period.
- Findings: feasible for some codebases, catastrophic for others; the difference was the quality of specs/tests/conventions the agent could lean on.
- Connects to [[spec-driven-development/_index|Spec-Driven Development]] and [[harness-engineering/_index|Harness Engineering]].

## Context engineering via .md and skills files
- The single highest-leverage activity for a team using Codex: maintain and iterate on markdown context files (CLAUDE.md / AGENTS.md equivalents) and custom skills.
- Sherwin treats context files as *the new code* — they're reviewed, versioned, and owned.
- Most productivity gains at OpenAI came from context hygiene, not model upgrades.

## Top-performer 50%+ time allocation
- Rule of thumb: top performers should spend **≥50% of their time working through agents**, not coding directly.
- Below 50% and you're under-leveraging; at 80%+ you're over the exhaustion cliff (see [[lennys-podcast/simon-willison|Simon Willison]]'s 11 a.m. wipe-out).
- The 50% number is a management lever — managers should actively coach time allocation upward.

## Surgeon team analogy (Mythical Man-Month)
- Sherwin revives Brooks's *surgeon team*: one principal programmer, everyone else in support roles.
- Updated for 2026: one senior engineer + a surgical team of agents.
- A 3-person human team running 10 agents each > a 30-person traditional team for most product work.
- See [[ai-organization/_index|AI Organization]].

## One-person billion-dollar startup — 2nd/3rd order effects
- Sherwin takes seriously the one-person $1B company thesis — but the **2nd/3rd order effects** are the story.
- 2nd order: existing SaaS incumbents become much more defensible because their distribution compounds against cheaper challengers' products.
- 3rd order: a golden age of B2B SaaS as solo builders extract value from niches too small for funded teams.
- The paradox: individual productivity soars, *and* big incumbents get stronger.

## SV bubble and negative-ROI AI deployments
- Sherwin's candid claim: **Silicon Valley is in a bubble** — most enterprise AI deployments in 2025–26 have *negative ROI* once fully loaded.
- The fix requires both **top-down mandate** (leadership committing to a new way of working) and **bottoms-up adoption** (engineers choosing to live in the tools daily).
- Either half alone fails: top-down-only = unused licenses; bottoms-up-only = scattered wins that don't change the org.

## Key Takeaways
- 95% of engineers / 100% PR review / +70% PR throughput — OpenAI API's current Codex baseline.
- We're sorcerers now, not wizards — reviewability and a stop button matter more than generation.
- Context engineering (.md + skills) is the new code; own it like source.
- Top performers should be ≥50% agent-mediated; manage time allocation deliberately.
- Surgeon-team model redux — one senior human + a swarm of agents beats a 30-person team.
- The 2nd/3rd order effects favor incumbents *and* solo builders simultaneously.
- Enterprise AI has a negative-ROI problem; fix requires top-down + bottoms-up together.

## Related
- [[harness-engineering/_index|Harness Engineering]] — .md files, skills, context engineering
- [[agent-architecture/_index|Agent Architecture]] — Codex as reviewer, surgeon-team pattern
- [[agent-workflows/_index|Agent Workflows]] — 100% PR review as a workflow gate
- [[spec-driven-development/_index|Spec-Driven Development]] — what made 100%-Codex possible
- [[ai-organization/_index|AI Organization]] — surgeon team, time allocation, top-down+bottoms-up
- [[ai-product-development/_index|AI Product Development]] — one-person unicorn, 2nd-order effects
- [[lennys-podcast/_index|Lenny's Podcast]]

# Improving Skill-Creator — Test, Measure, and Refine Agent Skills

**Source:** [claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills](https://claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills) (Anthropic, 2026-04-11)

Skill-creator (available in Claude.ai, Cowork, as a Claude Code plugin, and in the [anthropics/skills](https://github.com/anthropics/skills) repo) now bundles eval authoring, benchmark mode, multi-agent parallel execution, comparator agents, and description tuning. The goal: bring software-engineering rigor (testing, regression-catching, iterative improvement) to skill authoring without requiring authors to write code. Most skill authors are subject matter experts, not engineers.

## Two categories of skill (and why they need different eval strategies)

- **Capability uplift** — helps Claude do something the base model can't do consistently. Example: document-creation skills that encode patterns producing better output than prompting alone.
- **Encoded preference** — Claude can already do each piece, but the skill sequences them according to your team's process. Example: an NDA-review skill against set criteria, or a weekly-update drafter pulling data from MCPs.

Why the distinction matters for evals:

| Skill type | Why evals matter |
|---|---|
| Capability uplift | Becomes less necessary as models improve. Evals tell you **when** that happens. |
| Encoded preference | More durable, but only as valuable as fidelity to the actual workflow. Evals verify that fidelity. |

Either way: evals turn a skill that *seems* to work into one you *know* works.

## Eval authoring

Skill-creator helps you define:
- **Test prompts** (plus files if needed).
- **What "good" looks like** — the acceptance criteria.

It then runs the skill against the prompts and tells you whether it held up. Example: the PDF skill previously struggled with non-fillable forms (placing text at exact coords with no defined fields). Evals isolated the failure, leading to a fix that anchors positioning to extracted text coordinates.

Two headline uses for evals:

1. **Catching quality regressions** — as models and infrastructure evolve, a skill that worked last month may behave differently today. Run evals against a new model for an early signal before your team is impacted.
2. **Knowing when a skill has been outgrown** — if the base model starts passing your evals *without* the skill loaded, that's a signal the techniques have been absorbed into the model's default behavior. The skill isn't broken, it's obsolete.

## Benchmark mode

Standardized assessment using your evals, runnable after model updates or during iteration. Tracks:
- Pass rate
- Elapsed time
- Token usage

Results stay local — store them, feed a dashboard, or plug into CI.

## Multi-agent execution (speed + isolation)

- **Parallel agents** — each eval runs in an independent agent with its own clean context. Solves two problems at once: sequential runs are slow, and accumulating context bleeds between test runs.
- **Clean contexts** — per-run token and timing metrics with no cross-contamination.

## Comparator agents for A/B judgment

Blinded comparison of two outputs — two skill versions, or skill vs. no-skill — where the judge doesn't know which is which. Tells you whether a change **actually** helped, without prior-belief bias.

## Description tuning (triggering reliability)

Evals measure quality, but only matter if the skill **triggers** when it should.

- Too-broad descriptions → false triggers (skill fires when it shouldn't).
- Too-narrow descriptions → never fires.

Skill-creator analyzes the current description against sample prompts and suggests edits to cut both false positives and false negatives. Anthropic ran it across their own document-creation skills: 5 of 6 public skills showed improved triggering after tuning.

As skill counts grow, description precision becomes the bottleneck — see [[harness-engineering/skill-issue-harness-engineering|harness engineering]] on progressive disclosure and the small-surface-area principle.

## Looking ahead — skills as specifications

Today a `SKILL.md` is an implementation plan: detailed instructions telling Claude *how* to do something. As models improve, a natural-language description of *what* a skill should do may suffice, with the model figuring out the rest.

The eval framework is a step in that direction — evals already describe the "what." Eventually the eval set may **be** the skill.

## Key Takeaways

- **Capability-uplift vs encoded-preference** split: the former becomes obsolete as models improve; the latter is durable but only as good as its workflow fidelity. Evals answer different questions for each.
- Evals catch (1) regressions when models change under you, (2) the moment a skill has been absorbed into base-model capability.
- **Benchmark mode** tracks pass rate, time, and token usage across runs — treat a skill like production software.
- **Multi-agent parallel execution** isolates context across runs (no cross-contamination) and shrinks wall-clock time.
- **Comparator agents** do blinded A/B judging of two versions — the only reliable way to know a change actually helped.
- Description tuning attacks **triggering**, not output quality — false positives and false negatives both hurt as skill counts grow.
- Trajectory: today's `SKILL.md` files are how-to plans; evals are the "what"; eventually the eval set may be the skill itself.
- Related: [[harness-engineering/skill-issue-harness-engineering|Skill Issue — Harness Engineering]] on progressive disclosure and the cost of a crowded skill surface.

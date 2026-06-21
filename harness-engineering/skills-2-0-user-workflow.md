---
type: synthesis
title: Skills 2.0 User Workflow — Evals, AB Tests, and Context Engineering
description: Skills 2.0 adds built-in eval (testing) and AB test capabilities to the Claude skill creator.
bucket: ai-engineering
topic: harness-engineering
tags: []
source: https://www.youtube.com/watch?v=3cYusISFc9s
resource: https://www.youtube.com/watch?v=3cYusISFc9s
timestamp: 2026-04-29T21:24:51Z
status: active
related:
  - ai-engineering/harness-engineering/skill-creator-evals.md
  - ai-engineering/claude-code-practice/claude-cowork-full-guide.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/harness-engineering/nlh-meta-harness-harness-science.md
---

# Skills 2.0 User Workflow — Evals, AB Tests, and Context Engineering

**Source:** [How to Use Claude Skills 2.0 Better than 99% of People](https://www.youtube.com/watch?v=3cYusISFc9s)
**Author:** Ben van Sprundel (Ben AI YouTube channel)

---

## Summary

Skills 2.0 adds built-in eval (testing) and AB test capabilities to the Claude skill creator. This practical guide covers how to use evals effectively — with a single optimization target, explicit criteria, and controlled test setup — how to run AB tests for already-good skills, and how to use AB tests for context engineering decisions like reference file selection. It also covers the progressive update rule, which turns a skill into a self-learning system.

## What Skills 2.0 Evals Are and Why They Matter

Skills (skill MDs) are text files that instruct Claude what process to follow for a given automation. Anthropic updated the skill creator skill to include folders with eval viewer agents, scripts for benchmarking, and automated reporting. Users no longer have to evaluate a single test output manually; the skill creator can run multiple parallel test variations, score them automatically against user-defined criteria, and surface results in a structured report (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Skills are rarely finished after the first version. Most require five to ten iteration cycles before they become functional and optimal. Before Skills 2.0, it was hard to know what to change to improve output. Evals make this iteration loop ten times faster by making the feedback signal explicit and measurable (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

## The Iteration Loop

The core workflow is:

1. **Build** — create a first version of the skill using a structured prompt.
2. **Eval** — run automated tests against defined criteria.
3. **Review** — examine outputs and scores; add human feedback on individual variations.
4. **Optimise** — the skill creator updates the skill MD based on results and feedback.
5. Repeat until the skill performs well on the target criterion.

After creating a skill, the skill creator will automatically ask whether to run tests. Choosing yes triggers the eval flow (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

## Setting Up Evals Properly

### One Optimization Target at a Time

The most important constraint: **choose only one thing to optimize per eval run.** Testing five or six criteria simultaneously introduces too many variables. Optimize one thing, confirm the result, then move to the next (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Example: for a YouTube-to-newsletter skill, running an eval to optimize for "matching Ben's copywriting style and voice as closely as possible" yields much more actionable results than letting the skill creator define its own criteria (which tends to produce functional-correctness checks like "did it produce a .docx file" rather than quality signals) (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

### Define the Criteria Explicitly

When prompting the eval, specify:

- **What to optimize for** (one target — e.g., style and voice match)
- **Evaluation criteria** (e.g., how closely it follows reference examples; whether it uses em dashes; newsletter length; whether it includes personal stories from the author's background file)
- **Test setup** (e.g., five variations of the same YouTube video, not five different videos) (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md)

Controlling the test setup (same input, varied skill parameters) isolates the variable being tested. Using different inputs introduces confounding variation from the content itself.

### Reading the Results

Results come back as a structured report showing pass/fail per criterion per variation. Example outcome: two of five variations failed on word count and personal stories; one of five failed on style match. This is actionable — prompt the skill creator to optimize specifically for the failing criteria (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Human feedback on individual variation outputs can be added to the chat and used by the skill creator to further refine the skill MD.

## AB Tests: When and How to Use Them

### When to Use AB Tests

AB tests are for skills that are already functioning well. They are the tool for getting from "good" to "great" or "more efficient" — not for diagnosing broken skills (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Anthropic specifically recommends AB tests when a new model is released (e.g., Opus 4.7, 4.8), because a skill tuned for an older model may become redundant or suboptimal as the underlying model improves (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

### Running an AB Test

Tell the skill creator to run an AB test and specify:
- What to optimize for (speed, token usage, output quality, etc.)
- Any constraints (e.g., "it can't affect the step-by-step process — the process still has to be there and the reference file still has to be read")
- The test input to use

The skill creator spins up a version B (or multiple alternative versions) with modifications targeting the optimization goal, runs both versions on the same input, and produces a benchmark report comparing outputs and metrics (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Example result: original skill — 93,000 tokens, 204 seconds; optimised version — 77,000 tokens, 160 seconds. The optimised version initially failed on transcript extraction and word count, but a rerun confirmed this was tool behavior variation, not a skill regression. The optimised skill was adopted (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

## AB Tests for Context Engineering

One of the most impactful uses of AB tests is **reference file context engineering** — determining which combination of reference files actually improves output quality (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Example: run an AB test with all eight reference files vs. all eight minus the voice/personality reference file, on the same YouTube video, to assess whether the voice file improves or harms the copy. The outputs are compared side-by-side in the benchmark tab of the report (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Because copywriting output quality is subjective, the user still needs to read both outputs and make a judgment — but the AB test isolates the single variable (the reference file) so the judgment is on signal, not noise.

This kind of reference file experimentation is especially important for copywriting skills, where output quality depends heavily on which context files are provided and in what combination (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

## The Progressive Update Rule: Self-Learning Skills

One high-leverage addition to any skill prompt is a **progressive update rule**: instruct the skill MD to automatically update its own rules section whenever a user explicitly tells it not to do something anymore (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

Effect: any time the user gives quick feedback ("don't do X anymore"), the skill updates itself so it won't repeat that behavior in future runs. This turns the skill into a self-learning system — user feedback during normal operation becomes persistent skill improvement without requiring a formal eval cycle.

## Building a Skill Prompt That Works with Evals

A good skill-creation prompt should include all of the following sections (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md):

| Section | Purpose |
|---|---|
| **Trigger** | When should this skill activate? |
| **Goal** | What is the end output? |
| **Connectors / MCPs** | Which tools must be used? |
| **Reference files** | Context files to load (voice, strategy, examples) |
| **Step-by-step process** | Ordered instructions including human-in-the-loop checkpoints |
| **Output format** | Where and how to save/return the result |
| **Progressive updates** | Rule: if user says "stop doing X", auto-update the skill's rules section |

The demo YouTube-to-newsletter skill prompt illustrates this structure in concrete form:

- **Name and trigger condition**: "Skill: YouTube to Newsletter; trigger: user mentions repurposing a YouTube video into a newsletter."
- **Main goal**: one clear output objective.
- **Connectors**: explicit MCP tools required (e.g., YouTube transcript via Apify).
- **Reference files**: explicitly listed (voice/personality doc, writing framework, newsletter examples, ICP description, newsletter strategy).
- **Step-by-step process**: complete sequence including where human-in-the-loop pauses occur (e.g., "present five newsletter angles via Q&A box").
- **Output format**: where and how the final artifact is saved.
- **Progressive update rule**: "whenever a user specifies not to do something, automatically update the rules section in the skill MD."

The more precise the initial prompt, the more efficient and accurate the eval and iteration cycle (source: transcript-how-to-use-claude-skills-20-better-than-99-of-people.md).

## Key Takeaways

- Evals are the core iteration mechanism for Skills 2.0: automated multi-variation testing with automatic scoring against user-defined criteria.
- Always optimize one criterion at a time; testing multiple things simultaneously creates too many variables to act on.
- Define explicit eval criteria rather than letting the skill creator choose — functional-correctness defaults (did it produce a file?) are not quality signals.
- Control the test setup: same input, varied skill parameters, specified variation count.
- AB tests are for already-good skills — for optimizing speed, token usage, or quality, or for testing across model upgrades.
- AB tests are also the right tool for reference file context engineering decisions: isolate one variable (presence/absence of a file) and compare outputs.
- The progressive update rule makes a skill self-learning: user feedback during operation becomes persistent skill improvement.

## Related

- [[skill-creator-evals|Improving Skill-Creator — Test, Measure, Refine Skills]] — Anthropic's official engineering post on the same feature (eval authoring, benchmark mode, comparator A/B, description tuning)
- [[claude-cowork-full-guide|Claude Cowork — Full Practical Guide]] — broader non-technical agent harness context in which skills operate
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — practical guide to skills in the Claude Code context
- [[nlh-meta-harness-harness-science|NLH, Meta Harness, and the Science of Harness Engineering]] — context engineering as a harness-level discipline

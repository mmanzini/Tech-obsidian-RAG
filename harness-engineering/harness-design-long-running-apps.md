---
type: synthesis
title: Harness Design for Long-Running App Development
description: Building on Anthropic's earlier [[effective-harnesses-long-running|two-agent harness]], this work introduces a **GAN-inspired three-agent architecture** (planner, generator, evaluator) for autonomous full-stack app development.
bucket: ai-engineering
topic: harness-engineering
tags: []
source: https://www.anthropic.com/engineering/harness-design-long-running-apps
resource: https://www.anthropic.com/engineering/harness-design-long-running-apps
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# Harness Design for Long-Running App Development

**Source:** [Anthropic Engineering](https://www.anthropic.com/engineering/harness-design-long-running-apps) (2026)
**Author:** Prithvi Rajasekaran (Anthropic Labs)

---

## Summary

Building on Anthropic's earlier [[effective-harnesses-long-running|two-agent harness]], this work introduces a **GAN-inspired three-agent architecture** (planner, generator, evaluator) for autonomous full-stack app development. Key insight: separating the agent doing the work from the agent judging it is far more tractable than making a generator critical of its own output.

## Why Naive Implementations Fail

Two persistent problems even with good harness design:

1. **Context coherence loss** — models lose focus as context fills; some exhibit "context anxiety" (wrapping up prematurely near perceived context limits). Context resets (full clear + structured handoff) beat compaction for this.
2. **Self-evaluation bias** — agents confidently praise their own mediocre work. Especially bad for subjective tasks like design, but present even for verifiable ones.

## The Three-Agent Architecture

### Planner
- Takes a 1–4 sentence prompt → expands into full product spec
- Prompted to be **ambitious about scope** but stay at product/high-level technical design (not granular implementation details — errors in spec cascade downstream)
- Instructed to weave AI features into specs

### Generator
- Works in **sprints**, one feature at a time from the spec
- Self-evaluates at end of each sprint before handing off to QA
- Uses git for version control and recovery
- Stack: React, Vite, FastAPI, SQLite/PostgreSQL

### Evaluator (QA)
- Uses **Playwright MCP** to click through the running app like a real user
- Grades each sprint against criteria + bugs found
- Hard thresholds — if any criterion falls below, sprint fails with detailed feedback
- Before each sprint: generator and evaluator negotiate a **sprint contract** (what "done" looks like)

## Frontend Design: Making Subjective Quality Gradable

Four grading criteria (weighted toward design quality and originality):

1. **Design quality** — coherent mood/identity, not a collection of parts
2. **Originality** — evidence of custom decisions, not template/AI defaults (penalizes "purple gradients over white cards")
3. **Craft** — typography, spacing, color harmony, contrast ratios
4. **Functionality** — usability independent of aesthetics

Calibrated with few-shot examples. 5–15 iterations per generation, up to 4 hours per run.

## Iterating on the Harness (Opus 4.5 → 4.6)

Key principle: **every harness component encodes an assumption about what the model can't do on its own — stress-test those assumptions as models improve.**

With Opus 4.6:
- Removed sprint construct entirely (model handles coherence natively)
- Moved evaluator to single pass at end (still catches real gaps)
- Generator ran coherently for 2+ hours without decomposition
- Evaluator value is **proportional to task difficulty** — unnecessary for easy parts, essential at the edge of model capability

![[harness-design-solo-run.webp]]
*Solo-run output: broken game, wasted layout space.*

![[harness-design-full-harness-run.webp]]
*Full-harness run: working play mode, richer editors, integrated AI.*

### Cost Comparison (Retro Game Maker)

| Harness | Duration | Cost |
|---------|----------|------|
| Solo agent | 20 min | $9 |
| Full harness | 6 hr | $200 |

Solo: broken game, wasted space, rigid UX. Full harness: working play mode, richer editors, AI integration.

### DAW Build (Updated Harness, Opus 4.6)

| Phase | Duration | Cost |
|-------|----------|------|
| Planner | 4.7 min | $0.46 |
| Build R1 | 2 hr 7 min | $71 |
| QA R1 | 8.8 min | $3.24 |
| Build R2 | 1 hr 2 min | $37 |
| QA R2 | 6.8 min | $3.09 |
| Build R3 | 10.9 min | $5.88 |
| QA R3 | 9.6 min | $4.06 |
| **Total** | **3 hr 50 min** | **$125** |

## Key Takeaways

- **Separate generation from evaluation** — tuning an external evaluator to be skeptical is far easier than making a generator self-critical
- **Sprint contracts** between generator and evaluator bridge high-level spec → testable implementation
- **Re-examine harnesses when new models land** — strip what's no longer load-bearing, add new capabilities
- The space of interesting harness combinations **doesn't shrink** as models improve — it moves
- QA agents need significant prompt tuning; out of the box, Claude is a poor QA agent (talks itself out of real findings)

## See Also

- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — the earlier two-agent foundation this builds on
- [[adversarial-review|Adversarial Review]] — similar principle of forcing the reviewer to find problems
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — practical guide to harness configuration surfaces

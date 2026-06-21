---
type: synthesis
title: Quick Dev
description: Quick Dev is a BMad workflow designed to **minimize human-in-the-loop turns** without sacrificing output quality.
bucket: ai-engineering
topic: agent-workflows
tags: []
source: https://docs.bmad-method.org/explanation/quick-dev/
resource: https://docs.bmad-method.org/explanation/quick-dev/
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# Quick Dev

**Source:** [BMad Method docs](https://docs.bmad-method.org/explanation/quick-dev/)

---

## Summary

Quick Dev is a BMad workflow designed to **minimize human-in-the-loop turns** without sacrificing output quality. It lets the model run longer between checkpoints, then brings the human back only when judgment is required or it's time to review the result. Where [[research-plan-implement|Research Plan Implement (RPI)]] adds structure for predictability, Quick Dev rebalances the speed/control tradeoff for trusted execution.

![[quick-dev-workflow.png]]
*The Quick Dev workflow: intent compression → routing → autonomous run → human review.*

## Why It Exists

Human-in-the-loop turns are necessary but expensive. LLMs still misread intent, fill gaps with confident guesses, drift, and generate noisy review output. But constant human intervention limits velocity. Quick Dev trusts the model to run unsupervised for longer stretches — but only after the workflow has created a strong enough boundary to make that safe.

## Core Design

### 1. Compress Intent First
Human + model collaborate to shape a rough request into one coherent goal. Inputs can be: a couple phrases, a bug tracker link, plan-mode output, chat text, or a BMad story number. Before the workflow runs autonomously, intent must be:
- Small enough
- Clear enough
- Contradiction-free

### 2. Route to the Smallest Safe Path
- Small zero-blast-radius changes → straight to implementation
- Everything else → fuller path with planning, so the model has a stronger boundary

### 3. Run Longer with Less Supervision
On the fuller path, the approved spec becomes the boundary the model executes against. That's the whole point.

### 4. Diagnose Failure at the Right Layer
- If implementation is wrong because **intent** was wrong → patching code is the wrong fix
- If code is wrong because **spec** was weak → patching the diff is the wrong fix
- Review findings route correction back to the layer where the failure entered

Only truly local problems get patched locally.

### 5. Bring the Human Back Only When Needed
High-value human moments:
- **Intent clarification** — turning messy requests into coherent goals
- **Spec approval** — confirming the frozen understanding is right
- **Intent-gap resolution** — when review proves the workflow couldn't safely infer meaning
- **Final review** — the primary checkpoint at the end

## Why the Review System Matters

Review isn't just bug-finding — it's **routing correction without destroying momentum**.

Agentic reviews go wrong in two ways:
1. Too many findings → human sifts through noise
2. Surfacing unrelated issues → every run becomes an ad hoc cleanup project

Quick Dev treats review as **triage**:
- Some findings belong to the current change
- Some don't — those get **deferred** rather than forced into the current run
- Optimizes for **signal quality**, not exhaustive recall

Imperfect triage is acceptable. Better to misjudge some findings than flood the human with thousands of low-value comments.

The review design depends on **context-free subagents** — the workflow works best when subagents (or CLI-invoked LLMs) can review with fresh context, evaluating the artifact rather than the intent.

## Comparison with [[research-plan-implement|RPI]]

| Aspect | RPI | Quick Dev |
|--------|-----|-----------|
| Optimizing for | Predictability, correctness | Velocity with safety boundary |
| Human checkpoints | Between every phase | Only at intent + final review |
| Best for | Complex multi-file refactors, migrations | Faster iteration on routed work |
| Failure routing | FAR/FACTS gates | Layer-aware diagnosis |

## Key Takeaways

- **Compress intent before autonomy** — the workflow needs a strong boundary before it can run unsupervised
- **Route to the smallest safe path** — not every change needs a full plan
- **Diagnose failure at the right layer** — patching code can't fix bad intent
- **Treat review as triage**, not exhaustive bug-hunting; defer incidental findings
- **Context-free subagents are the cornerstone** of the review design
- Human attention is the bottleneck — save it for moments where human reasoning has the highest leverage

## See Also

- [[research-plan-implement|Research Plan Implement (RPI)]] — more structured phased alternative
- [[adversarial-review|Adversarial Review]] — BMad's technique for forcing genuine review (used as the review subagent here)
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — broader context on subagents as context firewalls

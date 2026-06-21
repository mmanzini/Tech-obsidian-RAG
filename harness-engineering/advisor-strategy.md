---
type: synthesis
title: The Advisor Strategy — Give Sonnet an Intelligence Boost with Opus
description: Pair **Opus as advisor** with **Sonnet or Haiku as executor** to get near-Opus intelligence at near-executor cost.
bucket: ai-engineering
topic: harness-engineering
tags: []
source: https://claude.com/blog/the-advisor-strategy
resource: https://claude.com/blog/the-advisor-strategy
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# The Advisor Strategy — Give Sonnet an Intelligence Boost with Opus

**Source:** [claude.com/blog/the-advisor-strategy](https://claude.com/blog/the-advisor-strategy)
**Published:** 2026-04-09

---

## Summary

Pair **Opus as advisor** with **Sonnet or Haiku as executor** to get near-Opus intelligence at near-executor cost. The executor runs the task end-to-end (tools, iteration, user-facing output); when it hits a decision it can't confidently solve, it consults Opus for a plan, correction, or stop signal — and resumes. The **advisor tool** (beta `advisor_20260301`) makes this a one-line addition to a Messages API request.

## The Inversion

This **inverts the common sub-agent pattern**. Instead of:

> Large orchestrator model → decomposes → delegates to smaller workers

The advisor strategy flips it:

> Small, cost-effective executor drives the loop → escalates to Opus only when needed

No decomposition, no worker pool, no orchestration logic. Frontier-level reasoning applies **only when the executor needs it**; the rest of the run stays at executor cost.

The advisor never calls tools or produces user-facing output — it only gives guidance.

## Benchmark Results

| Setup | Benchmark | Result |
|---|---|---|
| Sonnet 4.6 + Opus advisor | SWE-bench Multilingual | **+2.7pp** over Sonnet solo, **-11.9%** cost per task |
| Haiku 4.5 + Opus advisor | BrowseComp | **41.2%** vs 19.7% Haiku solo (more than 2x), **-85%** vs Sonnet solo cost |
| Sonnet + advisor | BrowseComp, Terminal-Bench 2.0 | Score improvements at lower cost per task than Sonnet alone |

Haiku + advisor trails Sonnet solo by 29% in score but costs 85% less — compelling for high-volume tasks.

## How It Works

Declare the advisor tool in your Messages API request. The executor decides when to invoke it. On invocation, curated context routes to the advisor model, the plan returns, and the executor continues — **all inside a single `/v1/messages` request**, no extra round-trips.

```python
response = client.messages.create(
    model="claude-sonnet-4-6",  # executor
    tools=[
        {
            "type": "advisor_20260301",
            "name": "advisor",
            "model": "claude-opus-4-6",
            "max_uses": 3,
        },
        # ... your other tools
    ],
    messages=[...]
)
# Advisor tokens reported separately in the usage block
```

## Economics

- **Advisor tokens** billed at advisor rates (typically 400–700 text tokens per plan)
- **Executor tokens** billed at executor rates (the bulk of the output)
- Overall cost stays well below running the advisor model end-to-end
- `max_uses` caps advisor calls per request for cost control
- Advisor tokens reported separately in the usage block → trackable spend per tier

## Getting Started

1. Add beta header: `anthropic-beta: advisor-tool-2026-03-01`
2. Add `advisor_20260301` to Messages API request
3. Modify system prompt per the suggested coding-task template

**Eval recommendation:** run three configurations against your existing eval suite:
- Sonnet solo
- Sonnet executor + Opus advisor
- Opus solo

## Key Takeaways

- **Advisor strategy ≠ orchestrator pattern.** The small model drives; the large model advises on demand
- **Score ↑, cost ↓.** Counter-intuitive but benchmarked: Sonnet + advisor beats Sonnet solo *and* costs less per task
- **One-line enablement** — the advisor tool is just another entry in the tools array, handoff happens inside a single request
- **Haiku + advisor** is the price/quality sweet spot for high-volume tasks that still need occasional frontier reasoning
- Works alongside web search, code execution, and other server-side tools in the same loop
- **Eval before rolling out** — compare executor-solo, executor+advisor, advisor-solo on your actual workload

## See Also

- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence — 3 Key Patterns]] — "don't change models" cache principle (advisor tool sidesteps this by scoping the switch inside a single request)
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — the sub-agent pattern that advisor strategy inverts
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — cost-control via Opus parent + Sonnet/Haiku sub-agents (the traditional pattern)
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — initializer/coder role specialization
- [[twelve-factor-agents|Twelve-Factor Agents]] — "own your control flow" principle
- [[context-engineering|Context Engineering]] — the instruction-budget constraint the advisor strategy sidesteps (rpi-methodology)

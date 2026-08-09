---
type: synthesis
title: Post-RPI Evolution
description: RPI hit three critical failure modes at scale — silent instruction-budget overflow (85 of ~150 instructions consumed before core work), magic-words dependency, and the plan-reading illusion — driving HumanLayer to evolve it into CRISPY/QRSPI (7–8 stages, each <40 instructions).
bundle: ai-engineering
topic: rpi-methodology
tags:
- agent-workflows
- harness-engineering
- context-engineering
- agent-architecture
generated:
  by: claude-code/atlas-consolidate
  at: '2026-04-29T21:54:50Z'
status: stable
related: []
---

# Post-RPI Evolution

**Source:** (self-authored)

## Summary

RPI hit three critical failure modes at scale — silent instruction-budget overflow (85 of ~150 instructions consumed before core work), magic-words dependency, and the plan-reading illusion — driving HumanLayer to evolve it into CRISPY/QRSPI (7–8 stages, each <40 instructions). In parallel, Geoff Huntley's Ralph Loop demonstrated that extreme context isolation (full agent reset between iterations, state in files) can outperform stateful sessions, and HumanLayer's CodeLayer IDE enabled distributed parallel RPI with up to 50% efficiency gains.

RPI was the baseline, not the endpoint. Within two years of its 2024 introduction, the methodology evolved in three directions: **deeper structural decomposition (CRISPY/QRSPI)**, **distributed execution (multi-agent orchestration)**, and **meta-layer infrastructure (harness engineering)**. This page traces the evolution and the failures that drove it.

## Problems RPI Hit at Scale

Three critical failure modes, documented by HumanLayer itself:

### 1. Silent Instruction Budget Overflow

- Frontier LLMs reliably follow **~150-200 instructions**, regardless of window size
- Original RPI prompt contained **85+ instructions** across phases → **56% of budget** before core work
- Failure mode is **silent**: the model doesn't error, it silently skips the deepest instructions, and you only find out after reviewing output

### 2. Magic Words Dependency

- RPI required specific trigger phrases (e.g., *"Work back and forth with me"* to activate interactive design discussion)
- Without the incantation, agents skipped critical steps
- Reliable tools should produce correct behaviour by default — magic-word dependency is a design flaw

### 3. The Plan-Reading Illusion

- Well-written plans **appeared to validate direction** but masked technical assumptions
- A coherent 1,000-line plan could describe ideal component interaction without understanding actual codebase constraints, dependency patterns, architectural debt
- Engineers reviewing plans experienced an *illusion* of validation while essentially performing equivalent work to post-hoc code review
- When plans diverged from implementation, the plan's persuasive prose **prevented timely course correction**

Plus: limited applicability to **greenfield** (nothing to research) and **non-deterministic** work (performance optimisation, security hardening, race-condition debugging).

## CRISPY / QRSPI: The Structured Evolution

HumanLayer expanded RPI from 3 phases to **7-8 stages**:

| # | Stage | Purpose |
|---|---|---|
| 1 | **Questions** | Define what the team is trying to accomplish; clarify objectives and constraints |
| 2 | **Research** | Investigate the codebase — shorter and more focused because questions are explicit |
| 3 | **Iterate / Discuss** | Present design options, discuss approach. Replaces "magic words" with explicit alignment artifacts |
| 4 | **Structure** | ~2-page outline of implementation structure (not a 1,000-line plan) |
| 5 | **Plan** | Detailed step-by-step instructions, shorter and more focused |
| 6 | **Worktree** | Set up working context, branch strategy, file layouts |
| 7 | **Implement** | Execute with verification |
| 8 | **Reflect** (optional) | Review outcomes, incorporate learning |

### Instruction Budget as Design Constraint

Critical change: **each stage contains fewer than 40 instructions**, respecting the budget. Decomposition allows full instruction adherence at each stage and creates explicit human review points between stages — replacing fragile prompt keywords.

### Outcomes

- **2-3x productivity gains** with maintained code quality
- Reduced silent failures
- Eliminated magic-words brittleness while preserving reliability benefits

## Ralph Loop: Extreme Context Reset

Parallel to HumanLayer's evolution, Geoff Huntley developed the **Ralph Loop** — named after a conceptual agent that constantly resets context.

```bash
while :; do cat PROMPT.md | npx --yes @sourcegraph/amp ; done
```

- Run an agent in an infinite loop with a simple declarative prompt
- Reset the entire agent between iterations
- Maintain state through external files

**Why it works:** stateless invocations with strong external state management can outperform stateful sessions. Context window *engineering* (managing what enters each invocation) matters more than context window *size*. Code is cheap — running the loop on fresh code with an identical prompt simply re-opens a PR.

### Timeline

- **June 2025** — initial community demonstration
- **August 2025** — practical applications emerge, particularly for refactoring
- **December 2025** — official Anthropic plugin, though users prefer simple bash implementations

### Connection to RPI

Takes CRISPY's context reset philosophy to its logical extreme: reset between *task iterations*, not just phases. Demonstrates that **radical context isolation** can maintain coherence across very long workflows, provided the declarative spec is strong enough.

## Distributed RPI: Parallel Workflows

**HumanLayer's CodeLayer IDE** enables keyboard-orchestrated parallel agent execution:

- Spawn multiple Claude instances with a keystroke
- Route specific tasks to specialised agents
- Merge outputs seamlessly
- **Up to 50% performance / token efficiency improvement** through parallelisation

**Practical example:** one session handles frontend refactoring while another tackles backend optimisations. Each operates in a fresh context window, executing RPI phases independently. The parent harness coordinates handoffs.

RPI's strength isn't monolithic execution — it's its capacity to be **distributed across task boundaries**, with each agent owning a focused piece.

## Instruction Budget Decomposition: The Core Insight

Academic validation for the evolution:

- **IFScale benchmark (2025)** — 500-instruction tasks across frontier models
  - Reasoning models (o3, Gemini 2.5 Pro) maintain near-perfect performance through **250-300** instructions
  - Standard frontier models (Claude Sonnet, GPT-4o) degrade measurably around **150-200**
  - Smaller models degrade exponentially

**Rather than waiting for models to improve**, the evolution decomposed monolithic prompts into smaller stages. Larger context windows actually *worsen* performance without improved instruction capacity — agents struggle to prioritise.

## The BAML Case Study

HumanLayer's most significant published success: refactoring a **300,000-line Rust codebase** (BAML, a programming language for LLM structured output).

- Developer: **amateur-level Rust** experience, new to the codebase
- Methodology: ACE framework with RPI phases
- **Phase 1 (Research & Planning):** 3 hours
- **Phase 2 (Implementation):** 4 hours
- **Total:** 7 hours
- **Outcome:** 35,000 lines of production code, all PRs passed expert review

≈ 3-5 days of senior engineer work compressed into 1 day.

**But:** single case study, single task class (feature addition to a Rust library). No published case studies for greenfield, large architectural refactors, non-deterministic optimisation, failed RPI projects, or RPI slower than alternatives.

## Broader Industry Adoption

CRISPY-like decomposition appears across multiple frameworks:

- **Anthropic's Harness Engineering** — two- and three-agent pipelines implement RPI-like phases across context windows ([[effective-harnesses-long-running]], [[harness-design-long-running-apps]])
- **Google ADK** — sequential pipeline, orchestrator-worker, generator-critic all implement multi-phase decomposition
- **McKinsey QuantumBlack** — Specification-Driven Development treats specs as primary artifact; agents decompose into deterministic stages (spec-first rather than research-first)

## Predictions for Evolution

1. **Toward reasoning models** — extended-thinking models (o3, Opus 4.6) handle higher instruction densities gracefully. Evolution might move from 7-stage CRISPY back to 3-4 explicitly-reasoned stages, with the model doing more internal decomposition
2. **Declarative specs over prescriptive workflows** — teams may express intent declaratively; the agent chooses the workflow. Harness provides multiple strategies (RPI, SDD, Quick Dev) and the agent routes
3. **Context isolation as default infrastructure** — sub-agents become standard; harnesses default to parent agents with focused sub-agents for specific tasks

## Counterpoint: Gartner's 40% Cancellation Prediction

**40% of agentic AI projects will be cancelled by 2027.** Root causes:
- Rising token costs ($0.14/task POC → $5-8/task production scale)
- Unclear value delivery
- Inadequate risk controls
- Cost exponentiality: some documented cases show **717x scaling from POC to production**

**Implication:** structured workflows reduce hallucination and improve output quality, but **do not eliminate the fundamental economics problem of token costs scaling faster than value delivery**.

## Conclusion

RPI evolved from a three-phase pattern into a distributed, multi-stage framework addressing specific failure modes as practitioners hit them. **CRISPY/QRSPI, Ralph Loop, and Anthropic's harness engineering all implement the same insights**:

- Context isolation beats context expansion
- Decomposition beats monolithic execution
- Explicit alignment beats implicit understanding

The field is moving toward infrastructure that treats **context as a precious, managed resource**. Effective agentic engineering is primarily an engineering challenge around **configuration and context management**, not a challenge requiring fundamentally smarter models.

Whether this can deliver at enterprise scale given token cost realities **remains open**.

## Key Takeaways

- **RPI's three failure modes at scale:** silent instruction-budget overflow (85 of 150 budget), magic-words dependency, plan-reading illusion
- **CRISPY/QRSPI = 7-8 stages**, each <40 instructions, explicit alignment replacing magic words
- **Ralph Loop** demonstrates extreme context reset outperforms stateful sessions — state lives in files, not context
- **Distributed RPI** via tools like CodeLayer gives up to 50% gains from parallelisation
- **Instruction budget is the constraint**, not context window size — backed by IFScale benchmark
- **BAML case study** is the most prominent success; failure cases remain unpublished
- **Gartner 40% cancellation forecast** is the sobering counterweight — structured workflows don't solve token-cost economics

## See Also

- [[humanlayer-and-hitl|HumanLayer and HITL]] — the ACE framework and 12-factor agents origin story
- [[context-engineering|Context Engineering]] — the instruction-budget constraint in detail
- [[industry-landscape|Industry Landscape]] — how other frameworks implement similar insights
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]]
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]]
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]]
- [[twelve-factor-agents|Twelve-Factor Agents]]

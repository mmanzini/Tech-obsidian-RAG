---
type: synthesis
title: Knowing When Not to Use AI — Agents vs Rules vs ML vs Humans
description: A four-way decision framework — humans for judgment and accountability, rules/code for stable deterministic logic, ML for patterns and predictions, generative AI for unstructured inputs and flexible reasoning — with tolerance for non-determinism as the deciding question.
bundle: ai-engineering
topic: ai-product-development
tags:
- agent-architecture
- product-management
- ai-native-business
resource: https://www.youtube.com/watch?v=_ZqSFVi6UDY
sources:
- id: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml
  resource: Resources/transcriptions/transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-05T09:00:00Z'
status: stable
related:
- ai-engineering/ai-product-development/ai-product-development-framework.md
- ai-engineering/model-fundamentals/large-database-models.md
- ai-engineering/agent-architecture/twelve-factor-agents.md
---

# Knowing When Not to Use AI — Agents vs Rules vs ML vs Humans

**Source:** [Knowing When Not to Use AI: AI Agents vs Rules vs ML](https://www.youtube.com/watch?v=_ZqSFVi6UDY) (IBM Technology, Sam Anthony)

---

## Summary

Most problems in software and data systems can be solved one of four ways: humans (judgment and accountability), rules/code (deterministic logic), machine learning (statistical pattern recognition), or generative AI (flexible reasoning and generation). These are not a ladder to progress through but distinct options with specific roles; choosing AI — and at what scale — is an engineering trade-off across accuracy, cost, complexity, and risk (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md). The thesis: most failures to reach production come not from bad models but from choosing the wrong system in the first place — the most important AI skill is knowing when not to use it (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md).

## The four options

| System | Best when | Examples | Trade-offs |
|---|---|---|---|
| **Humans** | High stakes, ownership/liability required, significant ambiguity | Hiring, medical diagnosis, legal interpretation, large-scale strategy/finance | High quality but expensive, slow, hard to scale |
| **Rules / code** | Logic is known and explicit ("if X do Y"), requirements stable, errors unacceptable | Payment processing, input validation, data transformations, security and access control | Fast, cheap, reliable, interpretable; breaks down when conditions constantly change or logic gets too complex |
| **Machine learning** | Patterns exist but aren't obvious; rules too complex to define; predictions needed | Fraud detection, churn prediction, demand forecasting, recommendations | Scales well, generally predictable; needs monitoring for model drift; low flexibility outside the defined scope; variable explainability |
| **Generative AI** | Inputs unstructured; interpretation/transformation needed; flexibility > precision; some error tolerated | RAG Q&A over documents, summarisation, code generation, multi-step agent workflows | Flexible and fast to value; non-deterministic, hard to test, correctness not guaranteed across runs, significantly costlier at scale |

(source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md)

## Illustrative boundaries

- If something goes wrong, accountability must sit with a real person — a hiring decision's final call involves nuance you don't fully automate; but no one manually reviews every credit-card transaction at millions per second (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md).
- Payment approval and email-format validation want code: deterministic, no room for error — AI here introduces unnecessary risk (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md).
- Static rules die against adaptive adversaries: "flag transactions over $1,000" quickly becomes useless as fraud behaviour evolves — that's the ML handoff (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md).
- ML forecasts demand better than intuition or simplistic rules, but can't write you a report on the societal causes of the trends — that interpretive layer is generative (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md).

## Hybrids and the deciding question

The key question for agents/LLMs: **how much non-determinism and uncertainty are you willing to tolerate?** (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md). A spending-analysis agent shouldn't be trusted to compute over thousands of transactions un-hallucinated — give it a calculator tool (code) for determinism, traditional ML for trend detection, and the LLM for the natural-language report. Hybrid systems tend to be the most successful; "make everything agents" is not literal — the best agents combine all four system types (source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md).

## The heuristic

- **Humans** — judgment, accountability, ethics, high-risk workflows.
- **Rules/code** — clearly defined logic that won't change often.
- **ML** — patterns and predictions in past data.
- **Generative AI** — interpret, generate, or reason over complex inputs where flexibility matters more than precision.

(source: transcript-knowing-when-not-to-use-ai-ai-agents-vs-rules-vs-ml.md)

## Key Takeaways

- Four distinct tools, not a maturity ladder: humans, rules, ML, generative AI — match the kind of intelligence to the problem.
- Tolerance for non-determinism is the gating question for agents and LLMs.
- The most successful AI systems won't be the ones using the most AI, but the ones consistently choosing the right system per decision.
- Great agents are hybrids: deterministic tools for math and guardrails, ML for patterns, LLMs for language and reasoning.

## Related

- [[ai-product-development-framework]] · [AI Product Development Framework](../ai-product-development/ai-product-development-framework.md) — the discovery/iteration discipline that sits upstream of this system-selection call
- [[large-database-models]] · [Large Database Models](../model-fundamentals/large-database-models.md) — companion IBM piece: what the ML quadrant looks like for structured enterprise data
- [[twelve-factor-agents]] · [Twelve-Factor Agents](../agent-architecture/twelve-factor-agents.md) — the same determinism-where-possible philosophy applied inside agent design

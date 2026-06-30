---
type: synthesis
title: |-
  The Three-Diamond Framework: Strategy, Discovery, and Delivery
  description: The AI product discourse is fixated on the third diamond — how to go from spec to code.
bundle: ai-engineering
topic: product-management
tags: [product-management, agent-workflows, ai-native-business, spec-driven-development]
source: ../../../Resources/Projects/articles-and-essays/001-the-last-third-of-product-development/001-the-last-third-of-product-development.md
resource:
timestamp: 2026-05-31T23:20:23Z
status: active
related:
  - ai-engineering/product-management/ai-and-product-strategy.md
  - ai-engineering/product-management/ai-prototyping-for-pms.md
  - ai-engineering/product-management/product-manager-as-builder.md
  - ai-engineering/product-management/unfair-pm-role.md
---

# The Three-Diamond Framework: Strategy, Discovery, and Delivery

**Source:** [001-the-last-third-of-product-development.md](../../../Resources/Projects/articles-and-essays/001-the-last-third-of-product-development/001-the-last-third-of-product-development.md)
**Author:** Max Manzini

---

## Summary

The AI product discourse is fixated on the third diamond — how to go from spec to code. But there are two earlier diamonds (strategy discovery and product discovery) where AI changes the economics just as profoundly, and almost nobody is talking about them. Each diamond is a diverge-then-converge cycle, producing one artefact that feeds the next. The three-diamond framework names those boundaries and explains why the cost of being wrong in the first two diamonds has collapsed.

## Why Diamonds and Why Three

A diamond is an expansion followed by a contraction: open up, explore widely, then prune to a decision. The classic double-diamond model from design thinking has two diamonds (problem and solution) (source: 001-the-last-third-of-product-development.md).

The AI era pushes this to three, and the **boundaries between them matter more than the diamonds themselves**. Each closure point is a clean handoff — everything converges, produces one artefact, and passes it to the next stage (source: 001-the-last-third-of-product-development.md):

1. **Strategy discovery** — asks *why*
2. **Product discovery** — asks *what and how*
3. **Product delivery** — builds it

The three are coupled. You cannot fix how you build (diamond 3) without rethinking how you decide what to build (diamond 2), because the cost of being wrong has collapsed and that changes the economics of deciding (source: 001-the-last-third-of-product-development.md).

## Diamond One: Strategy Discovery

The expansion phase is market analysis in the broad sense: who is competing, what business model fits, whether there is real demand or just a self-told story. Multiple lines of investigation run in parallel before contracting to one strategic approach (source: 001-the-last-third-of-product-development.md).

**The output is not a roadmap.** It is a Product Vision Canvas — a single page naming the vision, the target clients, the handful of features that actually move the business outcome, and the outcome itself. Deliberately not a delivery plan; its job is to carry intent forward without prematurely committing to execution (source: 001-the-last-third-of-product-development.md).

**Where AI already bites, quietly.** The expansion phase of strategy discovery used to be slow and expensive — people did less of it and committed earlier. With a capable agent doing competitive sweeps, pulling business-model comparisons and stress-testing demand signals, more branches can stay open for longer before pruning. Every time a stronger model lands, the strategy questions abandoned last quarter are worth re-running. That is a real shift in how the first diamond behaves (source: 001-the-last-third-of-product-development.md).

## Diamond Two: Product Discovery

**This is the diamond AI changes most, and the one the discourse skips almost entirely** (source: 001-the-last-third-of-product-development.md).

The Vision Canvas expands again into multiple, fundamentally different bets. Not variations on a theme — different interfaces, different interaction models, different backend shapes, different data models. Several prototypes in parallel. The principle is what counts (source: 001-the-last-third-of-product-development.md).

**None of this reaches production.** This is the failure mode that ruins everything downstream. The code generated here is throwaway by design: it exists to validate, nothing more. Each prototype is measured against success criteria set in advance, then run with real users to collect feedback against a defined idea of what good looks like. Bets that fail are pruned; bets that work expand into another round (source: 001-the-last-third-of-product-development.md).

**The value AI brings here is simple to state and large in effect.** You can try several genuinely different approaches, fast and cheap. The old constraint was that building a prototype good enough to test cost real time — so teams tested one or two ideas and called it discovery. Now you can test five. Three can be wrong, and being wrong costs almost nothing (source: 001-the-last-third-of-product-development.md).

**The output** is what currently lives in a product owner's head and a backlog: a roadmap, a set of specs, a prioritised list sharp enough that both the PO and the developers know what to build and how. This is the artefact that finally earns the word *spec* (source: 001-the-last-third-of-product-development.md).

**The trap:** a prototype that dazzled in the demo will fall over on launch if it is let through. The whole point of keeping diamond two sandboxed is that the thing validating the idea is not the thing shipping the idea. Confuse those two and you have shipped slop wearing a prototype's clothes (source: 001-the-last-third-of-product-development.md).

## Diamond Three: Product Delivery

Take the specs and the backlog and build — item by item — with proper testing, quality assurance, and evaluation at each step. Expand into the work, then contract onto a polished, releasable increment. First time round, that increment is the MVP. After that, it is the next version (source: 001-the-last-third-of-product-development.md).

The harness work, agent discipline, and guardrails against unreliable code all live here and are worth doing well. But delivery is the third diamond, not the first. A brilliant delivery engine pointed at the wrong thing gets you to the wrong place faster (source: 001-the-last-third-of-product-development.md).

## Feedback Loops

A framework that only flows forward is a slide deck. Feedback routes back to a different point depending on what kind it is (source: 001-the-last-third-of-product-development.md):

| Feedback type | Re-entry point | Mechanism |
|---|---|---|
| Bug | Just before diamond 3 | Agent harness picks up the report, fixes the code. No grand re-evaluation needed |
| Product / UX feedback | Diamond 2, if the vision holds | Check against the Vision Canvas first; if the vision still holds, open a new prototyping branch in diamond 2; don't restart, re-enter with a sharper question |
| New idea / strategic pivot | Diamond 1 | Full strategy pass. The expensive path — only take it when the feedback actually warrants an existential rethink |

**Backlogs sit across all of this.** A Jira or Notion or Azure board item is not a diamond-3 artefact. A backlog item can be a bug late in the flow, a spike in diamonds 1 or 2, or a feature in diamond 3. The tool does not decide which stage you are in — the nature of the work item does (source: 001-the-last-third-of-product-development.md).

## What This Means for the People Doing the Work

The PM and developer roles are merging. The old arrangement — PO handles stakeholders and discovery, hands a clean spec to developers who build it — assumed those were separate jobs done by separate people. When one person can carry an idea through strategy, prototype three versions over a weekend, and ship an increment with an agent doing the typing, the line stops being a line (source: 001-the-last-third-of-product-development.md).

The skills that survive are the ones the first two diamonds reward: figuring out what to build, figuring out whether it is any good, figuring out which problem is worth solving at all. The building is increasingly done for you (source: 001-the-last-third-of-product-development.md).

The obsession with diamond three is misplaced. The discourse has automated the part that was already the most mechanical, and is spending all its attention there — while the genuinely hard, genuinely human work has moved upstream to the two diamonds nobody is filming (source: 001-the-last-third-of-product-development.md).

## Key Takeaways

- Product work has three diamonds: strategy discovery (why), product discovery (what/how), product delivery (build it). The boundaries between them are the real value (source: 001-the-last-third-of-product-development.md).
- AI has collapsed the cost of both strategy research (diamond 1) and prototype-testing (diamond 2) — not just code generation (diamond 3). Most people are only talking about diamond 3.
- Diamond 2 prototypes must stay sandboxed and throwaway. The thing that validates the idea must not be the thing that ships the idea.
- Feedback routes back to the appropriate diamond based on type: bugs go to diamond 3, UX/product feedback to diamond 2, strategic pivots to diamond 1.
- The PM-developer boundary is dissolving; the surviving skills are upstream judgement, not downstream execution.

## Related

- [[ai-and-product-strategy]] — AI as strategy accelerator: where it helps, where it can't replace human judgment
- [[ai-prototyping-for-pms]] — practical tool selection and prompt patterns for AI-assisted prototyping in diamond 2
- [[product-manager-as-builder]] — the AI-enabled builder role and the diamond 3 / diamond 2 tension
- [[unfair-pm-role]] — tactics for the "super-IC" PM who now spans all three diamonds

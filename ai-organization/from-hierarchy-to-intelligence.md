---
type: synthesis
title: From Hierarchy to Intelligence
description: For 2,000 years, organizations have used hierarchy as an information-routing protocol to overcome the limit that one human can effectively manage 3-8 others.
bundle: ai-engineering
topic: ai-organization
tags: [ai-org-design, ai-native-business, multi-agent]
source: https://sequoiacap.com/article/from-hierarchy-to-intelligence/
resource: https://sequoiacap.com/article/from-hierarchy-to-intelligence/
timestamp: 2026-05-09T07:33:19Z
status: active
related: []
---

# From Hierarchy to Intelligence

**Source:** [Sequoia Capital](https://sequoiacap.com/article/from-hierarchy-to-intelligence/) (2026-03-31)
**Authors:** Jack Dorsey and Roelof Botha

---

## Summary

For 2,000 years, organizations have used hierarchy as an information-routing protocol to overcome the limit that one human can effectively manage 3-8 others. AI is the first technology powerful enough to actually replace what middle management does. Block is restructuring around this thesis: the **company itself becomes an intelligence** ("mini-AGI"), with humans pushed to the edge where they make contact with reality.

## A Brief History of Hierarchy

- **Roman Army** — contubernium (8) → century (80) → cohort (480) → legion (5,000). A nested information-routing protocol around the human "span of control" limit.
- **Prussian General Staff (1806)** — Scharnhorst and Gneisenau invented middle management: trained officers whose job was planning, processing information, coordinating across units. Created the line/staff distinction every corporation still uses.
- **American Railroads (1850s)** — West Point engineers brought military org thinking to private industry. McCallum (NY & Erie) drew the world's first org chart.
- **Frederick Taylor (early 1900s)** — Scientific Management optimized work *within* the hierarchy.
- **Manhattan Project (WWII)** — Oppenheimer's cross-functional teams worked, but were a wartime exception led by a singular figure.
- **McKinsey matrix (1959)** — Combined functional + divisional structures.
- **7-S framework (1970s)** — Hard Ss + soft Ss; structure alone insufficient.
- **Tech experiments** — Spotify squads, Zappos Holacracy, Valve flat structure. None scaled past the underlying constraint.

The constraint was always the same: narrowing span of control means more layers; more layers means slower information flow.

## What's Different Now

> *Most companies using AI are giving everyone a copilot, which makes the existing structure work slightly better without changing it. We're after something different: a company built as an intelligence.*

Block is questioning the underlying assumption that **humans must be the coordination mechanism**. AI is the first technology capable of actually performing the coordination functions that hierarchy exists to provide.

For this to work, a company needs:
1. A **world model** of its own operations
2. A **customer signal** rich enough to make that model useful

Block has both:
- **Remote-first**, so everything creates machine-readable artifacts (decisions, code, designs, plans)
- **Both sides of millions of transactions daily** — buyer (Cash App) + seller (Square) + operational data. Money is the most honest signal.

## The Four Pillars

Instead of product teams building predetermined roadmaps, you build:

### 1. Capabilities
Atomic financial primitives: payments, lending, card issuance, banking, BNPL, payroll. Hard to acquire (network effects, regulation). They have no UIs of their own — just reliability, compliance, and performance targets.

### 2. World Model
Two sides:
- **Company world model** — how the company understands itself; replaces the information that used to flow through management
- **Customer world model** — per-customer/merchant/market representation built from proprietary transaction data; evolves toward causal/predictive models

### 3. Intelligence Layer
Composes capabilities into solutions for specific customers at specific moments — proactively. Examples:
- Restaurant cash flow tightens before a seasonal dip → composes a short-term loan + adjusts repayment schedule + surfaces it before the merchant looks for financing
- Cash App user shows spending patterns of a move → composes new direct deposit + boosted-category card + savings goal

When the intelligence layer can't compose a solution because a capability doesn't exist, **that failure signal becomes the roadmap** — replacing PM hypotheses with customer reality.

### 4. Interfaces
Square, Cash App, Afterpay, TIDAL, bitkey, proto. Delivery surfaces. Important, but **not where the value is created.**

## The New Org: Three Roles

The intelligence lives in the system. People are at the **edge** — where the model makes contact with reality.

### Individual Contributors (ICs)
Build and operate capabilities, the model, the intelligence layer, and interfaces. Deep specialists. The world model gives them the context a manager used to provide.

### Directly Responsible Individuals (DRIs)
Own specific cross-cutting problems or customer outcomes. Example: "merchant churn in segment X for 90 days" with full authority to pull resources from any team. Persistent or rotating.

### Player-Coaches
Combine building with developing people. Replace traditional managers whose job was information routing. Still write code/build models/design — but invest in growing people. The world model handles alignment; DRIs handle priority; player-coaches handle craft and people.

**No permanent middle management layer.** Everything else the hierarchy did, the system coordinates.

## The Big Question

> *What does your company understand that is genuinely hard to understand, and is that understanding getting deeper every day?*

- **If the answer is nothing:** AI is just a cost-optimization story. Cut headcount, improve margins for a few quarters, get absorbed by something smarter.
- **If the answer is deep:** AI doesn't augment your company. It reveals what your company actually is.

Block's answer is the **economic graph**: millions of merchants and consumers, both sides of every transaction, financial behavior in real time. Compounds every second.

## Key Takeaways

- **Hierarchy is an information-routing protocol**, invented to work around the human span-of-control limit. AI is the first technology that can replace the routing function itself.
- **"Copilot for everyone" doesn't change anything** — it just makes the existing structure marginally faster
- The pattern requires two things: a **company world model** (operations as data) and a **customer world model** built from honest signal
- **Money is the most honest signal in the world** — Block's transaction visibility is what makes the customer model load-bearing
- The new org has three roles: ICs, DRIs (own outcomes), and player-coaches (replace managers as craft + people leaders)
- **Customer reality generates the backlog** — when the intelligence layer can't compose a solution, that failure becomes the next thing to build
- The hard question for any company: *what do you understand that's hard to understand, and is it getting deeper every day?*
- This pattern depends on the underlying agent infrastructure being production-grade — see [[twelve-factor-agents|Twelve-Factor Agents]] for the engineering principles that enable it

## See Also

- [[twelve-factor-agents|Twelve-Factor Agents]] — engineering principles for the production-grade AI systems this org model depends on
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — the kind of long-horizon agent infrastructure needed for autonomous coordination
- [[dorsey-mini-agi-podcast|Jack Dorsey — Every Company Can Now Be a Mini-AGI]] — Dorsey's own podcast walk-through of operationalising this article (40% RIF, three roles, four layers)
- [[openclaw-personal-agent-team|OpenClaw — A Personal Team of AI Agents]] — the personal-scale concrete preview of "team of agents" coordination
- [[anthropic-growth-takeaways|Anthropic Growth — Amol Avasare Takeaways]] — alignment as the residual human layer that AI doesn't touch
- [[claude-code-productivity-takeaways|Claude Code Productivity — Boris Cherny Takeaways]] — the productivity multiplier that makes the org shift economically forced
- [[product-job-market-2026|State of the Product Job Market 2026]] — empirical signal of how role mix is shifting in real time
- [[asha-sharma|Asha Sharma]] — AI products as organisms planned in seasons; post-training quality at scale
- [[howie-liu|Howie Liu]] — CEOs must become ICs again; fast-thinking vs slow-thinking group reorgs
- [[tomer-cohen|Tomer Cohen]] — diverge-then-converge bets and operating under uncertainty
- [[garrett-lord|Garrett Lord]] — audience as the only durable moat in the AI transition
- [[jeetu-patel|Jeetu Patel]] — timing, stamina, and the layers of the AI stack
- [[cat-wu|Cat Wu]] — timelines compressed; evals as the new PRD; product development inside Anthropic
- [[product-job-market-2025|State of the Product Job Market 2025]] — mid-2025 predecessor snapshot; the baseline before the 2026 acceleration

---
type: synthesis
title: RPI vs Other Frameworks
description: RPI is the accessible entry point to structured agentic workflows — low adoption cost, tool-agnostic, optimised for brownfield refactoring on solo or small teams.
bundle: ai-engineering
topic: rpi-methodology
tags:
- agent-workflows
- spec-driven-development
- multi-agent
- long-running-agents
- harness-engineering
generated:
  by: claude-code/atlas-consolidate
  at: '2026-04-29T21:54:50Z'
status: stable
related: []
---

# RPI vs Other Frameworks

**Source:** (self-authored)

## Summary

RPI is the accessible entry point to structured agentic workflows — low adoption cost, tool-agnostic, optimised for brownfield refactoring on solo or small teams. SDD is orthogonal (not a strict superset): RPI centres on discovery, SDD on specification, and they are complementary rather than competitive. The article provides head-to-head comparisons against SDD, Kiro Specs, Quick Dev, Vibe Coding, and Anthropic's two/three-agent harness patterns, with a feature matrix and illustrative cost table.

RPI is **a lightweight, immediate entry point** to structured agentic workflows. It is not the most comprehensive option; it is the most accessible. This page positions RPI against the alternatives.

## The Landscape

| Framework | Focus | Phases | Scope | Best For |
|---|---|---|---|---|
| **RPI** | Workflow simplicity | 3 | Solo / small teams | Brownfield refactors, feature additions |
| **SDD** | Durable specifications | 6 | Scaling organisations | Team/org standards, greenfield + brownfield |
| **Kiro Specs** | SDD + IDE integration | 6 | Teams on Kiro IDE | Same as SDD with tooling support |
| **Quick Dev** | Velocity with safety | 4 | High-switching teams | Fast iteration on trusted work |
| **Vibe Coding** | Exploration | Unstructured | Individuals prototyping | Learning, ideation, throwaway |

## RPI vs [[spec-driven-development/index|SDD]]

| Dimension | RPI | SDD |
|---|---|---|
| **Primary artifact** | Research and plan docs (workflow outputs) | The spec itself (durable, version-controlled) |
| **Phases** | 3 | 6 |
| **Scope of standard** | Workflow recipe | Standard with roles, boundaries, adoption levels |
| **Boundaries** | Implicit via FAR/FACTS gates | Explicit Always / Ask / Never per spec |
| **Tool coupling** | Tool-agnostic | Tool-agnostic by design |
| **Greenfield fit** | Weak (nothing to research) | Strong (brief → specify chain built for it) |
| **Brownfield fit** | Strong (research maps patterns) | Strong with delta semantics |
| **Adoption cost** | Low (folder + 2 commands) | Medium-high (training, governance) |
| **Contract H↔A** | Approved plan | The spec (Always/Ask/Never boundaries) |

### Are They Related?

**Yes, but orthogonal.** The "SDD is a strict superset of RPI" claim is **misleading** — they have different centers of gravity:

- **RPI** emphasises research and discovery (understanding what exists)
- **SDD** emphasises specification and standards (defining what should exist + how to verify)

**Complementary, not competitive.** A team running SDD benefits from RPI's research discipline during the Specify phase. A team running RPI benefits from SDD's structured spec format when formalising plans.

### When to Choose

- **RPI** — solo or small team, brownfield work, minimal governance, immediate adoption
- **SDD** — scaling organisation, mix of greenfield/brownfield, standardised H↔A contract, willing to invest in adoption
- **Both** — RPI for research + rapid planning, SDD for formalising standards across teams

## RPI vs Kiro Specs

Kiro Specs ≈ SDD with IDE integration.

| Aspect | RPI | Kiro Specs |
|---|---|---|
| Workflow | 3 phases, plain markdown | 6 phases, IDE-native UI |
| Spec format | Implicit in approved plans | Explicit SDD specs |
| Tool integration | Any agent | Kiro IDE |
| Adoption | Immediate | Requires Kiro |
| Best for | Individuals, small teams | Teams standardising on Kiro |

**Bottom line:** already on Kiro? Kiro Specs gives SDD with IDE conveniences. Otherwise RPI or plain SDD are more portable.

## RPI vs [[quick-dev|Quick Dev]]

Quick Dev optimises for **velocity over predictability** — minimises human-in-the-loop turns.

| Aspect | RPI | Quick Dev |
|---|---|---|
| **Optimisation** | Predictability, correctness | Velocity with safety boundary |
| **Human checkpoints** | Between every phase | Intent compression + final review |
| **Scope** | A workflow | A routing strategy |
| **Best for** | Complex refactors needing validation | Fast iteration on well-understood work |

**Hybrid approach:**
- Simple, well-scoped → Quick Dev (faster)
- Complex, risky → RPI (safer)
- Novel problems → iterative exploration (lighter)

## RPI vs Vibe Coding

| Aspect | RPI | Vibe |
|---|---|---|
| Structure | 3 phases + gates | None |
| Overhead | 30-45 min upfront | None |
| Error recovery | Checkpoints + mechanisms | Backtracking / rework |
| Best for | Production changes, refactors | Prototypes, learning |
| Token efficiency | Higher | Lower (trial and error) |

**Not either-or** — many developers use vibe coding to explore, then RPI once the approach is clear.

## Anthropic's Harness Patterns

### Two-Agent (Initializer + Coder)

| Component | Purpose |
|---|---|
| **Initializer** | Runs once: expands request to feature list, creates init script, sets up progress file |
| **Coder** | Runs every session: picks highest-priority incomplete feature, implements, verifies via browser automation, commits, updates progress |

**Best for:** building full apps iteratively across many sessions. See [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]].

### Three-Agent (Planner + Generator + Evaluator)

GAN-inspired. Planner expands prompt → Generator implements → Evaluator grades via browser automation + negotiates sprint contracts.

**Best for:** complex multi-hour app generation. See [[harness-design-long-running-apps|Harness Design for Long-Running App Development]].

### Comparison to RPI

| Aspect | RPI | Two-Agent | Three-Agent |
|---|---|---|---|
| Sessions | 1 per phase | Continuous (many coder sessions) | 3 agents orchestrated |
| Feature list | Not explicit | Explicit JSON | Full product spec |
| Verification | Tests, build, linter | Browser automation + tests | Browser + subjective grading |
| Scope | Single refactor/feature | Multi-day app development | Multi-hour app generation |
| Cost | Lower | Medium | Higher |

RPI is simpler and lighter. Harness patterns are for long-running projects where work spans many context windows.

## Feature Support Matrix

| Feature | RPI | SDD | Quick Dev | Vibe | Harness |
|---|---|---|---|---|---|
| Research phase | Yes | Optional | No | No | No |
| Spec/plan formalisation | Yes | Yes | Yes | No | Yes |
| Validation gates | Yes | Yes | No | No | Yes (evaluator) |
| Checkpoint recovery | Yes | Optional | No | No | Yes |
| Multi-session orchestration | No | No | Yes | No | Yes |
| Iteration support | Yes | Yes | Yes | Yes | Yes |

## Adoption Effort

| Framework | Setup | Training | Governance | Tools Required |
|---|---|---|---|---|
| **RPI** | 5 min | 1 hour | Light | Folder + markdown |
| **SDD** | 30 min | 4 hours | Medium | Specifications + roles |
| **Quick Dev** | 15 min | 2 hours | Light | Intent routing |
| **Vibe** | 0 | 0 | None | None |
| **Harness** | 1 hour | 6 hours | Heavy | Sub-agents, hooks, MCP |

## Illustrative Cost (10-file change)

| Framework | Research | Planning | Implementation | Total |
|---|---|---|---|---|
| **RPI** | $2 / 10 min | $1 / 5 min | $8 / 30 min | **$11 / 45 min** |
| **SDD** | $3 / 15 min | $2 / 10 min | $8 / 30 min | **$13 / 55 min** |
| **Quick Dev** | $0 / 0 min | $1 / 5 min | $8 / 30 min | **$9 / 35 min** |
| **Vibe** | $0 | $0 | $12 / 60 min (w/ rework) | **$12 / 60 min** |

*Illustrative; actual costs depend on model and complexity.*

## RPI's Position and Limitations

### Strengths

- **Immediate adoption** — no tooling, training, or governance overhead
- **Proven on real work** — case studies show zero rework on multi-file changes
- **Incremental** — adopt today, evolve to SDD as team scales
- **Any agent** — tool-agnostic

### Limitations

- **Workflow-first, not standard-first** — produces plans, not durable specs
- **Solo/small-team focused** — scales poorly to 20+ person orgs
- **No explicit boundaries** — FAR/FACTS are heuristics, not formal contracts
- **Brownfield-biased** — weak for greenfield

## Recommendations by Team Size

- **Solo:** Start with RPI. Minimal overhead, proven on brownfield. Graduate to SDD when formalising.
- **Small (3-10):** RPI for refactors + SDD principles for governance
- **Growing (10-30):** RPI + SDD hybrid (RPI for projects, SDD for org standards). Consider Kiro.
- **Large (30+):** Lead with SDD. RPI becomes one execution strategy within SDD's broader framework. Layer harness patterns for long-running work.

## Conclusion

RPI trades comprehensiveness for accessibility. If you need immediate results on a small team, RPI works. If you're scaling across an organisation, SDD is the higher-leverage investment. **Both can coexist.**

## Key Takeaways

- **RPI is the lightweight entry point** — not the endpoint
- **SDD is orthogonal**, not a superset — RPI centers on discovery, SDD on specification
- **Quick Dev optimises velocity**, RPI optimises predictability — hybrid routing works
- **Harness patterns** (two/three-agent) are for long-running work RPI doesn't address
- **Graduate as you scale** — RPI → SDD → harness, with hybrid approaches in between

## See Also

- [[spec-driven-development/index|SDD topic folder]] — the broader standard
- [[quick-dev|Quick Dev]] — velocity-optimised alternative
- [[adversarial-review|Adversarial Review]] — verification technique complementary to RPI
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — two-agent pattern
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — three-agent GAN pattern
- [[agentic-engineering-approaches-compared|Approaches Compared]] — the existing synthesis

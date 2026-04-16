# AI Product Development Framework

**Source:** Personal transcript (Massimiliano Manzini, 2026-04-16)

---

## Summary

A three-component framework for building better products with AI. The core premise: AI has accelerated production speed but has not improved product thinking. Most teams now skip Discovery → Prototyping → Validation and jump from idea to code. This framework reintroduces those phases without surrendering velocity.

The framework applies engineering-level patterns (already documented in [[agent-workflows/_index|Agent Workflows]] and [[harness-engineering/_index|Harness Engineering]]) one level up — to product decisions, prototyping, and architectural exploration.

---

## The Problem

Current AI-driven teams collapse the product development pipeline:

```
Idea → Code
```

Skipped phases:
- **Discovery** — understanding what problem to solve
- **Prototyping** — exploring how to solve it
- **Validation** — checking if the solution is right

Result: outputs that look complete but are shallow; systems that break under real-world complexity; expensive rework after significant investment.

---

## The Three Components

### 1. Sandbox Discovery — Exploration Before Commitment

The root cause of weak products is **premature convergence**: committing to the first plausible solution before the option space has been explored.

Steps:
1. Generate multiple distinct prototypes (different UX models, architectures, assumptions)
2. Evaluate with real users (preferred) or simulated users
3. Extract what works, what fails, and what trade-offs appear
4. Converge only after exploration — choose or combine approaches

Key rule: **If the build phase still feels exploratory, the discovery phase was too short.**

Related: [[research-plan-implement]] — the RPI Research phase is a direct instantiation of this principle for codebase changes. Also: product discovery methodology (Opportunity Solution Tree) validates the same thesis: wrong problem in → wrong solution out.

---

### 2. Adversarial Iteration — Structured Tension

AI systems optimise for plausibility, not correctness. Self-critique fails because the same reasoning that produced a flaw also evaluates it.

Two explicit roles:
- **Generator**: produces the output
- **Critic**: challenges it with a mandate to find problems

The loop:
1. Generator creates output
2. Critic evaluates — zero findings is a **halt**, not a pass
3. Findings return to Generator
4. Generator revises
5. Repeat until exit condition

Exit conditions (must define both):
- Maximum iteration count
- Convergence failure — if unresolved, **stop, output what is missing, surface unresolved issues explicitly**

Note: because the Critic is mandated to find problems, false positives are expected. **Human filtering is non-negotiable.**

Related: [[adversarial-review]] — the same pattern formally documented: mandatory findings, zero-findings halt, human filtering. Also: [[harness-design-long-running-apps]] — the GAN-inspired harness (planner / generator / evaluator) automates this separation at the infrastructure level.

---

### 3. Modular Context Navigation — Clarity at Scale

More context is not better context. Dumping a large codebase into a model's window reduces reasoning quality. The correct approach: provide the right context at the right level of detail.

Structure:
```
Index → Module → File → Detail
```

| Level | Contains |
|---|---|
| High-Level Index | System overview, key domains, entry points |
| Module Docs | Purpose, responsibilities, dependencies |
| Code References | Links to relevant files; no unnecessary detail |

Each level acts as a **context firewall** — sub-agents or sessions at one level do not load everything above and below.

Related: [[agentic-engineering-approaches-compared]] — "own your context window" is the single highest-leverage principle across all agentic approaches. [[twelve-factor-agents]] — modular context over full-codebase loading.

---

## How the Components Fit Together

| Component | Guards Against |
|---|---|
| Sandbox Discovery | Optimising the wrong thing |
| Adversarial Iteration | Accepting weak solutions |
| Modular Context Navigation | Inability to reason at scale |

Without discovery: you optimise the wrong problem.
Without adversarial loops: you accept the first plausible answer.
Without structured context: reasoning degrades as the system grows.

---

## Principles

1. **Exploration before optimisation** — commit to direction only after the space is explored
2. **Tension improves outcomes** — adversarial pressure is how you find what is actually wrong
3. **Iteration must be bounded** — define both a limit and an explicit failure exit
4. **Failure should be explicit** — surface what is missing; do not paper over it
5. **Context must be structured** — more context ≠ better reasoning
6. **Thinking and building are different phases** — conflating them forces the build to carry discovery weight

---

## Key Takeaways

- The problem is not AI speed — it is AI skipping the phases that make products good
- **Sandbox Discovery** prevents premature convergence by forcing option exploration before any production commitment
- **Adversarial Iteration** produces specific, actionable critique rather than rubber-stamp approval — zero findings is a halt
- **Modular Context Navigation** is a context firewall strategy, not just documentation
- These three components map to established patterns: [[research-plan-implement]], [[adversarial-review]], and sub-agent context management — applied at the product level rather than the code level

## See Also

- [[agent-workflows/_index|Agent Workflows]] — the engineering-level counterparts to each component
- [[research-plan-implement]] — RPI's Research phase as Sandbox Discovery for codebases
- [[adversarial-review]] — formal Critic mandate and human filtering rules
- [[agentic-engineering-approaches-compared]] — context ownership and sub-agent firewalls
- [[way-of-working/_index|Way of Working]] — complementary lens on how AI changes team structures

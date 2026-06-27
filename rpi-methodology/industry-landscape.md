---
type: synthesis
title: Industry Landscape
description: RPI is the atomic unit of work within a broader 2026 ecosystem where major frameworks — Anthropic's two/three-agent harness, OpenAI Agents SDK, Google ADK, Microsoft Agent Framework, CrewAI, and AWS Kiro — provide multi-agent orchestration infrastructure around it.
bucket: ai-engineering
topic: rpi-methodology
tags: [agent-architecture, multi-agent, harness-engineering, agent-workflows, spec-driven-development, long-running-agents]
source: self-authored
resource:
timestamp: 2026-04-29T21:54:50Z
status: active
related: []
---

# Industry Landscape

**Source:** (self-authored)

## Summary

RPI is the atomic unit of work within a broader 2026 ecosystem where major frameworks — Anthropic's two/three-agent harness, OpenAI Agents SDK, Google ADK, Microsoft Agent Framework, CrewAI, and AWS Kiro — provide multi-agent orchestration infrastructure around it. The industry has converged on explicit phases, context isolation, multi-agent coordination, and harness engineering as the primary reliability lever, though Gartner's 40% cancellation forecast reminds practitioners that structured workflows do not solve the fundamental economics problem of token costs scaling faster than value delivery.

RPI remains foundational to agentic engineering in 2026, but it occupies a distinct niche within a much broader ecosystem. **Key insight: RPI is the atomic unit of work; major frameworks provide orchestration infrastructure around it.**

## Anthropic: Harness Engineering

As the organisation behind the most-deployed model in production agentic systems, Anthropic's work focuses on the **infrastructure that makes agents reliable in production**.

### Two-Agent Pattern

| Agent | Role |
|---|---|
| **Initializer** | Runs once: expands high-level intent into comprehensive feature list (200+ for complex apps), creates `init.sh`, establishes `claude-progress.txt`, makes initial git commit. Feature list stored as JSON to resist model tampering |
| **Coding Agent** | Executes in repeated sessions: reads working directory state, inspects git history, reviews progress files, selects highest-priority incomplete feature, verifies with end-to-end tests, makes incremental progress, commits with descriptive messages |

### Three-Agent Pattern (March 2026)

Adds an **Evaluator Agent** — decoupled evaluation logic that prevents "context anxiety" where builder agents prematurely declare work complete. Evaluator operates in a fresh session with scoring criteria and few-shot examples, providing independent rating.

Mirrors the **Generator-Critic pattern** emerging across the industry. Separating generation from evaluation proves more tractable than making generators critical of their own output.

### Key Innovations

- **Context recovery** via git history + progress JSON enables rapid state reconstruction across context windows
- **One-shotting prevention** — feature lists force step-by-step development
- **Quality gates** via browser automation (Puppeteer) — agents must demonstrate working functionality, not just code existence
- **Managed Agents service** virtualises harness components (session, harness loop, sandbox)

### Relationship to RPI

Anthropic's harness engineering **implements RPI across multiple context windows**. Initializer ≈ research. Coding agent ≈ plan-execution. Evaluator ≈ verification. RPI phases distributed across *time*, not just phases of a single session.

See: [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]], [[harness-design-long-running-apps|Harness Design for Long-Running App Development]].

## OpenAI: Agents SDK

Released March 2025, replacing Swarm prototypes with a production-grade offering.

### Core Primitives

- **Agents** — LLM + instructions + tools, encapsulating a role or capability
- **Handoffs** — agent-to-agent delegation when one agent needs a different speciality
- **Guardrails** — input/output validation keeping agents in safe boundaries

### Two Orchestration Patterns

- **Manager Pattern** — central manager controls sub-agents as tools; decomposes and delegates
- **Handoff Pattern** — peer agents transfer control to each other; more decentralised

### Built-In

- **Tracing** — visualise and debug execution paths
- **Provider-agnostic** — works with OpenAI + 100+ other LLM providers
- **Clear primitives** — simple composable building blocks, not heavyweight frameworks

### Relationship to RPI

Provides **orchestration infrastructure** that *can support* RPI-like reasoning. Teams could compose research / planning / implementation agents using handoffs or manager patterns. SDK doesn't mandate RPI but enables it.

## Google: ADK Workflow Agents

Google's Agent Development Kit — **deterministic, composed multi-agent systems**.

### Core Execution Patterns

- **Sequential Pipeline** — Agent A → B → C (deterministic, auditable)
- **Loop Agent** — repeat until termination condition met
- **Parallel Agent** — concurrent execution of independent tasks
- **Generator-Critic** — generator drafts, critic validates against criteria
- **Human-in-the-Loop** — pause for human decision at key points

### Game Arena

Distinctive feature: **agents wargame each other in complex scenarios** to stress-test behaviour. Goes beyond correctness validation → tests behaviour under adversarial conditions. Particularly valuable for security and compliance.

### Relationship to RPI

Workflow composition enables **deterministic RPI at scale**. Sequential pipeline pattern directly implements research → plan → implement. HITL pattern mirrors HumanLayer's practice.

## Microsoft: Agent Framework

Merged AutoGen + Semantic Kernel into unified Agent Framework (October 2025, 1.0 in 2026).

- **Semantic Kernel foundations** — production primitives for LLM interaction
- **AutoGen orchestration** — multi-agent composition and coordination
- **Stable APIs** — .NET and Python with equivalent capabilities

Consolidation reflects **industry convergence on multi-agent orchestration as a core capability** — frameworks assume teams will compose multiple agents.

## CrewAI: Lightweight Multi-Agent

Independent, lean framework popular for coding tasks.

- **Crews** — specialised agents with specific goals, autonomous task delegation
- **Flows** — event-driven control with conditional branching
- **Performance** — 2-3x faster than comparable frameworks (vs. LangChain-based approaches)
- **Sophisticated memory** — short-term, long-term, entity, contextual layers

### Performance Overhead Reality Check

CrewAI exhibits significant **managerial overhead** — consuming **~3x the tokens of LangChain** even for single tool calls. **Framework choice itself materially impacts cost and latency**, independent of workflow design.

## McKinsey QuantumBlack: Specification-Driven

Promotes Specification-Driven Development — **explicit specifications as the primary artifact**.

### Core Philosophy

- Write clear requirements and user stories
- AI generates design and implementation plans from specs
- Formalise into discrete tracked tasks
- Implement following spec-guided task plans

### Relationship to RPI

If RPI is **research-based** (discover what exists, plan from discovery), SDD is **specification-based** (specify upfront, plan from specification).

- **RPI** excels in brownfield refactoring
- **SDD** excels in greenfield or spec-first work

SDD is arguably a **strict superset** of RPI's plan phase (which is driven by implicit research understanding), but they have different centers of gravity — see [[vs-other-frameworks|RPI vs Other Frameworks]] for the nuanced comparison.

## AWS Kiro: Enterprise Spec-Driven IDE

IDE built on SDD principles with production tooling.

### Three Phases

1. **Requirements** — user stories, acceptance criteria, bug analysis in structured notation
2. **Design** — technical architecture, sequence diagrams, implementation considerations (supports Requirements-First and Design-First)
3. **Implementation** — discrete trackable tasks with detailed plans and dependencies

### Agent Hooks

- **Pre-Task Execution** — setup scripts, prerequisite validation
- **Post-Task Execution** — tests, linting, external notifications

Enables automatic test sync, docs updates, code standardisation, git assistance.

### Positioning

Combines all three RPI-like phases in an IDE with explicit state preservation. Targets enterprise teams requiring durable specifications.

## Unified View: Multi-Agent Orchestration Patterns

A common set of patterns emerges across all frameworks:

| Pattern | Description | Use Case |
|---|---|---|
| **Sequential / Pipeline** | Agent A → B → C (linear flow) | Deterministic processing |
| **Orchestrator-Worker** | Central coordinator breaks work; workers execute independently | Parallel work on independent subproblems |
| **Generator-Critic** | Generator drafts → Critic (fresh context) reviews against spec | QA without self-validation bias |
| **Manager-Tool** | Manager decomposes; sub-agents are tools called by manager | Centralised decision-making with specialist delegation |
| **Peer Handoff** | Agents transfer control to each other | Decentralised routing by capability |
| **Human-in-the-Loop** | Explicit pause points for human judgment | Oversight at critical junctures |
| **Loop Agent** | Single agent iterating until termination | Iterative refinement, search, optimisation |
| **Ralph Loop** | Stateless invocation + external state, reset between iterations | Long-running work with extreme context isolation |

## RPI as the Atomic Unit of Work

The relationship becomes clear:

- **RPI operates at the tactical level** — how does one agent/pair solve a focused problem (refactor this module, implement this feature)?
- **Orchestration frameworks operate at the strategic level** — how do multiple agents with different specialisations coordinate to solve larger problems?

A team might implement:
- **Agent-level:** each agent follows RPI discipline
- **Orchestration-level:** generator-critic pattern where one agent runs RPI to produce code, another runs verification

Or:
- **Agent-level:** RPI across three sessions
- **Orchestration-level:** manager agent routes work to specialised RPI-implementing agents

## Points of Convergence

Across 15+ tools launched 2024-2025, the field has reached consensus on:

- **Explicit phases** > implicit vibe-based coding
- **Human engagement** at high-leverage points > passive post-hoc review
- **Context isolation** > accumulated context
- **Multi-agent systems** > single-agent for complex work
- **Verification is essential** — Generator-Critic or adversarial patterns catch failures single agents miss

## Where Agreement Breaks Down

Unresolved tensions:

| Tension | Camp A | Camp B |
|---|---|---|
| **Deterministic vs. Emergent** | McKinsey/QB (explicit state machines) | Twelve-Factor (agent-owned control flow) |
| **Specification vs. Discovery** | SDD (upfront spec required) | RPI (research-based) |
| **Context isolation vs. Coherent vision** | Atomic tasks prevent overload | Breaking work loses sight of the larger system |

## Convergence on Harness Engineering

Despite tensions, the industry converges on **harness engineering as the primary lever for reliability**. Rather than waiting for smarter models, teams systematically optimise:

- System prompts — clear, specific, minimal
- Tools — focused set with minimal overlap
- Context management — deliberate selection
- Sub-agents — focused-subtask isolation
- Verification — external critics, browser automation, test suites
- State management — persistent artifacts (git, progress files, feature lists) enabling recovery

**This shift from model-centric to harness-centric thinking may be the most significant change in agentic engineering in 2025-2026.**

## Gartner's Cost Reality

**40% of agentic AI projects will be cancelled by 2027**, primarily due to:
- Rising token costs
- Unclear value delivery
- Cost escalation POC → production (documented **717x scaling** in some cases)

**Orchestration complexity improves output quality but does not solve the fundamental economics problem.** The industry convergence around RPI and similar patterns may represent the most cost-effective approach currently available — but it doesn't eliminate the constraint that agentic systems are inherently token-expensive compared to traditional automation.

## Conclusion

RPI in 2026 is **no longer a standalone methodology** but a component within a broader ecosystem. Its strength: disciplined task decomposition and human alignment at leverage points. Its limitations emerge when scaled beyond brownfield refactoring.

Major frameworks — Anthropic's harness engineering, Google ADK, Microsoft Agent Framework, OpenAI Agents SDK, vendor platforms like Kiro — all implement similar principles with different emphasis and tooling. **Industry convergence suggests effective patterns have been identified.**

**Open question:** whether these patterns can deliver at enterprise scale given token costs and adoption friction.

## Key Takeaways

- **Anthropic's two/three-agent harness** implements RPI across multiple context windows
- **OpenAI Agents SDK** provides primitives (Agents, Handoffs, Guardrails) that can support RPI
- **Google ADK** directly implements RPI via Sequential Pipeline; has a unique Game Arena for adversarial stress-testing
- **McKinsey QuantumBlack** pushes SDD — spec-first alternative/complement to RPI's research-first
- **AWS Kiro** is SDD with IDE integration
- **CrewAI performance overhead** is ~3x LangChain — framework choice affects cost independent of workflow design
- **Industry converges on harness engineering** as the primary reliability lever in 2025-2026
- **Gartner 40% cancellation forecast** reminds us structured workflows don't fix token-cost economics

## See Also

- [[post-rpi-evolution|Post-RPI Evolution]] — how RPI itself evolved in parallel
- [[humanlayer-and-hitl|HumanLayer and HITL]] — HumanLayer's role in shaping the field
- [[vs-other-frameworks|RPI vs Other Frameworks]] — head-to-head comparisons
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic two-agent pattern
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — Anthropic three-agent / GAN pattern
- [[subagents-in-claude-code|Subagents in Claude Code]] — Anthropic's sub-agent primitive
- [[advisor-strategy|The Advisor Strategy]] — Anthropic's inverted orchestrator pattern
- [[spec-driven-development/index|Spec-Driven Development topic]]

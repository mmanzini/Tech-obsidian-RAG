# HumanLayer and Human-in-the-Loop

HumanLayer (founded by Dexter Horthy in 2024, Y Combinator X25 spring 2025) occupies a unique position: **not just an implementer of RPI, but the originator of the pattern, the discoverer of its limitations, and the architect of its evolution**. This page covers their methodological contributions, the ACE framework, 12-factor agents, the BAML case study, and the conflict-of-interest caveat.

## HumanLayer's Problem Statement

**Core insight:** AI coding tools struggle with real production codebases. Rather than waiting for smarter models, HumanLayer's hypothesis: the solution is **strategic context management**.

Horthy's specific problems preventing mainstream agent adoption:

- Engineers paste entire tickets into research tools and receive **opinions instead of facts**
- Planning prompts with 85+ instructions skip crucial interactive steps
- Frontier LLMs reliably follow only ~150-200 instructions — yet planning prompts alone contained 85+
- Expert agentic engineering practice **remained locked within individual practitioners**, didn't transfer to broader teams

Reframing: shift focus from **model capability** to **engineering discipline**.

> "The contents of your context window are the ONLY lever you have to affect the quality of your output."

LLMs are stateless → context quality directly determines output quality.

## Advanced Context Engineering (ACE) Framework

HumanLayer's systematic approach to managing AI agent context windows. Foundational principle: **context is a finite resource to be optimised ruthlessly**.

### Frequent Intentional Compaction (FIC)

- Structure development workflow around context management
- Target **40-60% context window utilisation** throughout the process
- Preserve buffer for unexpected complications

### Three-Phase ACE Workflow

Each phase has specific context-engineering goals (see [[workflow|Workflow]] and [[context-engineering|Context Engineering]]):

**Research** — fresh contexts to avoid polluting main agent's working memory. Output: research document, **document only, no opinions, no planning**.

**Planning** — precise implementation specs with detailed testing steps. **Strong plans prevent cascading errors that multiply through implementation.** Plans become the single source of truth for implementation. Human review prevents downstream compounding errors.

**Implementation** — execute incrementally with frequent verification checkpoints. After each: compact status updates back into the planning document; use the compacted plan as reference for the next phase. Prevents context drift across multiple windows.

### Context Engineering Priority Order

Three targets in **strict priority**:

1. **Correctness** (highest) — invalid information causes widespread failures
2. **Completeness** — missing context forces problematic assumptions
3. **Size** (lowest) — excessive tokens dilute signal

> Including slightly more correct information to ensure accuracy is preferable to omitting to save tokens. Incorrect information is categorically worse than either.

## Sub-Agents: Context Isolation Innovation

Parent agent owns the end-to-end plan; shells out specific phases to sub-agents:

- Each receives a **fresh messages array** (clean context window)
- Performs its specific task
- Returns only a **text summary** (not raw outputs)
- Summary incorporated back into parent's context

**Advantages:** prevents pollution, maintains coherence, enables specialisation, improves reliability, allows parallelisation.

**Backed by IFScale benchmark:** reasoning models maintain near-perfect performance until 250-300 instructions. Sub-agents with 40-60 instructions/invocation operate well within the reliable range.

## Human-in-the-Loop: Front-Loading Alignment

HumanLayer's HITL approach **differs fundamentally from post-hoc code review**. Rather than having humans review finished code, they front-load human involvement at **high-leverage points early in the workflow**.

### Design Discussion Phase

Before implementation, humans and agents align through **short design discussions (~200 lines)**:
- Discuss alternative approaches
- Surface applicable codebase patterns
- Let humans choose between deprecated and modern approaches
- Prevent downstream misalignment

### Structure Outline Phase

**~2-page structured outline:**
- Define implementation structure
- Create explicit checkpoints
- Establish verification criteria

### Why This Beats Post-Hoc Code Review

- Humans see high-leverage decisions **before coding starts**
- Agents incorporate feedback **before producing large amounts of code**
- Misalignment caught and corrected early

> "Magic prompts don't exist — human review at leverage points prevents downstream errors that multiply through implementation phases."

## The Ralph Loop Connection

HumanLayer incorporated principles from Geoff Huntley's Ralph Loop into ACE. Demonstrates that **context isolation beats context expansion**: larger windows often *degrade* performance because models struggle with instruction adherence in sparse contexts. See [[post-rpi-evolution|Post-RPI Evolution]].

## CodeLayer: The Commercial Product

HumanLayer's IDE built on Claude Code. Emphasises **keyboard-first interactions** for speed and control (Superhuman-style).

### Features

- **Keyboard-first workflows** — spawn Claude instances, route tasks, merge outputs, navigate entirely via keyboard
- **Multi-Claude parallel execution**
  - Distributed worktrees (different agents on different parts simultaneously)
  - Remote cloud workers (scaling beyond a single laptop)
  - Parallel optimisation (frontend + backend concurrently)
  - **Up to 50% token efficiency gains** through parallelisation
- **Battle-tested workflow templates**
  - Legacy code migration
  - Cross-service feature implementation
  - Bug triage across microservices
  - Dependency updates
  - Automatic rollback checkpoints, incremental validation, HITL approval gates for critical mods

## 12 Factor Agents

HumanLayer published "12 Factor Agents," inspired by 12-Factor App methodology. Core question: *"What principles build LLM-powered software good enough for production customers?"*

**Foundational insight:** successful AI products are **not purely agentic**. They're mostly software with **strategically placed LLM decision points**. Fastest path to quality is incorporating **small, modular concepts** from agent building into existing products.

### The 12 Factors

1. **Natural Language to Tool Calls** — convert user input to structured decisions
2. **Own Your Prompts** — direct control over prompt engineering
3. **Own Your Context Window** — manage what information reaches the LLM
4. **Tools as Structured Outputs** — tool calls as output formatting
5. **Unified State** — merge execution and business logic states
6. **Launch/Pause/Resume APIs** — simple operational control
7. **Human Contact via Tools** — escalate systematically
8. **Control Flow Ownership** — explicit decision logic over implicit routing
9. **Compact Errors** — keep failures concise for context efficiency
10. **Small, Focused Agents** — specialisation over generalisation
11. **Trigger Flexibility** — meet users across platforms
12. **Stateless Reducers** — clean, reproducible agent logic

**Shift:** from viewing AI as monolithic chatbots to viewing it as **precise tools orchestrated in broader software systems**. See [[twelve-factor-agents|Twelve-Factor Agents]] for the deep article.

## Instruction Budget Hypothesis — Evidence Assessment

### What HumanLayer Actually Claims

Their blog states frontier LLMs follow *"around 150-200 instructions with reasonable consistency"* — but provides **no original empirical research**. They reference external studies (IFScale).

### Academic Evidence

- **IFScale benchmark (2025):** reasoning models (o3, Gemini 2.5 Pro) maintain near-perfect performance until 250-300+ instructions. Standard models (GPT-4o, Claude Sonnet) show linear/exponential degradation. 150-200 is a reasonable **average** for standard models, not a hard ceiling.
- **Curse of Instructions study:** overall success = (success per instruction) ^ (number of instructions). If each is followed 90%, ten simultaneous yield only **35% overall success**.
- **ETH Zurich Agentfile study (2026):** LLM-generated context files *hurt* performance by 2-3% while adding 20% token cost. Developer-written files gave marginal 4% gains, also at 20% higher inference cost. Supports instruction budget hypothesis indirectly: unnecessary instructions waste tokens without benefit.

### Assessment: Partially Supported, Overstated

Better restated:

> "Standard frontier LLMs show measurable degradation in instruction following around 100-200 instructions, with reasoning models maintaining near-perfect performance to 250+ instructions. Exact threshold depends on model architecture, instruction complexity, and whether critical instructions are duplicated at prompt beginning and end."

**HumanLayer's contribution is not discovering the instruction budget** (earlier research had identified degradation) — it's **recognising the implications for workflow design**: rather than fighting the constraint, decompose to respect it.

## The BAML Case Study

HumanLayer's most significant published success:

- **Codebase:** BAML — 300,000-line Rust LLM structured output generation language
- **Developer:** approximately amateur-level Rust, **never touched BAML before**
- **Methodology:** ACE framework with RPI phases

### Timeline

- **Research & planning:** 3 hours
- **Implementation:** 4 hours
- **Total elapsed time:** 7 hours

### Outcomes

- **Phase 1:** fixed a bug approved by BAML maintainers
- **Phase 2:** added cancellation + WASM support
- **Scope:** **35,000 lines of production code shipped**
- **Comparison:** ≈ 3-5 days of senior engineer work
- **Quality:** all PRs passed expert review, merged by maintainers

### Critical Context

Impressive but limited. Demonstrates capability with **one codebase of one type** (Rust library), **one task class** (feature addition). **No case studies exist** for:
- Greenfield work
- Large architectural refactors requiring design decisions
- Performance optimisation / security hardening (non-deterministic)
- Teams where RPI discipline was not maintained
- Projects where RPI was slower than alternatives

**Proves** current models handle complex unfamiliar codebases and ship production code. **Does not prove** universal applicability or speed advantage.

## "No Vibes Allowed" Philosophy

> "AI agent-enabled coding is a deeply technical engineering craft, not magic words or vibes."

**Rejects:**
- Relying on "magic prompts" to fix broken workflows
- Vague handwaving about agent capabilities
- Treating agent engineering as an art form requiring intuition

**Advocates:**
- Systematic context management as the primary lever
- Decomposition respecting cognitive limits
- Explicit human engagement at high-leverage points
- Measurable validation frameworks (FAR, FACTS) replacing gut feeling

## Critical Note: Conflict of Interest

**HumanLayer has direct financial incentive to promote RPI and context engineering practices.** Their commercial offering (CodeLayer IDE) benefits from widespread adoption of RPI-style workflows.

Similar conflicts across the industry:
- **Kilo.ai** promotes RPI (lives on its platform)
- **Block** promotes RPI (via Goose slash commands)
- **Anthropic** promotes harness engineering (drives Managed Agents adoption)
- **AWS** promotes SDD (via Kiro IDE)

The contribution remains valuable — HumanLayer's insights are grounded in practice, and the RPI → CRISPY evolution addresses real observed failures. But **claims about universality, speed improvements, and cost-effectiveness should be evaluated against independent research**, not vendor assertions.

## Key Takeaways

- **HumanLayer coined context engineering** (Horthy, April 2025) — context quality > context size
- **ACE framework** emphasises Frequent Intentional Compaction, 40-60% target utilisation, 3-phase workflow
- **Priority order for context:** correctness > completeness > size (incorrect info is worst)
- **HITL front-loads alignment** (design discussion + structure outline) rather than post-hoc code review
- **12 Factor Agents:** AI products are software with strategically placed LLM decision points — not monolithic chatbots
- **BAML case study (35k LOC in 7 hours)** is impressive but singular — no published failure cases
- **Conflict of interest acknowledged** — HumanLayer, Kilo, Block, Anthropic, AWS all profit from promoting their preferred methodologies

## See Also

- [[Intelligence/Tech research/Wiki/rpi-methodology/principles|Principles]] — phase separation rationale
- [[context-engineering|Context Engineering]] — instruction budget and ACE mechanics
- [[post-rpi-evolution|Post-RPI Evolution]] — CRISPY/QRSPI response to failure modes
- [[industry-landscape|Industry Landscape]] — how other frameworks implement similar ideas
- [[twelve-factor-agents|Twelve-Factor Agents]] — the detailed article
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — HumanLayer's practitioner guide
- [[subagents-in-claude-code|Subagents in Claude Code]] — Anthropic's version of the sub-agent pattern

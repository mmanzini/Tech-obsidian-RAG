# Agentic Engineering & Harnessing — Approaches Compared

A synthesis of the main approaches to directing coding agents documented in this wiki, extended with 2026 industry research on how they apply to **greenfield** vs **brownfield** work. The goal: give a single place to decide *which workflow fits which job*.

## The Two Layers

The field splits into two orthogonal concerns:

1. **Harness engineering** — the *runtime* around the model: system prompts, tools, sub-agents, hooks, back-pressure, context isolation. See [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]].
2. **Agent workflows** — the *process* a human uses to direct the harnessed agent through a task: RPI, SDD, Quick Dev, two-agent / three-agent long-running patterns.

A workflow without a decent harness leaks context; a harness without a workflow produces confident nonsense. You need both.

## Harnessing Approaches

| Approach | Shape | Best when |
|---|---|---|
| **Single-agent harness** ([[skill-issue-harness-engineering]]) | One model + CLAUDE.md + skills + hooks + back-pressure | Daily coding, short-to-medium tasks, IDE workflows |
| **Two-agent initializer/coder** ([[effective-harnesses-long-running]]) | Initializer seeds a JSON feature list + `init.sh` + progress file; coder agent resumes one feature at a time | Greenfield long-running app builds spanning many sessions |
| **Three-agent GAN (planner/generator/evaluator)** ([[harness-design-long-running-apps]]) | External evaluator (Playwright MCP) grades each sprint; sprint contracts between generator and evaluator | Autonomous greenfield app-gen where quality is subjective (design, UX) |
| **Sub-agents as context firewall** | Parent sees only the sub-agent's result | Any long task — the single highest-leverage lever |

Core lessons (from [[skill-issue-harness-engineering]] and [[harness-design-long-running-apps]]): most failures are **configuration problems, not model problems**; every harness component encodes an assumption about model weakness, and those assumptions must be re-tested with every model upgrade.

## Workflow Approaches

| Workflow | Phases | Human checkpoints | Optimized for |
|---|---|---|---|
| **[[research-plan-implement|RPI]]** | Research → Plan → Implement (fresh session per phase, FAR + FACTS gates) | Between every phase | Predictability, correctness, multi-file refactors |
| **[[sdd-workflow|SDD]]** | Brief → Specify → Design → Task → Implement → Validate | At each checkpoint; Always/Ask/Never boundaries per spec | Durable intent, tool-agnostic, team/org scale |
| **[[quick-dev|Quick Dev]]** (BMad) | Compress intent → Route → Run → Review (triage) | Intent + final review only | Velocity on trusted execution |
| **[[kiro-specs|Kiro Specs]]** | Requirements → Design → Tasks (IDE-native) | Per phase | Same as RPI/SDD but baked into an IDE |
| **Vibe coding** | None | Continuous | Prototypes, throwaway spikes |

### How They Relate

- **RPI is the lightweight sibling of SDD.** Three phases vs six, thoughts/ folder vs a spec artifact, FAR/FACTS checklists vs Always/Ask/Never boundaries. RPI is a workflow; SDD is a *standard* — the spec itself is the durable, tool-agnostic artifact ([[sdd-overview]]).
- **Kiro Specs** is essentially RPI/SDD compiled into an IDE ([[kiro-specs]]).
- **Quick Dev** deliberately removes checkpoints that RPI adds — same problem, opposite bet on where the bottleneck lies.
- **The long-running harness patterns** ([[effective-harnesses-long-running]], [[harness-design-long-running-apps]]) apply a *similar phased structure* — planner/initializer, generator/coder, evaluator — at the **harness** level instead of the **human workflow** level. They automate what RPI asks a human to orchestrate.

## Greenfield vs Brownfield

This is the axis the wiki hasn't explicitly covered. Industry consensus in 2026 is that the two scenarios need different tools ([EPAM on brownfield spec-kit](https://www.epam.com/insights/ai/blogs/using-spec-kit-for-brownfield-codebase), [Augment Code — SDD for brownfield](https://www.augmentcode.com/guides/spec-driven-development-brownfield-codebases), [Hightower on SDD tool divergence](https://pub.spillwave.com/agentic-coding-gsd-vs-spec-kit-vs-openspec-vs-taskmaster-ai-where-sdd-tools-diverge-0414dcb97e46)).

### Greenfield

- No existing code to respect → the upfront investment in specification pays back cleanly.
- **Good fits:** SDD (Spec Kit, OpenSpec, Kiro), two-agent and three-agent long-running harnesses, BMad planner-first flows.
- **Risk:** over-specification — burning tokens designing a system you could discover by building.
- The Anthropic long-running harnesses ([[effective-harnesses-long-running]], [[harness-design-long-running-apps]]) are explicitly greenfield — they *generate* apps from a 1–4 sentence prompt.
- Dorsey's December 2025 threshold ([[dorsey-mini-agi-podcast]]) noted that *until then*, agents only really worked on greenfield prototypes.

### Brownfield

Brownfield is categorically different. Its failure modes:

- Generic init/specify templates don't reflect actual architecture, stack, or conventions — heavy manual customization required.
- Specs that describe "the world as it should be" without anchoring to the world as it *is* produce plans the codebase rejects.
- Changes are deltas, not greenfield docs — tools without delta semantics (ADDED / MODIFIED / REMOVED) drift.

What works for brownfield:

- **RPI's Research phase** is the single most brownfield-friendly primitive in this wiki. Its `find_files` / `analyze_code` / `find_patterns` sub-agents exist *specifically* to document what exists before planning anything. FAR validation (Factual / Actionable / Relevant) is the anti-hallucination gate brownfield most needs.
- **OpenSpec's delta markers** (ADDED / MODIFIED / REMOVED) are the brownfield-native extension of SDD.
- **Kiro Bugfix Specs** model *current vs expected vs unchanged* behavior explicitly — the right shape for brownfield regressions ([[kiro-specs]]).
- **BMad / AIDLC-style approaches** reportedly work *better* on brownfield than greenfield because their increments are naturally small-scope ([Hightower, 2026](https://pub.spillwave.com/agentic-coding-gsd-vs-spec-kit-vs-openspec-vs-taskmaster-ai-where-sdd-tools-diverge-0414dcb97e46)).
- **Twelve-factor agents' thesis** ([[twelve-factor-agents]]) — "drop modular concepts into existing products, not greenfield rewrites" — is itself a brownfield argument.
- Sub-agent firewalls ([[skill-issue-harness-engineering]]) matter more in brownfield because the surface area of potentially relevant context is enormous.

### Decision Table

| Scenario | Recommended stack |
|---|---|
| New product from scratch, long-running | Three-agent harness ([[harness-design-long-running-apps]]) **or** SDD brief+specify with a planner/coder split |
| New feature in existing product | **RPI** or **Kiro Specs** — structured enough for the refactor surface, lighter than full SDD |
| Legacy refactor / migration | **RPI** (Research phase is the point) **+** OpenSpec delta semantics **+** strong back-pressure (tests, typecheck) |
| Bug fix in legacy code | **Kiro Bugfix Spec** or RPI with scoped research |
| Small trusted change | **Quick Dev** |
| Throwaway prototype | Vibe |

## RPI vs SDD — Head to Head

| Dimension | RPI | SDD |
|---|---|---|
| **Primary artifact** | `thoughts/research/*.md` + `thoughts/plans/*.md` (workflow outputs) | The spec itself (durable, version-controlled, tool-agnostic) |
| **Phases** | 3 (Research → Plan → Implement) | 6 (Brief → Specify → Design → Task → Implement → Validate) |
| **Scope of the standard** | A workflow recipe | A *standard* with roles, boundaries, adoption levels |
| **Boundaries** | Implicit via FAR/FACTS gates | Explicit Always / Ask / Never per spec ([[sdd-roles-and-boundaries]]) |
| **Tool coupling** | Tool-agnostic (works with any agent that follows structured prompts) | Tool-agnostic *by design* — same spec must run across Kiro / Claude Code / Cursor |
| **Loop-back semantics** | Iterate slash command patches plan surgically | Looping back to Specify/Design is the workflow working correctly |
| **Sweet spot** | Solo dev / small team doing multi-file refactors | Teams or orgs where specs become the interface between humans and agents |
| **Greenfield fit** | OK — but you'll skip/shrink the Research phase | Strong — the brief→specify→design chain is built for it |
| **Brownfield fit** | **Strong** — Research phase is purpose-built | Strong *only if* paired with delta semantics (OpenSpec) or explicit current/expected/unchanged modeling |
| **Adoption cost** | Two slash commands + a `thoughts/` folder | Ranges from Level 1 (one-page brief) to Level 4 (org-wide standard) |

**Short version:** RPI is a workflow you can adopt this afternoon; SDD is a contract you adopt incrementally across a team. They are not competitors — a team running SDD internally still benefits from RPI's Research discipline during the Specify phase, and a solo dev running RPI is doing a low-ceremony subset of SDD.

## Cross-Cutting Principles

Every approach above converges on the same handful of truths:

- **Own your context window.** Sub-agents, progressive disclosure, context resets, fresh sessions per phase.
- **Separate generation from evaluation.** External evaluators > self-critique. Same reason [[adversarial-review]] works.
- **Make intent durable.** Whether as a spec, a plan file, a JSON feature list, or a progress file — intent must survive context compaction.
- **Back-pressure is the highest-leverage investment.** Typechecks, tests, Playwright runs — agents need to verify themselves.
- **Re-examine the harness when the model changes.** Harness components encode assumptions that expire ([[harness-design-long-running-apps]] stripped sprints on Opus 4.6).
- **Greenfield rewards planning; brownfield rewards research.** The same phased scaffolding serves both, but the center of gravity moves.

## Key Takeaways

- Two orthogonal layers: **harness** (runtime config) and **workflow** (human process). Pick from both.
- **RPI = lightweight workflow; SDD = durable standard; Quick Dev = velocity bet; long-running harnesses = automation of the same phased idea inside the harness.**
- **Greenfield** is where planner-first SDD and long-running harnesses shine.
- **Brownfield** is where RPI's Research phase, OpenSpec-style deltas, and Kiro Bugfix Specs earn their keep — generic greenfield templates actively hurt here.
- The approaches are composable: a team can run SDD at the org level while individual devs run RPI inside a single spec's Implement phase, all powered by a sub-agent-heavy harness.
- See [[ai-product-development/ai-product-development-framework|AI Product Development Framework]] for how these same principles (exploration before commitment, adversarial tension, context firewalls) apply one level up — at the product and prototyping layer rather than the code generation layer.

## Sources

### Wiki articles referenced
- [[research-plan-implement|Research → Plan → Implement (RPI)]]
- [[rpi-methodology/vs-other-frameworks|RPI vs Other Frameworks]]
- [[sdd-overview|SDD Overview]]
- [[sdd-workflow|SDD Workflow]]
- [[sdd-roles-and-boundaries|SDD Roles and Boundaries]]
- [[quick-dev|Quick Dev]]
- [[adversarial-review|Adversarial Review]]
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]]
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]]
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]]
- [[twelve-factor-agents|Twelve-Factor Agents]]
- [[kiro-specs|Kiro Specs]]
- [[kiro-hooks|Kiro Hooks]]
- [[kiro-steering|Kiro Steering]]
- [[dorsey-mini-agi-podcast|Dorsey — Mini-AGI Podcast]]

### External sources (2026 research)
- [Vishal Mysore — SDD Is Eating Software Engineering: Map of 30+ Agentic Coding Frameworks (Mar 2026)](https://medium.com/@visrow/spec-driven-development-is-eating-software-engineering-a-map-of-30-agentic-coding-frameworks-6ac0b5e2b484)
- [EPAM — Using Spec-Kit for Brownfield Codebases](https://www.epam.com/insights/ai/blogs/using-spec-kit-for-brownfield-codebase)
- [Augment Code — Spec-Driven Development for Brownfield Codebases](https://www.augmentcode.com/guides/spec-driven-development-brownfield-codebases)
- [Augment Code — What Is Spec-Driven Development?](https://www.augmentcode.com/guides/what-is-spec-driven-development)
- [Augment Code — 6 Best Spec-Driven Development Tools for AI Coding in 2026](https://www.augmentcode.com/tools/best-spec-driven-development-tools)
- [Rick Hightower — GSD vs Spec Kit vs OpenSpec vs Taskmaster AI: Where SDD Tools Diverge (Feb 2026)](https://pub.spillwave.com/agentic-coding-gsd-vs-spec-kit-vs-openspec-vs-taskmaster-ai-where-sdd-tools-diverge-0414dcb97e46)
- [GitHub — spec-kit](https://github.com/github/spec-kit)

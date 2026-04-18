# Research → Plan → Implement (RPI)

> **Deep reference:** see [[rpi-methodology/_index|RPI Methodology]] topic for principles, FAR/FACTS gates, context engineering, tool-agnosticism, team adoption, CRISPY/QRSPI evolution, and industry landscape. This page is the one-page overview.

**Sources:**
- [Block Goose tutorial](https://block.github.io/goose/docs/tutorials/rpi/)
- [path.kilo.ai framework guide](https://path.kilo.ai/introduction/patterns/rpi/)
- Originally introduced by [HumanLayer](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)

---

## Summary

RPI is a three-phase framework that replaces direct prompting ("just refactor this") with structured phases: **research what exists, plan the change, implement mechanically**. It trades speed for predictability, correctness, and dramatically reduced rework. Each phase happens in a fresh session to keep the LLM laser-focused on one goal.

## The Problem with Direct Prompting

When you ask an AI to implement something without structure, you're gambling on several things going right at once:
- AI correctly understands intent
- AI discovers all relevant code/patterns
- AI makes architectural decisions you'd agree with
- AI doesn't hallucinate APIs
- AI stays in scope

What actually happens: context overflow, hallucination, wrong problem solved, scope creep, untestable code.

## The Three Phases

### Phase 1: Research
**Goal:** Build context. Document what exists today. No opinions, no suggestions, no critique, no planning.

In Goose, `/research_codebase "topic"` spawns three parallel sub-agents:

| Sub-agent | Purpose |
|-----------|---------|
| `find_files` | Codebase locator — find relevant files |
| `analyze_code` | Read files fully and document how they work |
| `find_patterns` | Find similar features/conventions in the repo |

**Output:** A structured doc at `thoughts/research/YYYY-MM-DD-HHmm-topic.md` with git metadata, file/line references, flow descriptions, key components, and open questions.

#### FAR Validation
| Criterion | Description | Prevents |
|-----------|-------------|----------|
| **F**actual | Based on actual code, not assumptions | Hallucination |
| **A**ctionable | You know exactly what to build | Vague requirements |
| **R**elevant | Solves the real user need | Wrong problem solved |

A human must review the research before proceeding.

### Phase 2: Plan
**Goal:** Translate research into a phased implementation plan with success criteria.

The AI:
1. Reads the research doc
2. Asks **clarifying questions** (full removal vs deprecation? config cleanup behavior?)
3. Presents design options where multiple approaches exist
4. Produces a phased plan

**Output:** `thoughts/plans/YYYY-MM-DD-HHmm-description.md` with explicit phases, exact file paths, code snippets, success criteria, manual verification steps, and **checkboxes for tracking progress**.

#### FACTS Validation (per task)
| Criterion | Description | Prevents |
|-----------|-------------|----------|
| **F**easible | Can be done with available tools | Impossible tasks |
| **A**tomic | Single focused unit | Context overflow, scope creep |
| **C**lear | Unambiguous instructions | Misinterpretation |
| **T**estable | Has clear success criteria | Untestable code |
| **S**coped | Properly bounded | Runaway execution |

The plan must be explicit enough that a fresh AI session could execute it without additional context.

### Phase 3: Implement
**Goal:** Execute mechanically. If it feels creative, something upstream is missing.

The AI:
1. Reads the plan completely
2. Executes phases in order
3. Runs verification after each phase (tests, builds, lints)
4. Updates checkboxes directly in the plan file

**Recovery mechanism:** If context fills mid-implementation, the checkboxes let the AI compact and resume exactly where it left off.

### Optional: Iterate
`/iterate_plan "plan path" + feedback` — researches only what changed, edits the plan surgically without starting over.

## File Structure

```
thoughts/
├── research/
│   └── YYYY-MM-DD-HHmm-topic.md
└── plans/
    └── YYYY-MM-DD-HHmm-description.md
```

## Real-World Example: Removing a Feature from Goose

Removing the "Tool Selection Strategy" feature — touched 32 files across Rust, TypeScript, config, tests, and docs.

| Phase | Time |
|-------|------|
| Research | 9 min |
| Plan | 4 min |
| Implement | 39 min |
| **Total** | **52 min** |

PR built on first try. Code Review Agent had zero comments. The author "fell asleep" during implementation — that's the design intent.

## When to Use RPI

**Good for:** Refactors, migrations, feature additions, large upgrades, incident cleanup, documentation overhauls — anything spanning multiple files with high cost of errors.

**Skip for:** Single-file changes, obvious bug fixes, quick prototypes. The validation overhead isn't worth it for trivial work.

## Why It Works

| Failure mode | RPI prevention |
|-------------|----------------|
| Context overflow | Atomic tasks, fresh session per phase |
| Hallucination | FAR validation requires factual evidence |
| Wrong problem solved | Research validates relevance first |
| Untestable code | FACTS requires clear success criteria |
| Scope creep | Validation gates maintain boundaries |

## Key Takeaways

- **One goal per session** — start fresh between phases to keep the LLM focused
- **Research has strict rules:** document only, no opinions, no planning
- **Plans must be self-contained** — a fresh session needs to execute them without help
- **Atomic tasks + checkboxes enable graceful recovery** when context fills
- **Implementation should feel boring.** If it feels creative, the upstream phase was weak.
- The mindset shift: *not* "AI, build this feature" but "AI, help me understand what exists, plan systematically, then execute with validation"
- Tool-agnostic — works with Claude, Copilot, Cursor, any agent that follows structured prompts

## See Also

- [[quick-dev|Quick Dev]] — BMad's approach to minimizing human turns while preserving quality checkpoints
- [[adversarial-review|Adversarial Review]] — review technique that complements RPI's verification gates
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's analogous initializer/coder pattern
- [[twelve-factor-agents|Twelve-Factor Agents]] — broader principles RPI embodies (small focused agents, owning your context window)
- [[kiro-specs|Kiro Specs]] — Kiro IDE's automation of a similar requirements → design → tasks workflow
- [[ai-product-development/ai-product-development-framework|AI Product Development Framework]] — applies RPI's Research phase principle (explore before committing) one level up, at the product and prototyping layer

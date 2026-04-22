# Workflow

The full three-phase RPI lifecycle with deliverables, validation gates, and recovery mechanisms.

```
Research → Plan → Implement → (Iterate)
   ↓        ↓        ↓
 FAR     FACTS   Verification
review   review    gates
```

## Phase 1: Research (5-15 min)

**Objective:** Document what exists today, **no opinions, no solutions**.

### Sub-Agent Pattern

Three parallel sub-agents operate independently, then merge results:

| Sub-Agent | Role | Output |
|---|---|---|
| **find_files** | Locate relevant files | File inventory with paths and purposes |
| **analyze_code** | Read files thoroughly | Logic, data flow, key functions |
| **find_patterns** | Identify similar features / conventions | Pattern inventory, naming conventions |

### Reverse Prompting

The AI asks **clarifying questions one at a time** instead of requiring comprehensive upfront specification. Each answer guides the next direction — surfaces assumptions progressively.

### Deliverables

Markdown file at `thoughts/research/YYYY-MM-DD-HHmm-topic.md` containing:
- Git metadata (branch, recent commits, tags)
- Complete file reference list with paths and line numbers
- Flow diagrams, component descriptions
- Pattern analysis (how similar features are implemented)
- Open questions and unresolved assumptions
- **No opinions, no architectural critique, no implementation suggestions**

### FAR Validation (before planning)

See [[validation-gates|Validation Gates]]. Human review: Factual (≥ 4.0), Actionable (≥ 3), Relevant (≥ 3).

## Phase 2: Plan (5-10 min)

**Objective:** Design change with explicit success criteria and atomic task breakdown. Produce a **self-contained plan** a fresh session can execute.

### Plan Structure (`thoughts/plans/YYYY-MM-DD-HHmm-description.md`)

Numbered phases (typically 6-15), each with:
- Exact file paths + line numbers
- Code snippets (before/after for non-trivial changes)
- Checkboxes `- [ ]` for progress tracking
- Success criteria (tests, build, linter, manual verification)

```markdown
# Phase 1: Update Database Schema
- [ ] Create migration script
- [ ] Apply schema changes
- Verification: migration runs without errors
```

### Atomic Task Principle

Each task must be:
- **Singular** — one focused piece of work
- **Completable** — in one step without context switching
- **Verifiable** — success is measurable
- **Independent** — validatable in isolation (dependencies explicit)

Prevents context overflow mid-implementation, enables checkpoint recovery.

### FACTS Validation (before implementation)

See [[validation-gates|Validation Gates]]. Human review: Feasible, Atomic, Clear, Testable, Scoped.

## Phase 3: Implement

**Objective:** Execute mechanically. **Implementation should feel boring** — if creative, upstream is weak.

### Execution Loop

1. Read the complete plan before starting
2. Verify prerequisites met
3. Execute phases sequentially
4. After each phase: run verification → update checkbox `- [x]` → review criteria → proceed or stop

### Recovery via Checkboxes

If context fills mid-implementation:
- New session reads the plan
- Identifies completed phases (marked `[x]`)
- Resumes from first incomplete phase
- Continues verification from checkpoint

### Three Verification Strategies

| Strategy | When | Control |
|---|---|---|
| **Task-by-task** | High risk, low confidence, complex code | Maximum — human reviews after each task |
| **Phase-by-phase** | Medium risk, clear plan | Balanced |
| **Full plan** | Low risk, high confidence, well-tested patterns | Minimal interruption |

## Phase 4: Iterate (optional)

`/iterate` accepts plan path + feedback, then:
1. Researches only what changed (not the entire codebase)
2. Updates plan surgically — no rebuild, no lost checkboxes
3. Preserves prior progress, incorporates feedback

## Directory Structure

```
thoughts/
├── research/
│   └── YYYY-MM-DD-HHmm-topic.md
└── plans/
    └── YYYY-MM-DD-HHmm-description.md
```

**Naming:** `YYYY-MM-DD-HHmm-descriptive-name.md` → chronological sort, multi-artifact-per-day support.

## Case Study: Goose Tool Selection Strategy Removal

Rust/TypeScript/config codebase, 32 files affected:

| Phase | Time | Details |
|---|---|---|
| Research | 9 min | 3 sub-agents found all 32 files, dependencies, removal scope |
| Planning | 4 min | Broke into 6 atomic phases with success criteria |
| Implementation | 39 min | All phases with verification; zero build failures |
| **Total** | **52 min** | Zero rework, zero review comments, no regressions |

- Setup (R+P): 13 min (25%)
- Execution (I): 39 min (75%)
- Rework cycles: **0**
- Build success on first attempt: 100%

Comparison: a senior engineer doing this manually would likely take 2-4 hours plus build/debug cycles + 1-2 hours of code review.

## Key Takeaways

- **Research produces a FAR-validated fact document**; no opinions allowed
- **Plan produces a FACTS-validated, self-contained, atomic-task spec**; 6-15 phases is the sweet spot
- **Implementation is mechanical execution** with verification after each phase
- **Checkboxes enable recovery** — context exhaustion is no longer catastrophic
- **Iterate is surgical, not rebuild** — preserves completed work

## See Also

- [[Intelligence/Tech research/Wiki/rpi-methodology/principles|Principles]] — why phase separation works
- [[validation-gates|Validation Gates]] — FAR and FACTS in detail
- [[tool-agnosticism|Tool Agnosticism]] — Goose commands vs. manual prompting
- [[getting-started|Getting Started]] — worked example
- [[subagents-in-claude-code|Subagents in Claude Code]] — how Claude Code surfaces subagent invocation

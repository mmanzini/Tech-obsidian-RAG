# Glossary

**Source:** (self-authored)

## Summary

A reference for the key terms used throughout the RPI methodology documentation, covering foundational concepts (context engineering, instruction budget, context rot), RPI-specific constructs (FAR/FACTS scales, validation gates, checkpoint recovery, thoughts directory), and related frameworks (SDD, CRISPY/QRSPI, Ralph Loop, 12 Factor Agents). Definitions include cross-references to the relevant deep-dive articles.

Key terms used throughout the RPI methodology documentation.

**ACE Framework** — Advanced Context Engineering. HumanLayer's systematic methodology for managing AI agent context. Emphasises that context *quality* (selection, priority, structure) matters more than context *size*. See [[humanlayer-and-hitl|HumanLayer and HITL]].

**Adversarial Review** — Forced-reasoning review where the reviewer must find issues, not just approve. Zero findings triggers a halt. Produces actionable findings rather than generic approval. See [[adversarial-review|Adversarial Review]].

**Atomic Task** — A single focused unit of work completable in one context window. "Add type annotation to function X" — not "refactor the authentication system." Prevents context overflow; enables checkpoint recovery.

**Checkpoint Recovery** — Ability to resume work exactly where interrupted. In RPI, markdown checkboxes in the plan file track completion state. A new session reads the plan, identifies `[x]` phases, resumes from the first incomplete one.

**Context Engineering** — Systematic optimisation of information within an AI agent's context window to maximise output quality. Coined by Dexter Horthy (HumanLayer), April 2025. Core insight: quality > size. See [[context-engineering|Context Engineering]].

**Context Rot** — Output quality degrades as context window fills. Causes: position bias, attention dilution, instruction adherence loss. Severity increases past 40-60% utilisation.

**CRISPY / QRSPI** — Evolution of RPI: 7 phases (Questions → Research → Structure → Plan → Implement → Worktree → Implement, optionally + Reflect). Developed by HumanLayer to address: 1,000-line plans humans didn't read, monolithic 85+ instruction prompts. Each stage < 40 instructions. See [[post-rpi-evolution|Post-RPI Evolution]].

**FACTS Scale** — Plan validation framework: **F**easible, **A**tomic, **C**lear, **T**estable, **S**coped. Each dimension scored 0-5; target ≥ 3 for all. See [[validation-gates|Validation Gates]].

**FAR Scale** — Research validation framework: **F**actual, **A**ctionable, **R**elevant. Targets: Factual ≥ 4.0, Actionable ≥ 3, Relevant ≥ 3. See [[validation-gates|Validation Gates]].

**Fresh Context** — A new session with a clean context window, no accumulated history. RPI uses fresh contexts per phase to prevent pollution. See also: session isolation.

**Harness / Harness Engineering** — Runtime configuration around an AI model: instructions, tools, context management, verification hooks, back-pressure. Most agent failures are harness problems, not model problems. See [[skill-issue-harness-engineering|Skill Issue]].

**Instruction Budget** — Hard constraint on how many instructions a frontier LLM can reliably follow simultaneously. ~150-200 for standard models, ~250-300 for reasoning. **Independent of window size** — doubling tokens doesn't double capacity. Drove HumanLayer's evolution from RPI to CRISPY.

**Ralph Loop** — Extreme context isolation: run an agent in a loop with a simple declarative prompt; reset entirely between iterations, carrying state through files. Named after Geoff Huntley's "Ralph Wiggum as a software engineer" concept. Demonstrates context resets can outperform long-running sessions.

**Reverse Prompting** — Technique where the AI asks clarifying questions one at a time rather than requiring upfront specification. Each answer guides the next research direction. Surfaces assumptions progressively.

**RPI** — Research → Plan → Implement. Three-phase structured framework for AI-assisted development. Each phase in a fresh context. Originally HumanLayer; adopted by Block (Goose) and Kilo.

**SDD** — Specification-Driven Development. Broader framework than RPI, 6 phases: Brief → Specify → Design → Task → Implement → Validate. Emphasises durable, version-controlled specs as contracts between humans and agents. See [[vs-other-frameworks|RPI vs Other Frameworks]] and [[spec-driven-development/_index|Spec-Driven Development]].

**Session Isolation** — Running each phase (or subtask) in a separate session with a fresh context window. RPI's phase separation is session isolation applied to the workflow layer.

**Sub-Agent** — An autonomous agent spawned for a specific task in its own fresh context. Returns only a summary to the parent. Used for research (find_files, analyze_code, find_patterns) or high-risk implementation isolation. See [[subagents-in-claude-code|Subagents in Claude Code]].

**Target Utilisation** — Recommended context window fill: 40-60%. Maintains quality while preserving buffer. HumanLayer warns at 100k tokens (10% of 1M window) to stay in range.

**Thoughts Directory** — Project-local `thoughts/` folder containing RPI artifacts. Subdirs: `research/`, `plans/`. Enables version control and institutional knowledge accumulation. Uses `YYYY-MM-DD-HHmm-topic.md` naming.

**Validation Gate** — A checkpoint requiring human review before proceeding. RPI has two: FAR after research, FACTS after planning. Gates catch errors upstream.

**Verification Gate (Implementation)** — A checkpoint during implementation where automated tests, build, linter, or manual steps verify a phase succeeded. Tracked via plan checkboxes.

**Workability** — Term from McKinsey's agentic patterns. The degree to which implementation details can be deferred to later phases. RPI's planning phase must produce **low-workability** designs (specific, unambiguous, ready to execute).

## Key Takeaways

- Instruction budget (~150-200 for standard models, ~250-300 for reasoning) is independent of context window size — this distinction drives the evolution from RPI to CRISPY
- Context rot begins accelerating past 40-60% window utilisation; session isolation per phase is the primary mitigation
- FAR (Factual/Actionable/Relevant) validates research; FACTS (Feasible/Atomic/Clear/Testable/Scoped) validates plans — both gates require human sign-off before proceeding
- The Ralph Loop (full context reset between iterations, state persisted via files) can outperform long-running sessions for sustained agentic work
- Harness failures (runtime config, tool selection, verification hooks) account for most agent failures — not model capability

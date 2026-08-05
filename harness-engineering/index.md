# Harness Engineering

How to configure, design, and optimize the runtime environment ("harness") around AI coding agents — especially for long-running, multi-session tasks. Covers harness theory and design, plus the instruction mechanics that steer an agent inside one (CLAUDE.md, skills, hooks, subagents, verification loops). The execution substrate those harnesses run on — managed agents, sandboxes, orchestration runtimes, browser/computer tooling — lives in `agent-infrastructure`.

## Articles

- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — Practical guide to CLAUDE.md, MCP servers, skills, sub-agents, hooks, and back-pressure
- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence — 3 Key Patterns]] — Use what Claude knows, ask what you can stop doing, set boundaries carefully (Anthropic)
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's two-agent pattern (initializer + coder) for multi-context-window work
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — GAN-inspired planner/generator/evaluator architecture for autonomous full-stack builds
- [[nlh-meta-harness-harness-science|NLH, Meta Harness, and the Science of Harness Engineering]] — 6× performance gap; natural language harnesses; Meta Harness auto-optimisation; three eras (prompt → context → harness engineering)
- [[agentic-os-eight-components|Agentic OS — Eight-Component Architecture]] — static context, memory levels, skills, skill systems, planning, multi-client, output consolidation, remote access (Simon Scrapes)
- [[advisor-strategy|The Advisor Strategy — Opus Boost for Sonnet/Haiku]] — Small executor drives, Opus advises on demand via the beta `advisor_20260301` tool (Anthropic)
- [[coding-agent-model-comparison-2026|Coding Agent Model Comparison 2026 — DeepSeek V4 vs Opus 4.7 vs GPT 5.5]] — GPT 5.5 wins Terminal Bench; Opus 4.7 wins SWE-Bench; skills portable across harnesses (Chase AI)
- [[deep-modules-codebase-for-ai|Deep Modules — Designing Your Codebase for AI]] — Ousterhout's deep modules as greybox seams for AI; filesystem must mirror the mental map (Matt Pocock)
- [[atlas-codebase-intelligence-layer|Atlas Model Applied to Codebase Intelligence]] — external intelligence layer via MCP for codebases; independent of code review; post-session hooks feed Atlas consolidation; no merge conflicts across developers
- [[steering-claude-code-instruction-methods|Steering Claude Code — When to Use CLAUDE.md, Rules, Skills, Hooks, and Subagents]] — seven instruction methods compared by load timing, compaction, context cost, authority; anti-pattern smells (Anthropic)
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — Replace CLAUDE.md CLI rules with `PreToolUse` hooks; free up instruction budget (Matt Pocock)
- [[verification-loops-in-claude-code|Verification Loops in Claude Code — Encoding Manual Checks as Skills]] — standalone/embedded/chained/PR-wide skill taxonomy; /code-review → /simplify → /verify → /design chain (Anthropic)
- [[skill-creator-evals|Improving Skill-Creator — Test, Measure, Refine Skills]] — Eval authoring, benchmark mode, multi-agent parallel runs, comparator A/B, description tuning (Anthropic)
- [[skills-2-0-user-workflow|Skills 2.0 User Workflow — Evals, AB Tests, and Context Engineering]] — practical user guide to running evals, AB tests for speed/quality, reference-file context engineering (Ben AI)
- [[headless-skill-execution-contracts|Execution Contracts for Headless Skill Runs]] — inline SKILL.md contracts beat hook config when project hooks hijack scheduled headless runs; dashboard dedup-ledger pattern; daily scheduled-agent prompt pattern

## Related Topics

- [[../agent-infrastructure/index|Agent Infrastructure]] — the execution substrate below these harnesses: managed agents, sandboxes, orchestration runtimes, browser/computer tooling
- [[../claude-code-practice/index|Claude Code Practice]] — Claude Code features, workflows, and practitioner interviews
- [[../knowledge-engineering/index|Knowledge Engineering]] — PKM methodology and vault architecture (Atlas, autoresearch, Zettelkasten)
- [[../agent-workflows/index|Agent Workflows]] — structured patterns (RPI, Quick Dev) that run inside a harness
- [[../agent-architecture/index|Agent Architecture]] — 12-factor principles for production agent design
- [[../ai-dev-tools/index|AI Dev Tools]] — IDE-level features (Kiro) that complement harness config
- [[../rpi-methodology/index|RPI Methodology]] — structured workflow that runs inside these harnesses
- [[../ai-security/index|AI Security]] — security scanning and CI hooks as harness-level defences

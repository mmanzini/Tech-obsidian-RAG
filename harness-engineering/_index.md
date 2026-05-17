# Harness Engineering

How to configure, design, and optimize the runtime environment ("harness") around AI coding agents — especially for long-running, multi-session tasks.

## Articles

- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's two-agent pattern (initializer + coder) for multi-context-window work
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — GAN-inspired planner/generator/evaluator architecture for autonomous full-stack builds
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — Practical guide to CLAUDE.md, MCP servers, skills, sub-agents, hooks, and back-pressure
- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence — 3 Key Patterns]] — Use what Claude knows, ask what you can stop doing, set boundaries carefully (Anthropic)
- [[advisor-strategy|The Advisor Strategy — Opus Boost for Sonnet/Haiku]] — Small executor drives, Opus advises on demand via the beta `advisor_20260301` tool (Anthropic)
- [[skill-creator-evals|Improving Skill-Creator — Test, Measure, Refine Skills]] — Eval authoring, benchmark mode, multi-agent parallel runs, comparator A/B, description tuning (Anthropic)
- [[deep-modules-codebase-for-ai|Deep Modules — Designing Your Codebase for AI]] — Ousterhout's deep modules as greybox seams for AI; filesystem must mirror the mental map (Matt Pocock)
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — Replace CLAUDE.md CLI rules with `PreToolUse` hooks; free up instruction budget (Matt Pocock)
- [[browser-mcp-visual-feedback|Browser MCP — Visual Feedback Loops]] — Browser MCP servers (Chrome DevTools, Playwriter, Dev Browser) give agents visual feedback for frontend QA (Matt Pocock)
- [[nlh-meta-harness-harness-science|NLH, Meta Harness, and the Science of Harness Engineering]] — 6× performance gap; natural language harnesses; Meta Harness auto-optimisation; three eras (prompt → context → harness engineering)
- [[mcp-apps-interactive-ui|MCP Apps — Interactive UI Inside MCP Hosts]] — interactive HTML interfaces rendered in chat via sandboxed iframe; bidirectional data flow over MCP
- [[scaling-managed-agents|Scaling Managed Agents — Decoupling Brain from Hands]] — brain/hands/session decoupled as independent interfaces; OS-analogy meta-harness; TTFT −60% p50 (Anthropic engineering)
- [[skills-2-0-user-workflow|Skills 2.0 User Workflow — Evals, AB Tests, and Context Engineering]] — practical user guide to running evals, AB tests for speed/quality, reference-file context engineering (Ben AI)
- [[sandcastle-afk-agent-orchestration|Sandcastle — AFK Software Factory]] — composable `sandcastle.run` primitive; planner/implementer/reviewer/merger pipeline in Docker sandboxes (Matt Pocock)
- [[coding-agent-model-comparison-2026|Coding Agent Model Comparison 2026 — DeepSeek V4 vs Opus 4.7 vs GPT 5.5]] — GPT 5.5 wins Terminal Bench; Opus 4.7 wins SWE-Bench; skills portable across harnesses (Chase AI)
- [[agentic-os-eight-components|Agentic OS — Eight-Component Architecture]] — static context, memory levels, skills, skill systems, planning, multi-client, output consolidation, remote access (Simon Scrapes)

## Related Topics

- [[claude-code-practice/_index|Claude Code Practice]] — Claude Code features, workflows, and practitioner interviews
- [[knowledge-engineering/_index|Knowledge Engineering]] — PKM methodology and vault architecture (Atlas, autoresearch, Zettelkasten)
- [[agent-workflows/_index|Agent Workflows]] — structured patterns (RPI, Quick Dev) that run inside a harness
- [[agent-architecture/_index|Agent Architecture]] — 12-factor principles for production agent design
- [[ai-dev-tools/_index|AI Dev Tools]] — IDE-level features (Kiro) that complement harness config
- [[rpi-methodology/_index|RPI Methodology]] — structured workflow that runs inside these harnesses
- [[ai-security/_index|AI Security]] — security scanning and CI hooks as harness-level defences

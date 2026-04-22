# Harness Engineering

How to configure, design, and optimize the runtime environment ("harness") around AI coding agents — especially for long-running, multi-session tasks.

## Articles

- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's two-agent pattern (initializer + coder) for multi-context-window work
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — GAN-inspired planner/generator/evaluator architecture for autonomous full-stack builds
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — Practical guide to CLAUDE.md, MCP servers, skills, sub-agents, hooks, and back-pressure
- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence — 3 Key Patterns]] — Use what Claude knows, ask what you can stop doing, set boundaries carefully (Anthropic)
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — Sub-agents as context firewalls, invocation paths, and when to skip them (Anthropic)
- [[advisor-strategy|The Advisor Strategy — Opus Boost for Sonnet/Haiku]] — Small executor drives, Opus advises on demand via the beta `advisor_20260301` tool (Anthropic)
- [[skill-creator-evals|Improving Skill-Creator — Test, Measure, Refine Skills]] — Eval authoring, benchmark mode, multi-agent parallel runs, comparator A/B, description tuning (Anthropic)
- [[deep-modules-codebase-for-ai|Deep Modules — Designing Your Codebase for AI]] — Ousterhout's deep modules as greybox seams for AI; filesystem must mirror the mental map (Matt Pocock)
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — Replace CLAUDE.md CLI rules with `PreToolUse` hooks; free up instruction budget (Matt Pocock)
- [[browser-mcp-visual-feedback|Browser MCP — Visual Feedback Loops]] — Browser MCP servers (Chrome DevTools, Playwriter, Dev Browser) give agents visual feedback for frontend QA (Matt Pocock)
- [[claude-code-routines|Claude Code Routines]] — Saved configurations that run autonomously on cloud infrastructure via scheduled, API, and GitHub triggers (Anthropic, research preview)
- [[claude-code-desktop-parallel|Claude Code Desktop Redesign for Parallel Agents]] — Sidebar session management, integrated terminal/editor/diff, drag-and-drop layout for orchestrating multiple simultaneous sessions (Anthropic)
- [[opus-4-7-best-practices|Opus 4.7 Best Practices for Claude Code]] — Effort levels (xhigh default), adaptive thinking, front-loading context, and harness adjustments for the 4.6 → 4.7 upgrade (Anthropic)
- [[use-claude-code-more|Everyone Should Be Using Claude Code More]] — 50 non-technical use cases; reframe as "Claude Agent" on your laptop; right directory + MCP + CLAUDE.md + slash commands (Lenny)

## Related Topics

- [[agent-workflows/_index|Agent Workflows]] — structured patterns (RPI, Quick Dev) that run inside a harness
- [[agent-architecture/_index|Agent Architecture]] — 12-factor principles for production agent design
- [[ai-dev-tools/_index|AI Dev Tools]] — IDE-level features (Kiro) that complement harness config
- [[rpi-methodology/_index|RPI Methodology]] — structured workflow that runs inside these harnesses
- [[ai-security/_index|AI Security]] — security scanning and CI hooks as harness-level defences

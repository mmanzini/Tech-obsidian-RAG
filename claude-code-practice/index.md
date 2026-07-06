# Claude Code Practice

How to use Claude Code effectively — features, workflows, session management, and practitioner interviews with builders who ship via coding agents.

## Articles

- [[claude-creative-connectors|Claude for Creative Work — Connectors for the Creative Industry]] — MCP-based connectors for Ableton, Adobe, Blender, Autodesk Fusion, SketchUp, Splice, and more (Anthropic)
- [[claude-code-large-codebases|Claude Code in Large Codebases — Best Practices for Enterprise Scale]] — harness layering order, codebase navigability, CLAUDE.md maintenance, agent manager role (Anthropic)
- [[goal-command-autonomous-completion|/goal — Condition-Based Autonomous Completion in Claude Code]] — session-scoped evaluator loop: set a verifiable condition, Haiku checks after every turn (Anthropic)
- [[claude-code-routines|Claude Code Routines]] — saved configurations that run autonomously on cloud infrastructure via scheduled, API, and GitHub triggers (Anthropic)
- [[claude-code-desktop-parallel|Claude Code Desktop Redesign for Parallel Agents]] — sidebar session management, integrated terminal/editor/diff, drag-and-drop layout for orchestrating simultaneous sessions (Anthropic)
- [[claude-code-session-management|Claude Code Session Management and 1M Context]] — context rot, five turn-level options (continue/rewind/clear/compact/subagent), practical decision table (Anthropic)
- [[claude-code-quality-postmortem|Claude Code Quality Postmortem — April 2026]] — three issues (effort downgrade, caching bug, verbosity prompt) that caused degradation; root-cause analysis (Anthropic)
- [[claude-code-structured-memory|Claude Code Structured Memory — ~/.claude/memory/ Architecture]] — structured memory hierarchy with PreToolUse hook injection; Huryn/Conneely pattern
- [[claude-code-agentic-os|Claude Code Agentic OS — Three-Gap Framework]] — memory/consistency/access gaps, org-chart skill mapping, command center dashboard (Chase AI)
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — sub-agents as context firewalls, invocation paths, and when to skip them (Anthropic)
- [[use-claude-code-more|Everyone Should Be Using Claude Code More]] — 50 non-technical use cases; reframe as "Claude Agent" on your laptop (Lenny)
- [[opus-4-7-best-practices|Opus 4.7 Best Practices for Claude Code]] — effort levels (xhigh default), adaptive thinking, front-loading context, harness adjustments for 4.6 → 4.7 (Anthropic)
- [[claude-cowork-full-guide|Claude Cowork — Full Practical Guide]] — non-technical agent harness: skills, plugins, sub-agents, scheduled tasks, AI OS for teams (Ben AI)
- [[claude-cowork-setup-guide|Claude Cowork Setup Guide — 7 Steps for Non-Technical Users]] — install, global instructions, about-me folder, connectors, skills, plugins, scheduled tasks (Systems Made Better)
- [[claude-cowork-context-questionnaire|Claude Cowork Context Questionnaire — Business & Personal Brain Dump]] — onboarding questionnaire feeding organization.md, brand.md, strategy.md, icp.md, stakeholders.md
- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — filesystem-based cross-session memory, scoped stores, audit logs, enterprise controls (Anthropic)
- [[dhanji-prasanna|Dhanji Prasanna on Lenny's Podcast]] — GM orgs are wrong for AI-native companies; Goose saves 8–10 hrs/eng/week at Block
- [[scott-wu|Scott Wu on Lenny's Podcast]] — Devin as a named junior engineer; Jevons Paradox means cheaper code drives more demand
- [[sherwin-wu|Sherwin Wu on Lenny's Podcast]] — 95% of OpenAI API engineers use Codex; +70% PR throughput; we're sorcerers now
- [[lazar-jovanovic|Lazar Jovanovic on Lenny's Podcast]] — 80% planning / 20% building; externalise context into a Markdown PRD stack
- [[zevi-arnovitz|Zevi Arnovitz on Lenny's Podcast]] — non-technical PMs ship real products via Cursor + Claude Code + /command library; multi-model peer review
- [[claude-code-large-codebases|Claude Code in Large Codebases — Best Practices at Scale]] — harness-first guide to deploying Claude Code in monorepos and enterprise environments: CLAUDE.md layering, LSP, plugins, ownership model (Anthropic)
- [[claude-code-goal-command|Claude Code /goal — Autonomous Multi-Turn Completion]] — session-scoped command that keeps Claude working across turns until a separate evaluator model confirms the condition is met (Anthropic)
- [[claude-code-agent-view|Claude Code Agent View — Managing Multiple Background Sessions]] — full-terminal dashboard for dispatching and managing many background sessions; supervisor process, git worktree isolation, peek/attach flow (Anthropic)
- [[claude-code-dynamic-workflows|Claude Code Dynamic Workflows — Script-Driven Subagent Orchestration at Scale]] — rerunnable JavaScript scripts that orchestrate dozens-to-hundreds of subagents in the background; ultracode, /deep-research, saved workflow commands, 16-concurrent/1,000-agent limits (Anthropic)

## Related Topics

- [[harness-engineering/index|Harness Engineering]] — design theory and runtime patterns that underpin Claude Code configuration
- [[knowledge-engineering/index|Knowledge Engineering]] — PKM methodology and vault architecture (Atlas, autoresearch, Zettelkasten)
- [[rpi-methodology/index|RPI Methodology]] — structured workflow commonly run inside Claude Code
- [[agent-architecture/index|Agent Architecture]] — 12-factor agent design principles

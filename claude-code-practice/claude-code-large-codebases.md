---
type: synthesis
title: Claude Code in Large Codebases — Best Practices for Enterprise Scale
description: Anthropic's "Claude Code at scale" series, based on observed patterns from deployments in multi-million-line monorepos, legacy systems, and distributed microservice architectures.
bucket: ai-engineering
topic: claude-code-practice
tags: [claude-code, harness-engineering, skills-and-hooks, mcp, context-engineering, ai-org-design]
source: https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start
resource: https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start
timestamp: 2026-05-25T00:17:36Z
status: active
related:
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/claude-code-practice/subagents-in-claude-code.md
  - ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
  - ai-engineering/harness-engineering/deep-modules-codebase-for-ai.md
  - ai-engineering/harness-engineering/agent-view-multi-agent-management.md
---

# Claude Code in Large Codebases — Best Practices for Enterprise Scale

**Source:** [How Claude Code works in large codebases: Best practices and where to start](https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start)

---

## Summary

Anthropic's "Claude Code at scale" series, based on observed patterns from deployments in multi-million-line monorepos, legacy systems, and distributed microservice architectures. The core thesis: the harness (CLAUDE.md files, hooks, skills, plugins, LSP integrations, MCP servers, subagents) determines performance more than the model alone, and successful large-scale deployments share recognizable patterns in codebase setup, configuration maintenance, and organizational ownership.

## How Claude Code Navigates Large Codebases

Claude Code navigates like a software engineer: traverses the file system, reads files, uses grep to find what it needs, and follows references. No centralized index needs to be built or maintained — each developer's instance works from the live codebase (source: claude-code-large-codebases.md).

This contrasts with RAG-powered tools that embed the codebase and retrieve chunks at query time. Embedding pipelines can't keep up with active engineering teams — indexes become stale (referencing renamed functions, deleted modules) with no indication they're out of date (source: claude-code-large-codebases.md).

The tradeoff: agentic search requires enough starting context for Claude to know where to look. Quality of navigation is shaped by how well the codebase is set up with CLAUDE.md files and skills (source: claude-code-large-codebases.md).

## The Harness: Five Extension Points Plus Two Capabilities

The harness is built from five extension points in a recommended layering order (source: claude-code-large-codebases.md):

1. **CLAUDE.md files** — load automatically at session start; root file for big picture, subdirectory files for local conventions. The foundation everything else builds on. Keep them lean and focused on what applies broadly.

2. **Hooks** — scripts that run at key moments (start, stop, file write events). Two high-value uses: (a) stop hooks that capture session learnings and propose CLAUDE.md updates while context is fresh; (b) start hooks that load team-specific context dynamically per module. For linting and formatting, hooks enforce rules deterministically — more consistent than relying on Claude to remember instructions.

3. **Skills** — packaged instructions for specific task types, loaded on demand via progressive disclosure. Don't load everything into every session. Skills can be scoped to specific paths so they only activate in the relevant part of a monorepo. Example: a payments-service deployment skill that never auto-loads when working elsewhere.

4. **Plugins** — bundles of skills, hooks, and MCP configurations as a single installable package. The mechanism for distributing a working setup across the org. When a new engineer installs the plugin on day one, they have the same context and capabilities as veterans. Updates can be distributed via managed marketplaces.

5. **MCP servers** — connect Claude to internal tools, data sources, and APIs it can't otherwise reach. Most sophisticated teams build MCP servers exposing structured search as a tool Claude can call directly.

Additional capabilities:

- **LSP integrations** — give Claude symbol-level navigation ("go to definition", "find all references") rather than text pattern matching. Grep for a common function name in a large codebase returns thousands of matches; LSP returns only references pointing to the same symbol. Highest-value investment for multi-language codebases.
- **Subagents** — isolated Claude instances with their own context window. Spin up a read-only subagent to map a subsystem and write findings to a file, then have the main agent edit with the full picture. Splits exploration from editing.

## Three Configuration Patterns from Successful Deployments

### 1. Making the Codebase Navigable at Scale

Key practices observed across successful deployments (source: claude-code-large-codebases.md):

- **Lean, layered CLAUDE.md files**: root file = pointers and critical gotchas only. Subdirectory files for local conventions. Additive loading as Claude moves through the codebase.
- **Initialize in subdirectories, not repo root**: Claude automatically walks up and loads every CLAUDE.md it finds, so root-level context is never lost. Scoping to the relevant part reduces noise.
- **Scope test/lint commands per subdirectory**: prevents timeouts and irrelevant output from running the full suite when only one service was changed. Works well for service-oriented codebases; harder for compiled-language monorepos with deep cross-directory dependencies.
- **`.ignore` files and `permissions.deny` rules** in `.claude/settings.json` to exclude generated files, build artifacts, third-party code. Version-controlled so every developer gets the same noise reduction automatically.
- **Codebase maps** for non-conventional directory structures: a lightweight markdown file at the root listing top-level folders with one-line descriptions gives Claude a table of contents. Best as a layered approach — root describes highest-level structure, subdirectory CLAUDE.md files add detail on demand.
- **LSP servers** for symbol-level search. Requires installing a code intelligence plugin and the corresponding language server binary.

### 2. Actively Maintaining CLAUDE.md as Models Evolve

CLAUDE.md instructions written for one model generation can work against a future one. Skills and hooks built to compensate for specific model limitations become overhead once those limitations are fixed (source: claude-code-large-codebases.md).

Recommendation: meaningful configuration review every 3–6 months, and whenever performance feels like it's plateaued after major model releases (source: claude-code-large-codebases.md).

### 3. Assigning Ownership for Claude Code Management and Adoption

Technical configuration alone doesn't drive adoption. Patterns from successful rollouts (source: claude-code-large-codebases.md):

- Dedicated infrastructure investment *before* broad access — a small team wires up tooling so Claude already fits developer workflows when developers first touch it
- Emerging role: **agent manager** — hybrid PM/engineer function dedicated to managing the Claude Code ecosystem. Owns configuration, permissions policy, plugin marketplace, CLAUDE.md conventions.
- Cross-functional working groups: engineering + information security + governance representatives to define requirements together and build a rollout roadmap
- In regulated industries: start with a defined set of approved skills, required code review processes, and limited initial access; expand as confidence builds

Bottoms-up adoption generates enthusiasm but fragments without someone to centralize what works. Tribal knowledge stays tribal without a DRI (directly responsible individual) to assemble and evangelize conventions (source: claude-code-large-codebases.md).

## Key Takeaways

- The harness matters as much as the model — build it in the recommended layering order: CLAUDE.md → hooks → skills → plugins → MCP servers
- Agentic search (live codebase) beats RAG indexing at scale, but requires good starting context
- LSP integrations are the highest-value investment for multi-language large codebases
- Subagents separate exploration from editing, preventing context window bloat
- CLAUDE.md files need active maintenance as models evolve — review every 3–6 months
- Assign an agent manager or DRI; don't let good setups stay tribal

## Related

- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — detailed practical guide to the same harness components
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — when and how to invoke subagents for exploration/editing separation
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — hooks for consistent automated behavior
- [[deep-modules-codebase-for-ai|Deep Modules — Designing Your Codebase for AI]] — codebase architecture principles that make AI navigation effective
- [[agent-view-multi-agent-management|Agent View — Managing Multiple Claude Code Sessions]] — the multi-session management layer above individual session configuration

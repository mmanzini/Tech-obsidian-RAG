---
type: synthesis
title: Claude Code Structured Memory — ~/.claude/memory/ Architecture
description: A practical guide to giving Claude Code persistent, structured memory via a `~/.claude/memory/` directory hierarchy — eliminating the bloated flat-CLAUDE.md anti-pattern and enabling on-demand context loading.
bundle: ai-engineering
topic: claude-code-practice
tags:
- claude-code
- agent-memory
- context-engineering
- skills-and-hooks
- knowledge-management
sources:
- id: 2026-05-03-how-20i-20finally-20sorted-20my-20claude-20code-20memory-201
  resource: ../../../Resources/web-clippings/2026-05-03-How%20I%20Finally%20Sorted%20My%20Claude%20Code%20Memory%201.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-17T08:21:13Z'
status: stable
related:
- ai-engineering/harness-engineering/skill-issue-harness-engineering.md
- ai-engineering/claude-code-practice/claude-code-agentic-os.md
- ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
---

# Claude Code Structured Memory — ~/.claude/memory/ Architecture

**Source:** [2026-05-03-How I Finally Sorted My Claude Code Memory 1.md](../../../Resources/web-clippings/2026-05-03-How%20I%20Finally%20Sorted%20My%20Claude%20Code%20Memory%201.md)
**Author:** John Conneely (via Paweł Huryn's original tip)

---

## Summary

A practical guide to giving Claude Code persistent, structured memory via a `~/.claude/memory/` directory hierarchy — eliminating the bloated flat-CLAUDE.md anti-pattern and enabling on-demand context loading. Paweł Huryn's original insight (a few CLAUDE.md lines) matured into a full folder structure with hooks for guaranteed injection.

## The Problem

Claude Code starts every session from zero unless instructions exist in CLAUDE.md. A flat append-to-CLAUDE.md approach eventually bloats into a sprawling document loaded entirely at every session, causing irrelevant context to consume tokens on every call. (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)

## Paweł Huryn's Base Pattern

Paste into project CLAUDE.md:

```
## Memory Management

When you discover something valuable for future sessions — architectural decisions, bug fixes, gotchas, environment quirks — immediately append it to .claude/memory.md

Keep entries short: date, what, why. Read this file at the start of every session.
```

Costs almost zero tokens, survives crashes and context compaction, works on Claude Code and Claude Cowork. Files created via CLAUDE.md can also be synced to GitHub for cross-surface sharing — an advantage over Anthropic's auto-memory which is session-scoped. (source: 2026-05-03-Paweł Huryn (@huryn) 1.md)

## John Conneely's Extended Structure

```
~/.claude/memory/
  memory.md           ← index (read at session start)
  general.md          ← cross-project conventions and preferences
  tools/
    snowflake.md
    atlassian.md
    slack.md
  domain/
    {product}/
      {product}.md
    {project}/
      {project}.md
```

**memory.md** is the index — one line per topic file, read every session. **general.md** holds naming conventions, workflow preferences. **tools/** holds tool-specific quirks and configurations. **domain/** is where project/product knowledge builds up over time. (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)

## Global vs Project Memory

- **Global** (`~/.claude/memory/`): tools, conventions, credential patterns — applies everywhere
- **Project** (`~/.claude/projects/{mapped-path}/memory/MEMORY.md`): active tickets, codebase patterns — project-specific

MEMORY.md has a 200-line budget per session start. Routing rules belong in CLAUDE.md (loads in full), not in MEMORY.md — a common mistake that wastes the budget. (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)

## Domain Knowledge Lifecycle

1. **Staging** — knowledge accumulates in `domain/{name}/`
2. **Promotion** — enough knowledge to package as a Claude Code skill
3. **Pointer** — after promotion, memory file becomes a 3-line pointer to the skill

When a domain file grows too large, raise it to a skill. The memory entry just says where to find it. (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)

## PreToolUse Hook (optional)

A bash wrapper + Python script injects project MEMORY.md and global index before the first tool call of every session — including new subagents. Uses `os.getppid()` as session identifier (not `CLAUDE_SESSION_ID`, which doesn't exist in hooks). Two-file design: shell wrapper (~5ms) gates Python startup (~80ms) so overhead after first call is negligible. (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)

## Anthropic Auto-Memory Note

Auto memory shipped in Claude Code v2.1.59 (Feb 26, 2026) — scoped per git repo, stored in `~/.claude/projects/{path}/memory/`. The manual approach remains valuable because it creates shareable files (Code/Cowork/Web) via GitHub. (source: 2026-05-03-Paweł Huryn (@huryn) 1.md)

## Key Takeaways

- Flat CLAUDE.md memory anti-pattern causes context bloat; structured subdirectories load only what's relevant (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)
- Routing rules belong in CLAUDE.md (full load), not MEMORY.md (200-line budget) (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)
- PreToolUse hook guarantees injection even when the CLAUDE.md instruction is missed; `os.getppid()` is the correct session identifier (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)
- Domain knowledge lifecycle: staging → promotion → pointer; when a domain file earns a skill, memory just points at the skill (source: 2026-05-03-How I Finally Sorted My Claude Code Memory 1.md)
- Manual memory files sync via GitHub across surfaces; Anthropic auto-memory doesn't (source: 2026-05-03-Paweł Huryn (@huryn) 1.md)

## Related

- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — broader CLAUDE.md, MCP, skills, and hooks configuration guide
- [[claude-code-agentic-os|Claude Code Agentic OS — Three-Gap Framework]] — memory as one of three key gaps in agentic OS design
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — PreToolUse hooks for deterministic behaviour

# Built-in Memory for Claude Managed Agents

**Source:** https://claude.com/blog/claude-managed-agents-memory
**Date captured:** 2026-04-25
**Status:** Public beta

---

## What It Is

Memory on Managed Agents lets agents learn and improve across sessions without developers building custom memory infrastructure. Memories are stored as files — exportable, manageable via API, giving developers full control over what agents retain.

## How It Works

- Memory mounts directly onto a filesystem — agents use the same bash and code-execution capabilities they already have for other tasks
- Filesystem-based memory: Opus 4.7 saves more comprehensive, well-organised memories and is more discerning about what to retain for a given task
- **Cross-agent sharing**: stores can be shared across multiple agents with different access scopes (e.g., org-wide read-only store + per-user read-write store)
- Multiple agents can work concurrently against the same store without overwriting each other

## Enterprise Controls

- Scoped permissions per store and agent
- Detailed audit log: which agent + session a memory came from
- Rollback to earlier versions; redact content from history
- Session events surface in the Claude Console so developers can trace what was learned and where

## Production Use Cases

| Team | What they built |
|---|---|
| Netflix | Agents carry context across sessions, including insights uncovered over multiple turns and human corrections — no manual prompt/skill updates |
| Rakuten | Long-running task agents learn from every session; cut first-pass errors by 97% within workspace-scoped, observable boundaries |
| Wisedocs | Document verification pipeline uses cross-session memory to spot recurring issues; verification 30% faster |
| Ando | Workplace messaging platform — captures per-organisation interaction patterns instead of building memory infrastructure |

## Key Takeaways

- File-system-based memory integrates with existing agentic tool use (no new paradigm to learn)
- Scoped, auditable stores are the enterprise-safe pattern — one org-wide read-only store + per-user read-write store is the recommended baseline
- Memory is the mechanism by which agents compound their learning; without it, every session restarts cold
- Cross-session memory can eliminate categories of error (Rakuten: 97% first-pass error reduction) and speed up verification workflows significantly
- Developers retain full ownership: memories are just files, not opaque embeddings

## Getting Started

Available via Claude Console or the Managed Agents CLI. Documentation: https://platform.claude.com/docs/en/managed-agents/memory

## Related

- [[harness-engineering/_index|Harness Engineering]] — broader harness design context
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — multi-context-window agent patterns
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — context isolation patterns
- [[nlh-meta-harness-harness-science|NLH, Meta Harness, and the Science of Harness Engineering]] — how file-backed state enables harness durability

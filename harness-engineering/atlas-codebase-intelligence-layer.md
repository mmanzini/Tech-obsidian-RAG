# Atlas Model Applied to Codebase Intelligence

**Source:** [Projects/ai-driven-product-management/Ideas to include.md](../../../Resources/Projects/ai-driven-product-management/Ideas to include.md)
**Author:** Max

---

## Summary

A vision for applying the Atlas external intelligence layer model to software codebases. Rather than embedding `.md` context files directly into repositories (where they become part of PRs and create merge conflicts), an external intelligence layer — surfaced via MCP — lives independently of any codebase, is maintained by agents, and is always up to date. Engineers and AI assistants query it for current context without the layer ever appearing in code review (source: Ideas to include.md).

## The Vision

The standard approach to Claude Code context in large codebases is to spread `CLAUDE.md` and memory files throughout the repo tree, loading different context as Claude navigates the code. This works for single developers but breaks down in teams (source: Ideas to include.md):

- Intelligence files become part of PRs — they appear in code review noise, create merge conflicts across developers and teams, and get stale between reviews.
- The "state of the codebase" as understood by AI agents is embedded in the source tree, which is wrong: source code describes what the codebase does; agent context describes how the agents understand it, which is a separate layer.

**The Atlas alternative:** Keep the intelligence layer external. Specifically:

- The intelligence layer is maintained by agents (via Atlas-style consolidation) and is completely independent of any code repository.
- It is surfaced to agents via MCP — agents query the Atlas MCP server for current codebase context rather than reading `.md` files in the repo.
- Skills can write back to it: a post-code-review skill can summarise what changed and feed that context into the next Atlas consolidation cycle.
- No merge conflicts, no code review noise. The codebase stays clean; the intelligence layer evolves independently (source: Ideas to include.md).

## Implementation Pattern

The proposed cycle (source: Ideas to include.md):

1. **Claude Code session** — agent is given access to Atlas MCP, which surfaces current codebase intelligence alongside the code.
2. **Post-session hook** — a stop hook triggers a skill that summarises what changed in the session (new patterns, decisions, gotchas) and writes a new source file to `Resources/context/` in Atlas.
3. **Consolidation run** — the new context file is picked up in the next `consolidate` run, integrated into the Intelligence wiki, and made available to the next session.
4. **Nightly batch** — for scale, a nightly run processes the day's context additions, runs `refine`, and updates the wiki so every next-morning session starts with current intelligence.

This mirrors the Atlas daily-digest pattern, applied to codebase intelligence rather than personal daily activity.

## Claude Code Best Practices Context

The source file also excerpts best practices for Claude Code in large codebases that inform this vision (source: Ideas to include.md):

- **Hooks for self-improvement.** A stop hook can reflect on a session and propose `CLAUDE.md` updates while context is fresh. A start hook can load team-specific context dynamically per module. The Atlas model externalises this: instead of hooks updating `CLAUDE.md` in the repo, they feed the Atlas intelligence layer.
- **LSP integrations.** Language server protocol gives Claude symbol-level navigation (go to definition, find all references) — critical for C/C++ and multi-language codebases at scale.
- **CLAUDE.md layering.** Claude walks up the directory tree loading every `CLAUDE.md` it finds. Root-level context is always loaded; subdirectory files load on demand. In the Atlas model, the MCP supplement replaces most of this — per-module intelligence lives in Atlas, not in the code tree.
- **Codebase maps.** A root markdown file describing top-level folders helps Claude navigate large, unconventional structures. In the Atlas model, this map lives in `Intelligence/` and is served via MCP.

## Why This Matters

The standard Claude Code context model degrades in teams: multiple developers updating `CLAUDE.md` files creates merge conflicts and code review overhead for non-code changes (source: Ideas to include.md). The Atlas model scales because:

- Agent context is decoupled from the code review cycle entirely.
- The intelligence layer is always the consolidation of what agents actually observed, not what a developer thought to write.
- MCP gives every agent — across developers, teams, and stacks — the same current view of codebase intelligence without any per-developer setup.

This is the Atlas wiki's architectural hypothesis applied to software engineering teams rather than personal knowledge management.

## Key Takeaways

- Intelligence files should not live in codebases — they belong in an external layer surfaced via MCP.
- Atlas consolidation pattern + post-session hooks + nightly batch = always-current codebase intelligence without code review noise.
- LSP integrations (symbol navigation), hooks (self-improvement), and CLAUDE.md layering are complementary — Atlas externalises the storage, these tools feed the quality of what gets stored.
- No merge conflicts: the codebase and the intelligence layer are independent systems with a one-way feed (code → Atlas context → MCP → agent).

## Related

- [[atlas-sync-architecture]] — the Atlas sync infrastructure this vision extends to codebases
- [[claude-code-large-codebases]] — enterprise harness guide; the best-practices source this vision responds to
- [[zettelkasten-pkm]] — PKM methodology underpinning the Atlas external-intelligence model

---
type: pattern
title: Codebase intelligence layer lives outside the code repo
description: Architectural decision to host the AI-maintained intelligence layer in a standalone vault with its own git repo, separate from the codebase it describes — prevents merge conflicts and keeps AI as sole writer.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- knowledge-engineering
- atlas-infrastructure
- agent-collaboration
- agentic-patterns
sources:
- id: 2026-08-14-intelligence-layer-outside-repo
  resource: Resources/context/2026-08-14-intelligence-layer-outside-repo.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-16T00:00:00Z'
status: stable
related:
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
- ai-engineering/knowledge-engineering/agentic-knowledge-engine-overview.md
- ai-engineering/knowledge-engineering/team-agentic-os-gbrain.md
- ai-engineering/knowledge-engineering/agentic-knowledge-engine-team-security.md
---

# Codebase intelligence layer lives outside the code repo

**Source:** [2026-08-14-intelligence-layer-outside-repo.md](../../../Resources/context/2026-08-14-intelligence-layer-outside-repo.md)
**Date decided:** 2026-08-14

---

## Summary

When applying the Atlas pattern to a software codebase, the intelligence layer (wiki of consolidated knowledge about the code) must live in a standalone vault with its own git repo — separate from the codebase's source tree. Two reasons: the intelligence layer is a byproduct of consolidation, not source; and separating it prevents merge conflicts when multiple human operators work on the same codebase alongside AI consolidation.

## The decision

Max bootstrapped `portfolio-intelligence` as the pilot: a standalone Atlas vault for the `maxmanzini.com` portfolio codebase, in its own git repo, with AI as the sole writer (source: 2026-08-14-intelligence-layer-outside-repo.md). Tracked as T061; serves T007 (portfolio codebase) as the pilot.

The scope chosen for the pilot: core loop + episodes + evals; tier-2 hybrid search deliberately skipped while the wiki is small (source: 2026-08-14-intelligence-layer-outside-repo.md).

## Reasoning

### The intelligence layer is a byproduct, not source

"The Intelligence Layer is a byproduct of the consolidation of the codebase, and the codebase is the part that lives in the source tree." Mixing them conflates input and output — the codebase is what gets consolidated; the intelligence layer is the result (source: 2026-08-14-intelligence-layer-outside-repo.md).

### Merge-conflict contamination risk at scale

In a team setting, multiple human operators commit to the codebase concurrently. If the intelligence layer lives in the same repo, AI consolidation commits race against human development commits, producing merge conflicts. These conflicts corrupt the intelligence layer — not the source code — because the AI is the sole authoritative writer of wiki content (source: 2026-08-14-intelligence-layer-outside-repo.md). Keeping the intelligence layer in its own repo makes the AI the exclusive committer; no human development workflow touches it.

## Pattern structure

```
<codebase-repo>/          # human-owned, multi-operator
  src/
  tests/
  ...

<codebase>-intelligence/  # AI-owned, standalone git repo
  Intelligence/
    <bundle>/<topic>/<article>.md
  Resources/
  _episodes/
  _eval/
```

The intelligence vault reads the codebase (read-only access) and writes only inside itself.

## Key Takeaways

- Codebase intelligence layer = standalone vault in its own git repo; not a subfolder of the code repo.
- Motivation: separation of byproduct from source; prevention of merge conflicts.
- AI is the sole writer of the intelligence vault; human developers never commit to it.
- Pilot: `portfolio-intelligence` for `maxmanzini.com` (T061 / T007).
- Scope shortcut for small wikis: skip tier-2 hybrid search; core loop + episodes + evals suffices.

## Related

- [[llm-vault-structure-spec]] · [LLM Vault Structure Spec](llm-vault-structure-spec.md) — the bundle/topic/article architecture this standalone vault uses
- [[agentic-knowledge-engine-overview]] · [Agentic knowledge engine overview](agentic-knowledge-engine-overview.md) — end-to-end design of the two-memory system this pattern extends
- [[team-agentic-os-gbrain]] · [Team Agentic OS](team-agentic-os-gbrain.md) — the team-scale predecessor to this pattern; access-control and multi-operator context
- [[agentic-knowledge-engine-team-security]] · [Agentic knowledge engine — team security](agentic-knowledge-engine-team-security.md) — permission spine design that complements the repo-separation pattern

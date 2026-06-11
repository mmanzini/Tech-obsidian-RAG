# Intelligence-layer strand map

**Source:** [strand-map.md](../../../Resources/Projects/ai-driven-product-management/strand-map.md)
**Author:** Max (reference note, relocated 2026-06-02)

---

## Summary

The evergreen relationship narrative between Max's intelligence-layer / AI-PM project strands — seven independent projects, different targets, not stages of one project. Per-strand status and next-actions live as board tasks (T004/T005/T007–T011); this is the map of how they relate. Two clusters emerge: a "harness lineage" (the same Resources-zone + Intelligence-zone, agent-curated, MCP-at-the-boundary pattern on three deployment surfaces) and the Three-Diamond AI-PM framework that ties four of the strands into one operating model.

## The seven strands

1. **External intelligence layer for codebases** — dev-tool version; lifts the agent's context layer out of the codebase and serves it over MCP; hooks feed micro-updates, consolidation runs on PR merge (board T007) (source: strand-map.md)
2. **Agentic knowledge engine** — the personal-knowledge version; wiki-backed RAG vault operated by Claude Code, frozen harness in `schema.md`, iterable program in `CLAUDE.md`. The implementation Atlas itself runs on (source: strand-map.md)
3. **My intelligence-layer implementation** — earlier architecture sketch: Atlas as a GitHub-backed vault, auto-deployed via Vercel, reached over MCP via `/sync-atlas` (board T005) (source: strand-map.md)
4. **Super-agent in Slack** — one-agent-per-company pattern (board T008) (source: strand-map.md)
5. **Human + agent UX** — six UX affordances once humans and agents share an artefact (board T009) (source: strand-map.md)
6. **Senior Engineer Benchmark** — reprimeable eval pattern (board T010) (source: strand-map.md)
7. **Three-Diamond AI PM Framework** — the umbrella tying strands 1, 4, 5, 6 together (board T011) (source: strand-map.md)

## How they relate

**Strands 1, 2, 3 — the harness lineage.** Same pattern (Resources zone + Intelligence zone, agent-curated, MCP at the boundary), three deployment surfaces (source: strand-map.md):

- Strand 2 (`agentic-knowledge-engine`) is the **reference harness** — personal vault, file-backed, runs on Claude Code locally. Reading it first is the fastest way to understand the pattern.
- Strand 3 (`my-intelligence-layer-implementation`) is the **hosted-Atlas sketch** — same shape lifted onto Git + Vercel + MCP.
- Strand 1 (`intelligence-context-layer`) is the **dev-tool generalisation** — same shape retargeted at code teams; hooks replace `auto-capture`, PR merges replace manual consolidation triggers; the vault holds codebase context instead of personal context. This is where the pattern goes commercial (source: strand-map.md).

**Strand 7 — the umbrella.** The Three-Diamond AI PM Framework is the operating model; strands 1, 4, 5, 6 are its substrate, surfaces, and infrastructure (source: strand-map.md):

- Strand 1 is the **substrate** — every diamond reads from and writes into the vault.
- Strand 4 (`super-agent-in-slack`) is the **aggregation surface** for Diamond 1 internal signal and Diamond 3 feedback loops.
- Strand 5 (`human-plus-agent-ux`) is the **UX language** Diamond 2 prototypes and Diamond 3 increments inherit.
- Strand 6 (`senior-engineer-benchmark`) is the **eval philosophy** Diamond 2 validation and Diamond 3 delivery use.

Read strand 7 first for the operating model; the others are the parts. The Shipper podcast seeded strands 4–6 (source: strand-map.md).

## Key Takeaways

- Seven independent strands, not stages of one project — tracked as board tasks T004/T005/T007–T011
- Harness lineage (1,2,3): one pattern, three surfaces — personal reference harness → hosted Atlas → commercial dev-tool generalisation
- Strand 2 (agentic-knowledge-engine) is the reference implementation to read first
- Three-Diamond framework (strand 7) is the operating model; strands 1/4/5/6 are its substrate / aggregation surface / UX language / eval philosophy

## Related

- [[agentic-knowledge-engine-team-security|Agentic knowledge engine — team security & permissions]] — the T017 security design for scaling strand 2 to teams
- [[llm-vault-structure-spec|LLM Vault Structure Spec]] — the bucket/topic/article architecture strand 2 runs on
- [[atlas-sync-architecture|Atlas Sync Architecture]] — the hosted-sync layer behind strand 3
- [[autoresearch-methodology|Autoresearch methodology]] — the program.md/harness lineage the pattern descends from
- [[memory-three-jobs-and-atlas-tiers|Memory's three jobs and Atlas's tier adoption]] — the 2026-06-11 memory-tiers upgrade of strand 2's engine, benchmarked against external frameworks

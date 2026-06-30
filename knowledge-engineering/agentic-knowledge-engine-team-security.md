---
type: synthesis
title: Agentic knowledge engine — team security & permissions
description: A design note for taking the single-user Atlas pattern (frozen harness in `schema.md`, iterable program in `CLAUDE.md`, markdown bundles consolidated by Claude Code) and scaling it to a team **without leaking data across people, clients, or teams**.
bundle: ai-engineering
topic: knowledge-engineering
tags: [knowledge-management, ai-org-design, ai-security, harness-engineering]
source: ../../../Resources/Projects/ai-driven-product-management/team-knowledge-engine-security/README.md
resource:
timestamp: 2026-06-05T05:02:31Z
status: active
related:
  - ai-engineering/knowledge-engineering/team-agentic-os-gbrain.md
  - ai-engineering/knowledge-engineering/intelligence-layer-strand-map.md
  - ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
  - ai-engineering/knowledge-engineering/atlas-sync-architecture.md
---

# Agentic knowledge engine — team security & permissions

**Source:** [team-knowledge-engine-security/README.md](../../../Resources/Projects/ai-driven-product-management/team-knowledge-engine-security/README.md)
**Author:** Max (design note)
**Board task:** T017

---

## Summary

A design note for taking the single-user Atlas pattern (frozen harness in `schema.md`, iterable program in `CLAUDE.md`, markdown bundles consolidated by Claude Code) and scaling it to a team **without leaking data across people, clients, or teams**. The markdown substrate is already team-ready; security and access control are the hard part. It synthesises two reference models — GBrain (rigorous) and Team OS / Simon Scrapes (pragmatic) — into a permission spine where one source of truth is mirrored, never hand-maintained, onto every surface.

## The problem

Atlas today is single-user: one git-backed vault, one operator, no notion of "who may read this bundle" (source: team-knowledge-engine-security/README.md). A team version must answer one question — shared where it should be, private where it shouldn't — across every surface a person can reach the knowledge: the files, the agent runtime, version control, and any external memory index. Each surface is a separate leak vector and must be gated independently, all mirroring one source of truth for permissions (source: team-knowledge-engine-security/README.md).

## Two reference models

**GBrain (rigorous).** A *brain* is a database (personal or team mount); a *source* is a repo inside it — mapping cleanly to Atlas (vault = brain, bundle = source). Storage is markdown in git, synced into Postgres + pgvector; git deletions become DB soft-deletes. Access control uses scope-gated MCP tokens (`read`/`write`/`admin`), OAuth 2.1 + PKCE for cloud connectors, login-scoped reads, per-source and brain-wide DB keys, an audit log carrying the agent's identity, and zero-leak fuzz testing across every read path (source: team-knowledge-engine-security/README.md). (source: https://github.com/garrytan/gbrain)

**Team OS / Simon Scrapes (pragmatic).** A three-tier file model (human source-of-truth in Notion/GDrive → agent-maintained files in the Claude Code repo → GitHub as backup/version control) with access control across four systems all copying the shared-drive permissions: shared drive (source of truth), local Claude Code (least privilege by token), GitHub (one repo per client, membership mirroring the drive), and a memory DB (per-person index, or shared Supabase Postgres with row-level security) (source: team-knowledge-engine-security/README.md). Private overlays live in gitignored `CLAUDE.local.md`.

## Recommended Atlas team-edition architecture

Keep the markdown-first, git-backed core; add a permission spine every surface mirrors (source: team-knowledge-engine-security/README.md):

1. **Permission unit = bundle (optionally client).** A bundle's `_master-index.md` already declares scope; extend it with an **access declaration** (who may read/write) — the single source of truth.
2. **Mirror that declaration onto every surface automatically, never by hand:** Git (one repo/scoped path per client/sensitive bundle, membership generated from the declaration); MCP boundary (scope-gated, OAuth-scoped tokens whose reachable bundles equal the person's declaration; login-scoped reads); shared memory (one Supabase Postgres + pgvector, every row tagged by bundle/client, row-level security on every query); local (checkout holds only what the token pulled; `CLAUDE.local.md` for private overlays).
3. **Consolidation respects scope.** A per-bundle worker may only read/write its own bundle (already a `schema.md` hard rule); extend so it ingests only sources the operator is cleared for and tags emitted memory rows by bundle so RLS can gate them.
4. **Audit + verify.** Log every write with the acting agent/person identity; fuzz-test every read path for zero cross-bundle leaks before trusting it.

## Security principles (non-negotiables)

- One source of truth for permissions; every other system copies it — drift between git, token scope, and DB RLS is the leak (source: team-knowledge-engine-security/README.md)
- Least privilege by token: the runtime can only ever hold what the token permitted — isolation by construction, not by agent good behaviour
- Treat each surface as a separate boundary; gate all four
- Row-level security for shared memory, tagged by bundle/client
- Zero-leak fuzz testing as an acceptance gate, plus identity-stamped audit logs
- No vendor lock-in — stays markdown + git underneath

## Open questions

Permission granularity (per-bundle vs per-topic/article); where the access declaration lives (bundle `_master-index.md` vs a top-level `access.yaml`); identity provider for OAuth-scoped MCP tokens; self-hosted Postgres vs Supabase; how `refine`/`evaluate` run multi-tenant without seeing across scopes (source: team-knowledge-engine-security/README.md).

## Key Takeaways

- Markdown + git core is already team-ready; the work is a permission spine mirrored onto four surfaces (files, MCP runtime, git, memory DB)
- Permission unit = bundle; its `_master-index.md` access declaration is the single source of truth, generated onto every surface, never hand-maintained
- Shared memory = Postgres + pgvector with row-level security tagged by bundle/client (GBrain's and Team OS's shared conclusion)
- Acceptance gates: identity-stamped audit logs + zero-leak fuzz testing across all read paths
- Extends the existing per-bundle-worker isolation hard rule into a security boundary

## Related

- [[team-agentic-os-gbrain|Team Agentic OS — three-tier model and GBrain lineage]] — the source methodology this note applies to Atlas
- [[intelligence-layer-strand-map|Intelligence-layer strand map]] — where this design note sits among Max's intelligence-layer strands (T017)
- [[llm-vault-structure-spec|LLM Vault Structure Spec]] — the single-user architecture being scaled
- [[atlas-sync-architecture|Atlas Sync Architecture]] — the existing git-backed sync this extends

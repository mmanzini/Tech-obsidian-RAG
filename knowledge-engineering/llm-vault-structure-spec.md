---
type: synthesis
title: LLM Vault Structure Spec — Bundle/Topic/Article Architecture
description: "The design document that specified the Atlas vault's two-zone, two-layer architecture: one immutable input zone (`Resources/`) feeds one wiki zone (`Intelligence/`), with user-curated **bundles** as macro groups and agent-curated **topics** as clusters within bundles."
bundle: ai-engineering
topic: knowledge-engineering
tags: [knowledge-management, harness-engineering, agent-workflows, spec-driven-development]
source: ../../../Resources/documents/templates/LLM Vault structure.md
resource:
timestamp: 2026-05-17T08:21:13Z
status: active
related:
  - ai-engineering/knowledge-engineering/autoresearch-methodology.md
  - ai-engineering/knowledge-engineering/atlas-sync-architecture.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/knowledge-engineering/llm-wiki-schema-template.md
---

# LLM Vault Structure Spec — Bundle/Topic/Article Architecture

**Source:** [LLM Vault structure.md](../../../Resources/documents/templates/LLM Vault structure.md)

---

## Summary

The design document that specified the Atlas vault's two-zone, two-layer architecture: one immutable input zone (`Resources/`) feeds one wiki zone (`Intelligence/`), with user-curated **bundles** as macro groups and agent-curated **topics** as clusters within bundles. This spec defines the folder schemas, README flags, `_master-index.md` and `_index.md` conventions, wiki article format, image handling, and the consolidate/refine verbs — the precursor to the current `schema.md` and `CLAUDE.md`.

## The Design Intent

Extending Karpathy's Obsidian RAG pattern into a single vault-wide pipeline (source: LLM Vault structure.md). The previous pattern had per-vault duplication — each Intelligence subfolder owning its own `sources/` and `corpus/`. The new design centralises this into one `Resources/` zone and one `Intelligence/` wiki zone with a two-layer folder hierarchy.

## Two-Layer Hierarchy

**Bundles** (top-level inside `Intelligence/`, e.g. `tech-research/`, `political-economy/`) — user-created macro groups. Each has a `_master-index.md` declaring scope and listing topics. The agent never invents new bundles (source: LLM Vault structure.md).

**Topics** (one level deeper, e.g. `tech-research/agent-workflows/`) — agent-created clusters during `consolidate`. Topic creation is the agent's job; bundle creation is the human's (source: LLM Vault structure.md).

## Resources Subfolder README Flags

Two independent flags in each `Resources/<folder>/README.md` frontmatter (source: LLM Vault structure.md):

```yaml
include_in_consolidation: true   # false → folder skipped during consolidate entirely
delete_after_consolidation: true # false → sources stay after consolidation
```

- `include_in_consolidation: false` → dormant input, human-reference only.
- `delete_after_consolidation: false` → consolidated but originals kept (archival value).
- `delete_after_consolidation: true` → sources deleted after successful consolidation (ephemeral material like web clippings).
- Default if missing: both `true`.

## Index Schemas

**`_master-index.md`** (bundle level): scope paragraph + topics list with one-line descriptions (source: LLM Vault structure.md).

**`_index.md`** (topic level): cluster description + articles list + `Related Topics` block. Related Topics may link within the same bundle only (source: LLM Vault structure.md).

## Article Schema

Mirror of the current wiki article format: title, Source, Author, Summary, body sections with inline citations `(source: <path>)`, Key Takeaways, Related. Images embedded with `![[image.png]]` must live in the same topic folder (source: LLM Vault structure.md).

## Image Handling

When a source references an image (source: LLM Vault structure.md):
1. Locate the image beside the source file in `Resources/`.
2. Copy it into the article's topic folder — articles must be self-contained so deleting the source doesn't break embeds.
3. Reference with `![[image-filename.ext]]`.
4. If the same image is needed in multiple topics, copy it into each — storage cost is trivial; self-containment matters more.

## Consolidate Verb (Original Spec)

The spec's consolidate algorithm (source: LLM Vault structure.md):
1. Walk `Resources/`, skip `include_in_consolidation: false` folders.
2. Assign each source to the best-fit bundle(s) by comparing content against `_master-index.md` scope paragraphs. No fit → quarantine to `_unsorted/`.
3. Within the bundle, pick the best-fit topic. No fit → create a new topic with `_index.md`, register in `_master-index.md`.
4. Write/update the article using the schema.
5. Update `_index.md`, `_master-index.md`, `Intelligence/index.md` as needed.
6. Append to `Intelligence/log.md`.
7. Delete sources per `delete_after_consolidation` flag (only after steps 1–6 succeed).

## Scope and Limitations Surfaced

The spec explicitly asks two open questions before generating the starter vault (source: LLM Vault structure.md):
- Which domain to seed the example bundles with.
- Whether duplicated articles across bundles should track each other (e.g. shared front-matter ID) or stay fully independent copies.

The current Atlas implementation chose: fully independent copies with no cross-bundle tracking.

## Key Takeaways

- The two-zone, two-layer design separates human curation (bundles) from agent curation (topics and articles).
- README flags (`include_in_consolidation`, `delete_after_consolidation`) are the orchestration control surface for each input folder.
- The same article can live in multiple bundles — copying is preferred over cross-bundle linking.
- `_master-index.md` scope paragraphs are the routing signal; they must be precise enough to route new sources correctly.
- This spec is the direct precursor to the current `schema.md` and `CLAUDE.md`.

## Related

- [[autoresearch-methodology|Autoresearch Methodology]] — the Karpathy pattern this vault extends
- [[atlas-sync-architecture|Atlas Sync Architecture]] — how the vault's content syncs to public repos
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — how the harness layer (CLAUDE.md) configures agents at runtime
- [[llm-wiki-schema-template|LLM Wiki Schema Template]] — an earlier, simpler wiki CLAUDE.md template this spec supersedes

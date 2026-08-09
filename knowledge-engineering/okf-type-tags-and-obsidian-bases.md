---
type: synthesis
title: Type vs tags, content-tag backfill, and Obsidian Bases as the OKF consumption layer
description: How Atlas separates OKF type (the required conceptual kind) from tags (a per-bundle content facet), the 613-article content-tag backfill, Obsidian Bases as the consumption-time property view, and the silent-YAML-failure landmine that nearly broke all of it.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- okf
- knowledge-engineering
- frontmatter
- metadata
- interoperability
sources:
- id: session-2026-06-27-1531
  resource: Resources/context/session-2026-06-27-1531.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-06-27T20:00:00Z'
status: stable
related:
- ai-engineering/knowledge-engineering/open-knowledge-format.md
- ai-engineering/knowledge-engineering/okf-spec-v0-1.md
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
---

# Type vs tags, content-tag backfill, and Obsidian Bases as the OKF consumption layer

**Source:** [session-2026-06-27-1531.md](../../../Resources/context/session-2026-06-27-1531.md), [session-2026-06-27-1634.md](../../../Resources/context/session-2026-06-27-1634.md)
**Author:** Max + Atlas (consolidation session, 27 June 2026)

---

## Summary

Two OKF frontmatter fields do different jobs and must not be conflated: `type` is a property — the conceptual *kind* of an article, the one field OKF requires — while `tags` contextualise the *content* of the article as a facet, drawn from a per-bundle controlled vocabulary. This article records that distinction, the 613-article content-tag backfill that implemented it, the choice of Obsidian Bases as the consumption-time view that reads `type`, and the silent YAML-parse failure that had been quietly destroying frontmatter across a third of the wiki (source: session-2026-06-27-1531.md, session-2026-06-27-1634.md).

## Type ≠ tags

`type` classifies the kind of concept (synthesis, digest, analysis, repo-writeup, …) and is the only field OKF mandates; it is filtered at consumption time by a property view, not by tag navigation. `tags` are a content facet — synthesised from the subject/themes of the article — and must **never mirror the type** into the tag set. Keeping them orthogonal means the type axis stays a clean, single-valued classification while tags stay a navigable thematic facet. `schema.md` was left untouched by this work; the distinction is a consolidation/program convention, not a harness change (source: session-2026-06-27-1531.md, session-2026-06-27-1634.md).

## The Atlas type distribution

A discovery scan found 629 articles, all carrying a `type`, across 9 types: synthesis 179, repo-writeup 147, mechanic 89, analysis 73, digest 55, narrative 36, reference 35, profile 13, rules 1. Before this work, 97.5% of articles had empty `tags` — the type axis was complete but the content facet was effectively unused, and although the Obsidian Bases core plugin was enabled, zero `.base` files were in use (source: session-2026-06-27-1531.md).

## The content-tag backfill

The fix was a per-bundle controlled-vocabulary backfill: each bundle's `index.md` gained a `## Tag vocabulary` block (the stable controlled set), then every article received 3–6 lowercase-kebab content tags drawn from that set, additive to any existing tags (e.g. the `daily` bundle's existing `daily` tag was preserved). The run shipped 613 articles tagged and 16 skipped. The pipeline parallelised per bundle — vocabulary blocks first (≈12 agents), then tagging chunks within each bundle (32+ agents) — idempotent, and benefiting from staged validation before fanning out, at a cost of roughly 4M tokens for the 613 articles. This validated the incremental per-bundle pilot plus fresh-agent-validation pattern for large backfills (source: session-2026-06-27-1531.md, session-2026-06-27-1634.md).

## Obsidian Bases as the consumption layer

Type-filtering is done with an Obsidian **Base** — a consumption-time property view that scans the `type` frontmatter and is robust and self-updating, rather than navigating by tags. An optional secondary path is graph property-query colour groups, which require Obsidian build support for `["type":"x"]` syntax and degrade harmlessly if unsupported. The Base is the durable answer: it reads the property directly, so it stays correct as articles are added (source: session-2026-06-27-1531.md, session-2026-06-27-1634.md).

## The silent-YAML landmine

The work exposed a serious data-quality issue: 214 of 683 articles (31%) had invalid YAML frontmatter — unquoted `title:`/`description:` scalars containing `: ` (e.g. `title: The Modern Elder: crystallized intelligence`). Obsidian silently drops *all* properties on a parse failure, so any property-based view (Base, graph colouring, tag pane) would have been blind to a third of the wiki, with no error surfaced. The `okf_tools --check` conformance test used a lenient parser and therefore never caught it. 213 files were fixed (0 invalid remaining), and a new standing task — **T045** — was opened to harden `okf_tools --check` to flag strict-YAML-invalid scalars (unquoted colons, leading `*`) and to quote scalars at write time so the failure cannot recur silently. The general lesson: any property-based workflow depends on strict YAML validity, and a lenient validator gives false confidence (source: session-2026-06-27-1531.md, session-2026-06-27-1634.md).

## Key Takeaways

- `type` (required, conceptual kind, filtered by a property view) and `tags` (per-bundle content facet) are orthogonal — never mirror type into tags.
- 629 articles span 9 types; a per-bundle controlled-vocabulary backfill added 3–6 content tags to 613 of them (16 skipped), parallelised per bundle at ≈4M tokens.
- Obsidian Bases (consumption-time property scan) is the durable type-filtering surface, not tag navigation.
- Invalid YAML (unquoted colons in `title:`/`description:`) makes Obsidian silently drop all properties; it hit 31% of the wiki and the lenient `okf_tools --check` missed it — hardening it is T045.

## Related

- [[open-knowledge-format]] · [Open Knowledge Format (OKF)](open-knowledge-format.md) — the spec and the type-as-only-required-field principle this builds on
- [[okf-spec-v0-1]] · [OKF v0.1 — normative specification](okf-spec-v0-1.md) — the conformance tests `okf_tools --check` implements
- [[llm-vault-structure-spec]] · [LLM Vault Structure Spec](llm-vault-structure-spec.md) — the bundle/topic/article architecture the tag vocabularies attach to

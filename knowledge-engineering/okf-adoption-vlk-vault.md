---
type: synthesis
title: OKF Adoption — VLK Work Vault Frontmatter and Interoperability Layer
description: The VLK work vault (the Atlas fork Max ran at Van Lanschot Kempen) adopted the Open Knowledge Format as a machine-readable interoperability surface, backfilling structured frontmatter across all articles, standardising router filenames, and adding conformance tooling.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- okf
- knowledge-management
- harness-engineering
- interoperability
sources:
- id: session-2026-06-25-1508
  resource: Resources/context/session-2026-06-25-1508.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-07T13:17:47Z'
status: stable
related:
- ai-engineering/knowledge-engineering/open-knowledge-format.md
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
- ai-engineering/knowledge-engineering/llm-wiki-schema-template.md
---

# OKF Adoption — VLK Work Vault Frontmatter and Interoperability Layer

**Source:** `session-2026-06-25-1508.md` (VLK work-vault capture; imported from the vlk Intelligence vault 2026-08-07)

---

## Summary

The VLK work vault — the Atlas fork Max ran at Van Lanschot Kempen — adopted the Open Knowledge Format (OKF) as its machine-readable interoperability surface, layering structured YAML frontmatter on top of the existing human-readable articles rather than replacing them. The change backfilled frontmatter across the whole wiki, standardised router filenames, added per-bundle logs, and introduced conformance tooling, all while preserving the vault's VLK-specific adaptations (source: session-2026-06-25-1508.md).

## What Changed

- **OKF frontmatter spec adopted** — every article gains a structured block with `type`/`title`/`description`/`bundle`/`topic`/`tags`/`source`/`resource`/`timestamp`/`status`/`related`, making articles parseable by agents, the search indexer, Dataview, and external OKF consumers (source: session-2026-06-25-1508.md).
- **Dual-link channels** — the frontmatter `related:` array and relative-path markdown links act as a portable graph that may cross bundles, alongside the same-bundle-only `[[ ]]` wiki-link channel the query walk follows (source: session-2026-06-25-1508.md).
- **Per-bundle `log.md`** — eight new per-bundle log files were added (source: session-2026-06-25-1508.md).
- **Conformance tooling** — `okf_tools.py` was added for conformance checking (source: session-2026-06-25-1508.md).
- **Router rename strategy** — `_master-index.md` and `_index.md` were renamed to `index.md` only in the nine real bundles, while governance folders (`_search`, `_episodes`, `_eval`) keep `_index.md` (source: session-2026-06-25-1508.md).

## Migration Scope

- 341 articles backfilled with OKF frontmatter; 60 routers renamed; 8 new per-bundle `log.md` files; zero new link breakage introduced (source: session-2026-06-25-1508.md).
- **Key technical discovery:** pre-existing `---` horizontal rules in articles were not YAML frontmatter, so the backfill had to insert frontmatter above the article titles rather than reuse the existing rules (source: session-2026-06-25-1508.md).

## Preserved VLK Adaptations

The harness merge kept the VLK-specific deviations from the upstream pattern: the `.github/hooks/` paths, the `Tasks/` working zone, and the push-to-main Git rule (source: session-2026-06-25-1508.md).

## Verification and Deployment Posture

- Verification used `okf=conformant` checks plus a wiki-link audit with Obsidian suffix-matching logic; one pre-existing dangling cross-bundle link was noted as out-of-scope (source: session-2026-06-25-1508.md).
- Deployment respected the workspace guardrails: changes were not pushed automatically, awaiting confirmation (source: session-2026-06-25-1508.md).

## Key Takeaways

- OKF is additive: structured frontmatter makes the wiki machine-parseable without disturbing the human-readable body or the citation discipline.
- The migration was large but link-safe (341 articles, 60 routers, zero new breakage), with the `---`-horizontal-rule discovery being the main backfill gotcha.
- The same OKF migration ran independently in both vaults (Atlas main and the VLK fork), confirming the format's portability across forks of the same harness.

## Related

- [[open-knowledge-format]] · [Open Knowledge Format](open-knowledge-format.md) — the OKF spec both vaults adopted
- [[llm-vault-structure-spec]] · [LLM Vault Structure Spec](llm-vault-structure-spec.md) — the vault architecture spec this OKF layer extends with a machine-readable frontmatter surface
- [[llm-wiki-schema-template]] · [LLM Wiki Schema Template](llm-wiki-schema-template.md) — the simpler upstream wiki template; OKF formalises its interoperability surface

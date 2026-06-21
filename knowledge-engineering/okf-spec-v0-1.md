---
type: reference
title: OKF v0.1 — normative specification
description: The structural rules of Open Knowledge Format v0.1 — bundle layout, reserved filenames, concept documents, cross-linking, and the three conformance requirements.
bucket: ai-engineering
topic: knowledge-engineering
tags: [okf, specification, conformance, knowledge-engineering, interoperability, markdown, frontmatter]
source: Resources/web-clippings/2026-06-21-knowledge-catalogokfSPEC.md at main.md
resource: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
timestamp: 2026-06-21T22:30:00Z
status: active
related:
  - ai-engineering/knowledge-engineering/open-knowledge-format.md
  - ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
---

# OKF v0.1 — normative specification

**Source:** [knowledge-catalog/okf/SPEC.md at main](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
**Author:** Google Cloud Platform (knowledge-catalog)
**Version:** 0.1 — Draft

---

## Summary

This is the normative reference for Open Knowledge Format v0.1 — the exact structural rules a directory of markdown files must follow to be a conformant OKF bundle. The companion article [[open-knowledge-format]] covers the *why* and the lineage; this one is the *what*: bundle layout, reserved filenames, concept-document shape, cross-linking forms, and the three conformance tests. The spec is deliberately minimal — "if you can `cat` a file, you can read OKF; if you can `git clone` a repo, you can ship it" (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Bundle structure

A **bundle** is a directory tree of markdown files; the layout is independent of the domain (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md):

```
path/to/bundle/
├── index.md          # Optional — directory listing for progressive disclosure
├── log.md            # Optional — chronological update history
├── <concept>.md      # A concept at the bundle root
└── <subdirectory>/   # Subdirectories group concepts
    ├── index.md
    ├── <concept>.md
    └── <subdirectory>/
```

A bundle MAY be distributed as a git repository (recommended — history, attribution, diffs), a tarball/zip, or a subdirectory within a larger repository (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Reserved filenames

Only two filenames carry defined meaning at any level and MUST NOT be used for concept documents (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md):

| Filename | Purpose |
|---|---|
| `index.md` | Directory listing (§6) |
| `log.md` | Update history (§7) |

**All other `.md` files are concept documents** (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md). OKF specifies no separate file format for tag aggregation — a tag-browsing view is synthesised at consumption time by scanning frontmatter (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Concept documents (`<concept>.md`)

`<concept>.md` is a metavariable, not a literal filename: every non-reserved markdown file *is* a concept document. A **concept** is a single unit of knowledge — a tangible asset (a table, an API), an abstract idea (a metric, a process), or anything between. A concept's **ID** is its file path within the bundle minus the `.md` suffix (e.g. `tables/users.md` → `tables/users`) (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

Each concept file has two parts: a YAML frontmatter block delimited by `---`, then a free-form markdown body (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

**Frontmatter — the only required field is `type`** (a short, self-explanatory string identifying the kind of concept; values are not centrally registered and consumers MUST tolerate unknown types). Recommended-but-optional, in priority order: `title`, `description`, `resource` (canonical URI for the underlying asset), `tags`, `timestamp` (ISO 8601). Producers MAY add arbitrary keys; consumers SHOULD preserve unknown keys and SHOULD NOT reject documents with unrecognised fields (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

**Body** — standard markdown, favouring structure (headings, lists, tables, code blocks) over prose. No section is required; three headings carry conventional meaning when applicable: `# Schema`, `# Examples`, `# Citations` (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Cross-linking

Concepts link to other concepts with standard markdown links, in two forms (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md):

- **Absolute (bundle-relative)** — begins with `/`, resolved from the bundle root, e.g. `/tables/customers.md`. The *recommended* form: stable when documents move within a subdirectory.
- **Relative** — ordinary relative paths, e.g. `./other.md`.

A link asserts an untyped, directed relationship; the kind of relationship is conveyed by surrounding prose, not the link. Consumers MUST tolerate broken links — a missing target may just be not-yet-written knowledge (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Index and log files

`index.md` (any directory, including root) enumerates contents for progressive disclosure. It carries **no frontmatter** — except the bundle-root `index.md`, the one place an `okf_version: "0.1"` declaration is permitted. Its body groups entries under headings, each entry a link plus the linked concept's description. May be generated or synthesised on the fly (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

`log.md` (any level, optional) records that scope's change history: date-grouped entries newest-first, ISO 8601 `YYYY-MM-DD` headings, prose entries with a conventional leading bold word (`**Update**`, `**Creation**`, `**Deprecation**`) (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Conformance

A bundle is conformant with OKF v0.1 if and only if (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md):

1. Every non-reserved `.md` file contains a parseable YAML frontmatter block.
2. Every frontmatter block contains a non-empty `type` field.
3. Every reserved filename (`index.md`, `log.md`) follows its specified structure when present.

Everything else is soft guidance. Consumers MUST NOT reject a bundle for missing optional fields, unknown `type` values, unknown frontmatter keys, broken cross-links, or missing `index.md` files — the permissive consumption model is intentional so bundles stay useful as they grow and are partially agent-generated (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## How Atlas maps to the spec

Atlas is a conformant producer that *exceeds* the spec on governance. The mapping (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md):

| OKF construct | Atlas |
|---|---|
| Bundle | each `Intelligence/<bucket>/` is an in-place bundle |
| `<concept>.md` | every `<topic>/<article>.md` |
| Reserved `index.md` | bucket and topic routers |
| Reserved `log.md` | per-bucket changelog derived from `log.tsv` |
| Required `type` | present on every article (Atlas-defined vocabulary) |
| Cross-link | the `related:` frontmatter array (may cross buckets) |

Atlas keeps Obsidian `[[ ]]` wikilinks as a same-bucket-only channel on top of the portable relative/`related:` graph — additional to the spec, not required by it. Governance Atlas adds beyond OKF: drift counting, episodic memory, and the eval harness (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Versioning

Future revisions use `<major>.<minor>`: minor bumps add backward-compatible options; major bumps may rename required fields or reserved filenames. A bundle MAY declare its target version via `okf_version` in the root `index.md` frontmatter; consumers that don't understand a declared version SHOULD attempt best-effort consumption (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Key Takeaways

- `<concept>.md` is a placeholder — **any** non-reserved markdown file is a concept document; there is no file literally named `<concept>.md` and none is missing from a bundle that uses real article names (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).
- The entire conformance bar is three rules, and only one frontmatter field (`type`) is required (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).
- Permissive consumption (tolerate broken links, unknown types, missing indexes) is a deliberate design choice, not a gap (source: 2026-06-21-knowledge-catalogokfSPEC.md at main.md).

## Related

- [[open-knowledge-format]] · [Open Knowledge Format (OKF)](open-knowledge-format.md) — the motivation, lineage, and Atlas alignment narrative this spec underpins
- [[llm-vault-structure-spec]] · [LLM Vault Structure Spec](llm-vault-structure-spec.md) — Atlas's own architecture spec that OKF conformance layers onto

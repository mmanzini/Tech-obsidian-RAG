---
type: synthesis
title: LLM Wiki Schema Template — Simple Single-Vault CLAUDE.md
description: A minimal CLAUDE.md template for a single-topic LLM wiki maintained by Claude Code, based on Andrej Karpathy's LLM Wiki pattern.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- knowledge-management
- claude-code
- agent-workflows
- harness-engineering
sources:
- id: llm-wiki-schema-template-claude
  resource: ../../../Resources/documents/templates/LLM_wiki_schema_template_CLAUDE.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-17T08:21:13Z'
status: stable
related:
- ai-engineering/knowledge-engineering/autoresearch-methodology.md
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
- ai-engineering/knowledge-engineering/zettelkasten-pkm.md
---

# LLM Wiki Schema Template — Simple Single-Vault CLAUDE.md

**Source:** [LLM_wiki_schema_template_CLAUDE.md](../../../Resources/documents/templates/LLM_wiki_schema_template_CLAUDE.md)

---

## Summary

A minimal CLAUDE.md template for a single-topic LLM wiki maintained by Claude Code, based on Andrej Karpathy's LLM Wiki pattern. Uses a flat `raw/` + `wiki/` two-folder structure with a simple ingest workflow: read source, discuss with user, write summary page, create/update concept pages, add wiki-links, update index and log. Simpler than the full bundle/topic/article Atlas architecture — intended for focused single-domain wikis (the template example is a Japan trip planner).

## Structure

Two folders (source: LLM_wiki_schema_template_CLAUDE.md):

```
raw/          -- source documents (immutable — never modify these)
wiki/         -- markdown pages maintained by Claude
wiki/index.md -- table of contents for the entire wiki
wiki/log.md   -- append-only record of all operations
```

## Ingest Workflow

When the user adds a new source and asks to ingest it (source: LLM_wiki_schema_template_CLAUDE.md):

1. Read the full source document.
2. Discuss key takeaways with the user before writing anything.
3. Create a summary page in `wiki/` named after the source.
4. Create or update concept pages for each major idea or entity.
5. Add wiki-links (`[[page-name]]`) to connect related pages.
6. Update `wiki/index.md` with new pages and one-line descriptions.
7. Append an entry to `wiki/log.md` with the date, source name, and what changed.

A single source may touch 10–15 wiki pages — that is normal.

## Page Format

Every wiki page follows this structure (source: LLM_wiki_schema_template_CLAUDE.md):

```markdown
# Page Title

**Summary**: One to two sentences.
**Sources**: List of raw source files this page draws from.
**Last updated**: Date of most recent update.

---

Main content. Use clear headings and short paragraphs.
Link to related concepts using [[wiki-links]] throughout the text.

## Related pages

- [[related-concept-1]]
- [[related-concept-2]]
```

## Citation Rules

- Every factual claim should reference its source file: `(source: filename.pdf)`.
- If two sources disagree, note the contradiction explicitly.
- If a claim has no source, mark it as needing verification (source: LLM_wiki_schema_template_CLAUDE.md).

## Question Answering

When the user asks a question (source: LLM_wiki_schema_template_CLAUDE.md):
1. Read `wiki/index.md` first.
2. Read relevant pages and synthesise an answer.
3. Cite specific wiki pages.
4. If the answer is not in the wiki, say so clearly.
5. If the answer is valuable, offer to save it as a new wiki page.

## Hard Rules

- Never modify anything in `raw/`.
- Always update `wiki/index.md` and `wiki/log.md` after changes.
- Page names: lowercase-with-hyphens.
- When uncertain about categorisation, ask the user.

## Comparison with Atlas Architecture

This template is the simpler predecessor to the full Atlas bundle/topic/article pattern (source: LLM_wiki_schema_template_CLAUDE.md). Key differences:

| Dimension | LLM Wiki Template | Atlas |
|-----------|-------------------|-------|
| Scope | Single domain | Multi-bundle, multi-domain |
| Hierarchy | Flat (`wiki/`) | Bundle → Topic → Article |
| Input zone | `raw/` (no subfolders) | `Resources/<folder>/` with README flags |
| Log | `wiki/log.md` (prose) | `Intelligence/log.tsv` (TSV) |
| Routing | Manual per-page creation | Agent-driven bundle/topic assignment |
| Human-agent split | Human decides what to ingest | Agent decides bundle, topic, article |

The template is appropriate for focused, single-topic use cases. The Atlas architecture is appropriate for persistent, multi-domain, long-running knowledge bases.

## Key Takeaways

- The LLM Wiki template is Karpathy's original pattern: two folders, ingest-on-demand, discuss before writing.
- Atlas extends this with multi-bundle routing, structured indexes, autonomous consolidation, and TSV logging.
- The core invariant is the same: `raw/` (or `Resources/`) is immutable; the wiki is agent-maintained.
- For small personal projects, the template's simplicity is a feature; for a growing knowledge base, the Atlas structure pays off.

## Related

- [[autoresearch-methodology|Autoresearch Methodology]] — the Karpathy training experiment that established the `program.md` harness pattern
- [[llm-vault-structure-spec|LLM Vault Structure Spec]] — the full spec extending this template to multi-bundle architecture
- [[zettelkasten-pkm|Zettelkasten — Foundational PKM Methodology]] — the historical predecessor to wiki-linked knowledge bases

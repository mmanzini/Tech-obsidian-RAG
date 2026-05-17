# Karpathy's LLM Knowledge Bases — Personal Workflow

**Source:** [Thread by @karpathy](https://x.com/karpathy/status/2039805659525644595)
**Author:** Andrej Karpathy
**Date:** 2026-04-02

---

## Summary

Karpathy describes his personal workflow for building LLM-powered knowledge bases: raw source documents are fed into a `raw/` directory, an LLM incrementally "compiles" a wiki of markdown articles with summaries and backlinks, and Obsidian serves as the human-readable frontend. This thread is the upstream inspiration for the Atlas vault pattern itself — the Atlas autoresearch architecture is a direct descendant.

## Workflow Stages

### Data Ingest

Source documents are indexed into a `raw/` directory. The LLM incrementally compiles a wiki — markdown files containing summaries, backlinks, and concept articles — from this raw material (source: 2026-04-30-Thread by @karpathy.md).

### IDE / Frontend

Obsidian serves as the frontend for viewing raw data, the compiled wiki, and derived visualisations. The LLM writes and maintains all wiki data; the human rarely touches it directly. Supporting tools include the Obsidian Web Clipper extension for ingest and Marp for generating slideshows (source: 2026-04-30-Thread by @karpathy.md).

### Q&A / Research

Once the wiki reaches sufficient size (roughly 100 articles, ~400K words), the LLM can answer complex questions by researching the wiki. Index files and brief per-article summaries allow the LLM to navigate the corpus without requiring a fancy RAG retrieval stack (source: 2026-04-30-Thread by @karpathy.md).

### Output Generation

Outputs include rendered markdown, Marp slideshows, and matplotlib images — all viewable inside Obsidian. Generated outputs are filed back into the wiki to iteratively enrich it (source: 2026-04-30-Thread by @karpathy.md).

### Linting / Health Checks

The LLM performs periodic audits: finding inconsistent data, imputing missing information via web search, discovering interesting connections, and suggesting new article candidates (source: 2026-04-30-Thread by @karpathy.md).

### Extra Tooling

A vibe-coded search engine (web UI + CLI) provides an additional interface over the wiki for both the human and the LLM-as-tool (source: 2026-04-30-Thread by @karpathy.md).

### Future Direction

Karpathy flags synthetic data generation and finetuning as the next frontier: once the wiki is large enough, use it to finetune an LLM so the knowledge lives in model weights, not just retrieval (source: 2026-04-30-Thread by @karpathy.md).

## TLDR Architecture

`raw data → LLM-compiled wiki → LLM operates on it via CLIs → all viewable in Obsidian`. Karpathy's closing note: *"There is room here for an incredible new product."* (source: 2026-04-30-Thread by @karpathy.md)

## Key Takeaways

- Index files + brief summaries are sufficient for LLM navigation — expensive vector RAG is not required for a well-structured wiki.
- The human's role is curator of raw inputs and harness configuration; the LLM maintains the wiki itself.
- Linting (consistency, gap-finding, connection-surfacing) is a first-class LLM task, not an afterthought.
- Obsidian is the rendering layer, not the data layer — the LLM owns the data.
- Finetuning on the compiled wiki is the logical end-state: knowledge moves from retrieval into weights.

## Related

- [[autoresearch-methodology]] — Karpathy's autoresearch pattern (overnight GPU loops, program.md steering); Atlas's direct precursor
- [[llm-vault-structure-spec]] — the architectural spec that formalises bucket/topic/article layers derived from this workflow
- [[llm-wiki-schema-template]] — the minimal flat wiki template (raw/ + wiki/) that Atlas extends
- [[zettelkasten-pkm]] — foundational PKM methodology underlying the linked-article pattern
- [[atlas-sync-architecture]] — Atlas's concrete implementation of this pattern across multiple repos

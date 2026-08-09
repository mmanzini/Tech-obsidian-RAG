---
type: synthesis
title: Team Agentic OS — the three-tier file model and GBrain lineage
description: A blueprint for scaling a single-user agentic OS — a set of folders and files that inject the right context at the right moment — into a team system that is "shared where it should be, private where it shouldn't." It borrows from Garry Tan's GBrain and from software-architecture principles (separate what changes often from what stays stable), and rests on three tiers (human-edited / agent-edited / version control), access control across four systems, and a markdown-first substrate that avoids vendor lock-in.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- knowledge-management
- ai-org-design
- agent-memory
- context-engineering
sources:
- id: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use
  resource: ../../../Resources/transcriptions/you-tube/transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-06-05T05:02:31Z'
status: stable
related:
- ai-engineering/knowledge-engineering/agentic-knowledge-engine-team-security.md
- ai-engineering/knowledge-engineering/atlas-sync-architecture.md
- ai-engineering/knowledge-engineering/karpathy-llm-knowledge-bases.md
---

# Team Agentic OS — the three-tier file model and GBrain lineage

**Source:** [transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md](transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md)
**Author:** Simon Scrapes (YouTube)
**Video:** https://www.youtube.com/watch?v=TE6zNesGcvY

---

## Summary

A blueprint for scaling a single-user agentic OS — a set of folders and files that inject the right context at the right moment — into a team system that is "shared where it should be, private where it shouldn't." It borrows from Garry Tan's GBrain and from software-architecture principles (separate what changes often from what stays stable), and rests on three tiers (human-edited / agent-edited / version control), access control across four systems, and a markdown-first substrate that avoids vendor lock-in. This is the external-methodology companion to Atlas's own knowledge-engine design.

## What an agentic OS is

Underneath the jargon it is just a set of folders and files that manage context — telling the model to inject brand voice, client details, or rules only when needed (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md). It exists because an out-of-the-box LLM is forgetful under load (context rot) and has no long-term recall of the business, clients, voice, or past decisions. The framing: the model provides intelligence; the OS provides memory and the judgment about what to load when (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).

## The three-tier file model

Where a file lives depends on who maintains it (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md):

1. **Human-maintained → Notion / Google Drive (markdown only).** The source of truth for human edits — global rules (`CLAUDE.md`), brand context. Chosen because it is where teams already work and is pleasant to edit. Limited to markdown; scripts and system files do not go here (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).
2. **Agent-maintained → inside Claude Code, backed by GitHub.** Files the agents update and that do not need to face the team — settings, memory files, and **skills** (the one human-facing exception, kept in Claude Code because YAML front-matter formatting can break when passed through Notion/Drive) (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).
3. **GitHub → version control + backup of everything**, including files pulled down from Notion/Drive. Non-technical members need never touch it (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).

Shared global rules flow down from Notion, but a user can override or extend them locally in a gitignored `CLAUDE.local.md`. Outputs are version-controlled in GitHub and optionally pushed back into Notion as read-only displays for non-technical members (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).

## Access control across four systems

Each surface is a separate boundary and must be gated independently, all copying the shared-drive permissions (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md):

1. **Shared drive** = source of truth: per-doc view/edit rights; your login only sees spaces you were added to.
2. **Local Claude Code** = holds only what the Notion/Drive token was allowed to pull — least privilege by construction.
3. **GitHub** = a *separate* permission system; Notion permissions do nothing here. Use **one repo per client**, scoped to exactly that client's people; git membership must mirror drive membership.
4. **Memory database** (if used): either one index per person (free isolation, no shared brain) or — the scalable option — a shared **Supabase Postgres** with **row-level security** tagged by client, refusing rows the asker cannot access (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).

## No vendor lock-in

The whole OS underneath is just markdown files and folders, so it is portable across harnesses — build infrastructure you can move (source: transcript-how-to-build-an-agentic-os-your-whole-team-can-actually-use.md).

## Key Takeaways

- An agentic OS is folders/files that manage context injection; the model supplies intelligence, the OS supplies memory + judgment about what to load when
- Three tiers by maintainer: human (Notion/Drive, markdown) → agent (Claude Code, GitHub-backed; skills live here) → GitHub (backup + version control)
- Four independent permission surfaces, all mirroring the shared drive: drive, local checkout, GitHub (one repo per client), memory DB
- Shared Postgres + row-level security (tagged by client) is the scalable shared-memory answer — the same conclusion GBrain reaches
- Markdown-first substrate = portability, no vendor lock-in

## Related

- [[agentic-knowledge-engine-team-security|Agentic knowledge engine — team security & permissions]] — applies this model (plus GBrain's rigour) to scaling Atlas for teams
- [[atlas-sync-architecture|Atlas Sync Architecture]] — Atlas's own git-backed two-location pattern
- [[karpathy-llm-knowledge-bases|Karpathy's LLM Knowledge Bases]] — the personal-scale precursor to a shared brain

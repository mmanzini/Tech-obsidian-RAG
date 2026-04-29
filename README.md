# Tech Research

A curated, LLM-compiled wiki on how to build well with AI.

This repository is a public mirror of one bucket from a personal Obsidian RAG vault (Atlas). Articles are distilled from primary sources — talks, papers, blog posts, podcast episodes, internal experiments — into concise, cross-linked notes with inline source citations.

## What's in here

- **Agent architecture and workflows** — coordination patterns, structured methodologies (RPI, spec-driven development, adversarial review), and the practical patterns of doing AI-assisted product and engineering work.
- **Harness engineering** — configuring the runtime around coding agents: CLAUDE.md, MCP, skills, sub-agents, hooks, long-running patterns, eval workflows, and routines.
- **AI and the practice of work** — how AI changes team and organisation design, the PM craft, and design tooling for agents.
- **AI security and defensive patterns** — vulnerability management, zero trust, incident response in an AI-accelerated threat landscape.
- **AI-native verticals** — banking, open-data apps, and other domain-specific patterns.
- **Long-form podcast and interview signal** — episode-level distillation of Lenny's Podcast and adjacent shows for PM and engineering leaders.

## Browsing

The folders at the root of this repo are the topic clusters. Each topic folder contains:

- `_index.md` — a one-line description of every article in that topic.
- The article files themselves (`.md`).
- Any embedded images or documents referenced inline by those articles.

`_master-index.md` at the root lists every topic with a short description. Start there if you want a map; otherwise just browse the folder tree.

## Method

Sources go through a structured `consolidate` pass: each article cites its source files inline, contradictions between sources are surfaced rather than smoothed over, and a separate `refine` pass audits the wiki for orphans, broken links, and schema violations. The full method lives in the parent vault and is not published here.

## Updates

This repo syncs from the parent vault on a schedule — content is added or revised whenever the upstream notes change. The history of changes is visible in the commit log.

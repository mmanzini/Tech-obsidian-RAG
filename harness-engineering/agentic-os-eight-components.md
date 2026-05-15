# Agentic OS — Eight-Component Architecture (Simon Scrapes)

**Source:** [transcript-creating-your-own-agentic-os-is-easy.md](../../../Resources/web-clippings/transcript-creating-your-own-agentic-os-is-easy.md)
**Channel:** Simon Scrapes — "Creating Your Own Agentic OS is Easy (Insanely Powerful)"

---

## Summary

A practitioner's breakdown of an Agentic Operating System (Agentic OS) as eight interlocking components — structured context management under the AI tool rather than in it. The core thesis: consistent high-quality outputs 90% of the time come from building the right system underneath the model, not from better prompting or a smarter model.

## The Core Thesis

Two people using the same AI tools get different outcomes because one built a system underneath the tool and the other didn't. An Agentic OS is "just clever context management — folders, files, and a structure that tells your AI tool exactly what it needs to know, exactly when it needs to know it." None of this is code; it's organisational structure. (source: transcript-creating-your-own-agentic-os-is-easy.md)

## The Eight Components

### 1. Static Context (You & Your Business)
Every agentic tool reads an identity file first (CLAUDE.md in Claude Code, agents.md in Codex, soul.md in Open Claw). Split into: **user.md** (who you are, how you work) and **personality.md** (how the agent responds). Brand context (voice, ICP, positioning) lives separately and is referenced by skills when relevant. Don't write these from scratch — have Claude interview you with 15 questions. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 2. Improved Memory
Six levels: (1) CLAUDE.md static rules; (2) session-start hook for deterministic injection; (3) semantic search (MemSearch, claude-mem) for meaning-based recall — the "80/20" level; (4) verbatim recall (MemPalace) for exact-phrasing use cases; (5) knowledge bases; (6) cross-tool memory. Levels 1+2+3 cover most business needs. Context rot = the quality degradation that happens as a conversation window fills. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 3. Repeatable Processes (Skills)
Skills should be short and modular (< 200 lines) with progressive disclosure — name/description loads first, full skill.md loads next, additional context loads only when needed at a specific step. Skills reference shared brand context rather than guessing. Add a learnings.md self-improvement loop: skill reads feedback before running and prompts for feedback after. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 4. Multi-Step Workflows on a Schedule (Skill Systems)
Chain skills into pipelines (skill systems) with human-in-the-loop steps. A skill system is a meta-skill that orchestrates multiple skills in sequence: skill A completes → human review → skill B continues. Schedule these via Claude Desktop scheduled tasks. Don't think of skills as isolated; think of them as modular pipeline components. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 5. Planning That Matches the Project
Three levels: (1) built-in shift-tab planning for simple tasks; (2) a PRD-generating framework for half-day to multi-day projects with scoped files and tick-box tasks; (3) GSD (Get Sh*t Done) meta-prompting framework for complex work — plan each phase, execute, verify. Context rot kills large projects; phased planning injects the right context at the right time. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 6. Managing Projects & Clients
Multi-client architecture: one root CLAUDE.md as parent passes down shared methodology. Each client folder has its own CLAUDE.md (overrides parent), client brand context, and agent context (memory + learnings per client). Claude Code inherits context from parent folders by working directory. Skills stay at root level with a working copy per client. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 7. Output Consolidation
By default, Claude stores outputs wherever it wants. Fix: a `projects/` folder structure per project per skill, so outputs land in predictable paths. Skill-level outputs in `projects/{client}/{skill}/`; multi-skill brief outputs in `projects/{client}/briefs/{brief-name}/`. (source: transcript-creating-your-own-agentic-os-is-easy.md)

### 8. Access Anywhere
Two parts: (1) run the system on a VPS or Claude Cloud, not on a laptop, for uninterrupted scheduled tasks; (2) message it via Anthropic's built-in channels feature (Telegram/Discord) to interact without opening a terminal. Claude Code, Codex, Open Claw, Hermes all support mobile messaging interfaces. (source: transcript-creating-your-own-agentic-os-is-easy.md)

## Key Takeaways

- An Agentic OS is structured context management: folders and files that tell the AI what to know and when to know it — not code (source: transcript-creating-your-own-agentic-os-is-easy.md)
- Memory levels 1+2+3 cover most needs; 4–6 are optional bolt-ons for specific cases (source: transcript-creating-your-own-agentic-os-is-easy.md)
- Skills should be < 200 lines with progressive disclosure; self-learning via learnings.md closes the improvement loop (source: transcript-creating-your-own-agentic-os-is-easy.md)
- Skill systems (chained pipelines with human-in-the-loop) are where real time savings come from — not isolated skill use (source: transcript-creating-your-own-agentic-os-is-easy.md)
- The architecture is tool-agnostic and portable; the structure outlives any particular model or tool (source: transcript-creating-your-own-agentic-os-is-easy.md)

## Related

- [[claude-code-agentic-os|Claude Code Agentic OS — Three-Gap Framework]] — complementary three-gap framing (memory/consistency/access) by Chase AI
- [[claude-code-structured-memory|Claude Code Structured Memory — ~/.claude/memory/ Architecture]] — detailed memory implementation guide
- [[nlh-meta-harness-harness-science|NLH, Meta Harness, and the Science of Harness Engineering]] — harness science and meta-optimisation
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — CLAUDE.md, MCP, skills, hooks practical guide

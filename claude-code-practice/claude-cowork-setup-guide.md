---
type: synthesis
title: Claude Cowork Setup Guide — 7 Steps for Non-Technical Users
description: A 7-step practical guide to setting up Claude Cowork (the Claude desktop app's agentic workspace) for non-technical users.
bucket: ai-engineering
topic: claude-code-practice
tags: []
source: ../../../Resources/transcriptions/you-tube/transcript-set-up-claude-cowork-better-than-99-of-people.md
resource:
timestamp: 2026-05-17T08:21:13Z
status: active
related:
  - ai-engineering/claude-code-practice/claude-cowork-full-guide.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
  - ai-engineering/claude-code-practice/claude-managed-agents-memory.md
  - ai-engineering/harness-engineering/nlh-meta-harness-harness-science.md
---

# Claude Cowork Setup Guide — 7 Steps for Non-Technical Users

**Source:** [transcript-set-up-claude-cowork-better-than-99-of-people.md](../../../Resources/transcriptions/you-tube/transcript-set-up-claude-cowork-better-than-99-of-people.md)
**Channel:** Systems Made Better
**Video:** https://www.youtube.com/watch?v=pl90LATQlHI

---

## Summary

A 7-step practical guide to setting up Claude Cowork (the Claude desktop app's agentic workspace) for non-technical users. The system centres on three design principles: a CLAUDE.md-based instruction hierarchy that scopes behaviour per folder, a persistent `memory.md` log that prevents context amnesia across sessions, and tool connectors that turn Claude into a cross-platform assistant operating on real files, email, calendar, and knowledge bases. The guide is aimed at Mac users with no coding background and positions the morning's setup time as the threshold between a blank-slate tool and a functioning personal assistant.

## What Claude Cowork Is

Claude Cowork is Claude Code's CLI wrapped in a desktop UI with a shared workspace folder. You designate a folder as the workspace; everything Claude can act on lives inside it, everything outside is off-limits by default. The underlying model is the same Claude Code base model (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

## The 7 Steps

### Step 1: Install the Claude Desktop App

Download from `claude.com/download`, drag to Applications, open. Sign up for a Claude account (free tier works; Pro at ~£15/month recommended; Max for heavier workloads). Model selection: Sonnet for speed on most tasks, Opus for ambitious or complex projects. Enable extended thinking for Opus (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

### Step 2: Write Global Instructions

Global instructions are a CLAUDE.md file read at the start of every conversation. They are the single most important setup step. Two scope levels are available:

- **System-level** (Settings → Cowork → Global Instructions): applies across all folders.
- **Per-folder CLAUDE.md**: overrides or extends system instructions for a specific workflow.

Recommended contents of global instructions:

- Who you are: name, job, technical comfort level ("explain in non-technical language").
- What you need help with: tools, workflows, content types.
- Preferences: spelling style, tone, directness.
- Safety rules: "never delete, send, or publish anything without checking with me first."
- Honesty clause: flag scope drift, over-engineering, or a simpler path ("don't build a spaceship when a bicycle will do").

(source: transcript-set-up-claude-cowork-better-than-99-of-people.md)

### Step 3: Create an About Me Folder

Three files that give Claude persistent personal context — the difference between a generic AI and one that knows how you work:

**`about-me.md`** — who you are, your business, tools, audience, current projects. Everything a smart new team member would need to know on day one (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**`writing-rules.md`** — how you want things written. Sourced from anti-AI writing style research. Contains banned phrases, style directives, and tone guidance to prevent generic AI output (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**`memory.md`** — a persistent log Claude appends to after each session. Records global preferences, project decisions, and context. The update to global instructions should direct Claude to read all three files at session start and to fill out `memory.md` at session end. This prevents context amnesia across conversations (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

The global instructions reference these three files explicitly so Claude reads them automatically.

### Step 4: Connect Your Tools (Connectors)

Available connectors: Gmail, Google Calendar, Google Drive, Notion, Slack, Supabase, Vercel, Make.com automations. Each connector can be scoped with granular permissions (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

Key connectors to add immediately:

- **Gmail**: drafts only by default — Claude creates draft replies, does not send without permission.
- **Google Calendar**: lets Claude check the week, surface priorities, create events.
- **Claude in Chrome** (browser extension): lets Cowork browse websites, read pages, fill forms, and navigate multi-step workflows. Beta feature; opt-in.

Safety note: Cowork creates drafts, not sends. The global instructions safety rule ("never send without checking") is a second layer. For added privacy, disable "help improve Claude" and "sending sessions to Anthropic AI models" in Settings → Privacy (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**Notion as a special case:** Notion uses an MCP server rather than a simple API connector. This enables query, write, read across the entire workspace. To avoid Claude burning tokens searching Notion blindly, create a `my-context-map.md` in the About Me folder that describes your Notion database structure. The same principle applies to Google Drive or Obsidian: give Claude a map of where information lives (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

Practical test: combine Calendar + Gmail in one query ("check my inbox and calendar and surface anything important for tomorrow") to verify connectors are working.

### Step 5: Use Built-in Skills

Skills are specialised capabilities for creating specific file types — documents, spreadsheets, presentations, PDFs, HTML slides. To enable: Customise → Skills → the `+` sign. Built-in examples include canvas design, visual arts, PDF generation. The skill creator skill helps you build custom ones.

Folder discipline tip: create an `outputs/` folder (for all Claude-generated files, organised by project) and a `projects/` folder (for per-project memory and CLAUDE.md). This turns the workspace into a structured second brain rather than a flat dump of files (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

### Step 6: Install Plugins

Plugins are specialist sub-agents with their own knowledge bases and methods — think of them as hiring a specialist rather than adding a tool. Available via Customise → Personal Plugins → Browse Plugins.

Anthropic-provided examples: Legal (NDA triage, meeting prep), Engineering (code review, architecture). Third-party examples: Apollo (lead search). You can also build custom plugins using the specialist sub-agent builder skill, exporting domain expertise (e.g. a YouTube strategy knowledge base) as an installable plugin (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

Distinction: skills handle standard singular jobs from the existing toolbox; plugins are self-contained specialists that bring their own tools and methods.

### Step 7: Set Up Scheduled Tasks

Scheduled tasks run automatically on a timer using all configured instructions, connectors, and context. They require the computer to be on with internet access — the guide argues for a dedicated always-on desktop for this reason (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

Examples from the guide:

- **Weekly briefer** (Tuesday 10:00): search tasks and projects in Notion, Gmail, and Calendar; email an overview of the week ahead.
- **Weekday inbox triage** (9:30 daily): triage inbox, surface priorities, create draft replies.

Related features: **Dispatch** (control Claude desktop from phone via the Claude app); **Computer Use** (Claude takes actions in Chrome and apps, with app-level exclusion lists) (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

## Key Design Principles

**Global instructions as a new-assistant manual.** Think of CLAUDE.md not as a prompt but as the onboarding document you'd hand a smart new team member. It sets scope, voice, safety rails, and escalation rules permanently (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**memory.md as persistent context.** Without it, every session starts from zero. With it, Claude knows where you left off on projects, what decisions were made, and what you prefer — across any number of sessions (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**writing-rules.md as anti-AI style guard.** Generic AI output is identifiable by its filler phrases and cadence. A writing-rules file built from anti-AI style research gives Claude explicit signals for what not to do (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**Context maps over token burning.** For rich connected tools like Notion, a one-time context map (describing database structure, page locations, key resources) is far more efficient than letting Claude explore the workspace from scratch every session (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

**Per-folder CLAUDE.md for workflow scoping.** Different folders can have different instructions — a writing projects folder can have different rules than a coding folder, both drawing on the global baseline (source: transcript-set-up-claude-cowork-better-than-99-of-people.md).

## Practical Tips

- Use **Whisper Flow** (or similar voice transcription) to reduce the friction of writing prompts — especially useful for longer instructions and natural language tasks.
- Ask Claude to help you write your own global instructions ("suggest a solid system instruction for me").
- When connecting Notion or a complex tool, ask Claude to generate the context map by pointing it at a master page with all databases linked.
- Start with Sonnet for setup and daily tasks; switch to Opus for complex projects or documents that need deeper reasoning.

## Key Takeaways

- The 7-step setup is a morning's work with no code required; the payoff is a persistent, context-aware, tool-connected assistant.
- CLAUDE.md + about-me.md + memory.md + writing-rules.md form the core "operating context" layer that separates a configured Cowork setup from a blank-slate one.
- Connectors (especially Gmail, Calendar, Notion) unlock multi-tool queries and automation that would otherwise require manual copy-paste.
- Scheduled tasks complete the transition from interactive assistant to autonomous background worker.

## Related

- [[claude-cowork-full-guide]] — Ben AI's full practical guide to Cowork (different creator, deeper coverage of skills, plugins, sub-agents, and team use); this article is a complementary entry-level companion
- [[skill-issue-harness-engineering]] — CLAUDE.md, MCP servers, and hooks from a more technical harness-engineering perspective
- [[claude-managed-agents-memory]] — Anthropic's built-in cross-session memory for managed agents (compare to the manual memory.md approach here)
- [[nlh-meta-harness-harness-science]] — natural language harnesses and the science behind instruction design

# Claude Code Desktop Redesign for Parallel Agents

**Source:** [Anthropic — Redesigning Claude Code on desktop for parallel agents](https://claude.com/blog/claude-code-desktop-redesign) (April 2026)

---

## Summary

The Claude Code desktop app has been redesigned for parallel agentic work — multiple sessions running across different repos simultaneously, with the human acting as orchestrator rather than driver. The redesign focuses on session management, inline tooling, and reduced context-switching.

---

## Core Design Premise

Agentic coding is no longer sequential (one prompt, wait, respond). Modern workflows involve:
- A refactor in one repo
- A bug fix in another
- A test-writing pass in a third
- Checking results, steering when something drifts, reviewing diffs before shipping

The app is built for "many things in flight, and you in the orchestrator seat."

---

## New Features

### Session Management (Sidebar)
- All active and recent sessions in one sidebar
- Filter by status, project, or environment
- Group by project for faster navigation
- Sessions auto-archive when their PR merges or closes
- **Side chat** (⌘ +; / Ctrl +;): branch off a question from the main thread without polluting the task context

### Integrated Tooling
- **Integrated terminal** — run tests or builds alongside the session
- **In-app file editor** — open files, make spot edits, save without leaving the app
- **Faster diff viewer** — rebuilt for large changesets
- **Expanded preview** — HTML files, PDFs, and local app servers in-app

All panes are drag-and-drop: arrange terminal, preview, diff viewer, and chat in whatever grid fits the workflow.

### Parity with CLI
- Desktop app now has parity with CLI plugins
- Org-managed plugins and locally installed plugins work identically in desktop and terminal

### SSH Support
- SSH to remote machines now supported on Mac (previously Linux-only)

### View Modes
Three modes dial transparency up or down:
- **Verbose** — full transparency into tool calls
- **Normal** — standard view
- **Summary** — results only

---

## Navigation

- `⌘ + /` (or `Ctrl + /`) — show full keyboard shortcut list
- New shortcuts for session switching, spawning, and navigation
- Usage button shows both context window and session usage at a glance

---

## Key Takeaways

- The UX shift is from "single session driver" to **orchestrator of parallel sessions** — the key mental model change
- Side chats are explicitly context-isolated from the main thread — they pull context in but don't push it back, preventing task contamination
- Integrated terminal + editor + diff viewer + preview means less context-switching to external tools
- Auto-archiving keeps the sidebar signal-to-noise ratio high

## See Also

- [[claude-code-routines]] — cloud-based autonomous sessions that complement local parallel desktop sessions
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — harness configuration that governs what sessions can do
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — sub-agent patterns that parallel sessions can leverage

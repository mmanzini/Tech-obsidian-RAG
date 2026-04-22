# Everyone Should Be Using Claude Code More

**Source:** Lenny Rachitsky, [Lenny's Newsletter](https://www.lennysnewsletter.com/p/everyone-should-be-using-claude-code) (2025-10-14) — 50 ways non-technical people use Claude Code, crowdsourced from 500+ responses.

## The reframe

**Forget the word "Code". Think of it as Claude *Local* or Claude *Agent*.** It's a super-capable agent that runs on your machine, with local file/interface access and long-running sessions. Handles larger files, runs longer than chat, and is versatile beyond code.

Note: Claude Code *executes* locally; the model still runs in the cloud. The "local" feeling comes from direct filesystem + CLI access.

## Install (one-liner)

- **macOS:** `curl -fsSL https://claude.ai/install.sh | bash` then `claude`
- **Windows:** `irm https://claude.ai/install.ps1 | iex`
- Using [Warp](https://www.warp.dev/) as the terminal smooths install hiccups.

## Non-technical use-case taxonomy

Grouped from the 50 community examples:

### File & computer admin
- Clearing storage / diagnosing slow Mac (load, memory pressure, swap, Docker, Time Machine)
- Renaming + sorting invoices into `YYYY-MM-DD Vendor - Invoice - X.pdf` folders
- Deduping and organising the home directory
- Uninstalling stubborn Adobe products
- Formatting NTFS SD cards; resizing oversized GIFs
- Downloading YouTube videos or Google Doc images en masse

### Knowledge work & writing
- Writing content in VS Code + Claude Code: outline, hook, citations, section review (Teresa Torres' process)
- Voice notes → research themes → article in the user's own voice → LinkedIn version (Helen Lee Kupp)
- Reverse-building PRDs and decision logs from messy Slack/chat history
- Weekly self-review: journal + git commits → COO-style gap analysis

### Sales, GTM, research
- Brainstorming domain names + checking availability across TLDs
- Lead generation: "look at what I'm building, find top 5 pilot candidates in my area"
- Scraping GitHub repos for signals of the right prospect (e.g. sensitive values indicating code-assist usage)
- Competitor ad screen-grabbing via browser automation
- arXiv / paper synthesis — find needle-in-haystack cross-connections across thousands of papers

### Product + engineering ops (non-coders)
- Refining Linear issues, release notes, UX polish as CPO/PM custom commands
- Intercom ticket summaries → Asana bug reports with repro steps
- Jira automation from call transcripts (via MCP)
- Self-driving documentation (Claude + Playwright explores the app, finds gaps, writes docs)
- Changelogs: scan commits, apply house style — hours → 10–15 min
- Roadmapping with codebase context: a `{project}-product` repo with folders for `bugs`, `execution`, `press-release`, `product-flywheel`, `user-research`, `vision`; a `CLAUDE.md` points to engineering repos for architectural context

### Media & creative
- Slide decks as HTML (prompt → adjust → template in `.md` → next/prev buttons for presenting)
- Audio: sample-rate conversion, renaming, PT↔EN translation
- Transcribe + chunk doctor's-appointment audio into a personal context file

### Personal & family
- Noticing subtle conflict avoidance across downloaded meeting recordings (Dan Shipper)
- Dad-built slide-tower design with a DIY subagent
- Scheduling an art-model rotation from Gmail replies
- Resizing daughter's artwork photos into a PDF for a school application
- "Claude CEO" daily digest across Gmail, Brex, Mercury, Linear

## The patterns underneath

- **Run from the right directory.** Home dir for personal org; project root for product work; a dedicated `{project}-product` repo for PM work tied to a codebase.
- **Give it tools.** MCP + browser automation (Playwright) radically expand the action space — scraping, Jira, Intercom, Gmail, routers.
- **CLAUDE.md is the back of the filing cabinet.** Link to related repos, house style, decision logs. Context makes the difference between "fine" and "feels like a teammate".
- **Custom slash commands / subagents** for anything repetitive: Reddit monitor, self-review, changelog, PRD generator.
- **Voice → text → agent** is an emerging workflow for parents / mobile-first users. The agent replaces the "sit and write" step.

## Key Takeaways

- Rename the mental model: **Claude *Agent*** on your laptop, not Claude *Code*.
- The install is two commands; Warp smooths out issues.
- Non-technical value is *massive* — file admin, lead gen, presentations, writing, ops. The community examples repeat themselves across 50 different jobs.
- The highest-leverage setups all share: right directory + MCP tools + CLAUDE.md context + reusable slash commands.
- If you're a PM, wiring Claude Code into Linear/Jira/Intercom through MCP turns it into your product-ops hub.

## Related

- [[harness-engineering/opus-4-7-best-practices|Opus 4.7 best practices]]
- [[harness-engineering/subagents-in-claude-code|Subagents in Claude Code]]
- [[harness-engineering/claude-code-routines|Automate work with routines]]
- [[harness-engineering/skill-issue-harness-engineering|skill-issue harness engineering]]
- [[ai-organization/claude-code-productivity-takeaways|Claude Code productivity takeaways]]

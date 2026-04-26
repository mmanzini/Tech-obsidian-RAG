# Claude Cowork — Full Practical Guide

**Source:** YouTube — Ben AI, "Claude Cowork Full Course (2+ Hours)"
**Video:** https://www.youtube.com/watch?v=2HyBA-wkWsA
**Date captured:** 2026-04-25

---

## What Cowork Is

Claude Cowork is Claude's desktop-based agent interface for non-technical knowledge workers — conceptually the same underlying system as Claude Code but with a more business-friendly UI, permission settings, and collaboration features. Available on Pro/Team/Enterprise Claude Desktop (not browser, not free tier).

Key difference from Claude Chat: Cowork is an AI agent — it can read, create, edit, and update files directly on your computer and take actions across connected tools.

---

## Core Concepts

### File Access and Projects

- **Folder access**: Connect a folder per session; Claude reads *and writes* all file types (presentations, CSVs, Google Docs) directly into that folder — eliminates copy-paste context
- **Projects**: Persistent configuration combining a folder, instructions (system prompt), memory, scheduled tasks, and chat history for a domain (e.g., YouTube, sales)
  - Each project can have specific memory rules (e.g., "always mimic my tone of voice from transcripts")
  - Creates at-a-glance history of all chats in that domain

### Connectors and Tool Access (efficiency hierarchy)

1. **Built-in connectors** (native integrations: Notion, Gmail, Slack, Fireflies, CRM) — fastest, most reliable; set "always allow" to avoid permission prompts
2. **MCP servers** — for tools without native integration; find via Google (`[tool name] MCP server`); add via remote URL or editing the JSON config file; Claude's built-in MCP builder skill can generate MCP servers from scratch for tools with APIs
3. **Browser use** — fallback only; token-heavy, error-prone, expensive
4. **Computer use** — last resort; vision-based, very slow and costly; requires enabling in Settings → General

**Apify** is the most important connector for marketing/sales use cases — thousands of scrapers for hard-to-scrape sites (LinkedIn, Instagram, Facebook, Apollo). Connect via `mcp.apify.com`, select scrapers, copy the token string after `=` as the "enable tools" value.

### Dispatch

Remote control of Claude Cowork from phone — install Claude app, connect same account, allow dispatch; then trigger agent sessions and automations while away from the computer.

---

## Skills — The Most Important Feature

### What Skills Are

A skill = a folder containing:
- `skill.md` — the SOP / process instruction (keep this clean and focused on the step-by-step process)
- Reference files — text context (ICP, voice personality, examples, MCP instructions), asset files (images, presentations), code scripts (Python/JS for API calls)

**Progressive disclosure**: only the name + description are stored in agent memory; the skill.md loads when triggered; reference files load only when the skill instructs it. This allows one agent to hold thousands of skills.

### When to Build a Skill

Skills sit between one-off prompts and hard-coded automation: they handle context-dependent, judgment-requiring, human-in-the-loop work. Good fit when: the task is repetitive, requires domain-specific context, or benefits from self-improvement over time.

### Skill Building Framework

1. **Name + trigger** — when should the agent invoke this skill?
2. **Goal / objective** — one-line description of what good looks like
3. **Connectors / MCPs** — which tools does this skill need?
4. **Step-by-step process** — the most important section; define:
   - What each step does
   - When to insert human-in-the-loop (QA boxes: checkboxes, open field, single select)
   - Which reference files to load at each step
   - What the expected output is at each step
5. **Rules section** — predict failure modes and add guardrails; always include:
   - Explicit instruction to read reference files (agents skip them otherwise)
   - Instruction to offer multiple variations at human-in-the-loop points
   - Progressive update rule: "whenever a user says not to do X, auto-update the rules section"

**Key insight**: the process before prompting (mapping out the ideal workflow, preparing context files, collecting output examples) has more impact than prompt quality.

### Essential Context Files for Copywriting Skills

Reuse these across all writing-related skills:
- What we do (business description)
- ICP (ideal customer profile)
- Voice personality (tone attributes, key misconceptions, signature phrases)
- Personal background (stories, career history for personal references)
- Newsletter/LinkedIn/YouTube strategy
- Writing framework
- Example outputs (most important for tone matching)

### Skill Evaluation (Skills 2.0)

Anthropic's updated skill creator skill includes Eval Viewer agents, benchmarking scripts, and report generation.

**Effective eval usage**:
- Always specify what to optimise for (one criterion at a time)
- Define explicit scoring criteria (e.g., word count range, style match, M-dashes, personal story inclusion)
- Define the test setup (number of variations, which input to use)
- Review outputs + copy feedback into chat to guide optimisation

**A/B testing**: compare two skill versions (e.g., original vs. leaner for speed; with vs. without a reference file); use only when you already have a working skill and want to push it further; Anthropic recommends also running after new model releases to check if skill scaffold is still needed.

---

## Agents and Sub-Agents

### Why Sub-Agents

- Before: bulk processing overloaded single-agent context window, was slow
- Now: Cowork can spin up isolated parallel sub-agents for bulk tasks; each can use tools and skills
- Main agent receives summaries only → context stays clean

### Sub-Agent Patterns

- **Bulk processing**: lead qualification (150 leads, 15 parallel sub-agents, 2 min), lead enrichment, personalised outreach writing
- **Multi-step workflows**: chain multiple bulk skills into one automated pipeline
- Always explicitly instruct Cowork to "spin up N parallel sub-agents" — without explicit instruction it may run sequential or skip sub-agents

**Practical limits**:
- Best batch size per sub-agent: 5–15 tasks
- Practical ceiling for bulk runs: ~100–200 tasks (above this: slow, expensive, error-prone)
- For large non-human-in-the-loop bulk runs: use n8n / Make.com instead
- Max plan recommended if using sub-agents regularly

### Sub-Agents vs. Agent Teams (Claude Code)

| | Cowork sub-agents | Claude Code agent teams |
|---|---|---|
| Communication | Isolated (no inter-agent comms) | Can communicate with each other |
| Token efficiency | Lower | Higher |
| Best for | Isolated parallel tasks (research, qualification, writing) | Shared-goal engineering tasks (e.g., building a compiler) |

---

## Plugins

Plugins = bundled sets of skills + connectors + commands + agent definitions.

- Can bundle multiple skills into one shareable unit per department (sales plugin, marketing plugin)
- **Commands**: chain multiple skills in sequence with one slash command trigger
- Versionable; shareable via zip file or GitHub
- Anthropic's built-in plugins (Sales, Marketing, Customer Support, etc.) are open-source baselines to customise

**SaaS implications**: plugins are starting to resemble SaaS software in structure; several SaaS companies (Salesforce, ServiceNow, Adobe) saw stock drops at plugin launch; hypothesis is that plugins become the interface layer and SaaS companies that survive will build their own plugins.

---

## Scheduled Tasks

Schedule a prompt or skill to run automatically at a defined interval (daily, weekly, every 10 min).

Three ways to schedule:
1. Tell Claude directly in chat: "schedule this skill every day at 8am"
2. Slash command: `/schedule`
3. Specify time when building the skill

**Limitation**: scheduled tasks only run while Claude Desktop is open.

### Useful Scheduled Skill Patterns

- Daily email categorisation / priority summary
- Post-meeting to-do extraction from Fireflies transcripts → Notion
- Monthly accounting (Gmail + Stripe + spreadsheet)
- Daily content ideation (scan sources, qualify by ICP/strategy, generate HTML report)
- Failed payment follow-up drafts (check Stripe → draft Gmail → save as draft)
- Daily task review and prioritisation

**Database pattern for recurring skills**: maintain a CSV tracking processed items (source URL, date, qualified/unqualified) so recurring runs skip already-processed content.

---

## AI Operating System / Second Brain

Scaling context across a team:
- Shared folder with hundreds of context documents (brand guidelines, ICP, strategy docs, process notes)
- Connect the folder across all team member Cowork accounts
- All agents share persistent context without copy-paste
- Plugins + commands distribute skills and workflows company-wide

---

## Key Takeaways

- **Skills are the leverage point**: anyone can make a skill in 3 seconds, but good skills take iteration — the gap between a quick skill and a well-built one is the gap between mediocre and excellent output
- **Context files are the multiplier**: ICP, voice personality, personal background, output examples — building these once unlocks all writing skills; effort pays back immediately
- **Progressive disclosure is what makes scale possible**: name/description in memory, skill.md on trigger, reference files on demand
- **Self-learning skills**: the progressive update rule (auto-update rules section when user gives negative feedback) makes skills compound over time
- **Efficiency hierarchy for connectors**: native connector > MCP > browser > computer use; browser use is a last resort
- **Sub-agents = parallelism**: 100–200 tasks that would take hours sequentially become minutes; explicit instructions required
- **Skills → commands → scheduled tasks**: the natural maturity arc from manual to partially automated to fully autonomous workflows
- **Cowork vs. Claude Code**: same underlying system; Cowork adds collaboration, permissions, and non-developer UX; choose Cowork for business workflows, Claude Code for engineering tasks

## Related

- [[harness-engineering/_index|Harness Engineering]] — broader harness design
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — Claude Code skill building patterns
- [[skill-creator-evals|Improving Skill-Creator — Test, Measure, Refine Skills]] — eval methodology deep-dive
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — sub-agent decision framework
- [[claude-code-routines|Claude Code Routines]] — scheduled / API / GitHub-triggered task automation
- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — cross-session memory for agents

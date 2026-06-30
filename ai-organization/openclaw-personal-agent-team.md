---
type: synthesis
title: "OpenClaw: A Personal Team of AI Agents"
description: Claire Vo's OpenClaw setup runs nine specialised AI agents (PA, family, marketing, sales, helpdesk, course ops, podcast, developer, kids' tutor) on owned hardware, each with its own Markdown-based identity files and workspace, reachable via Telegram, WhatsApp, or Slack.
bundle: ai-engineering
topic: ai-organization
tags: [multi-agent, agent-architecture, ai-org-design, agent-memory]
source: https://www.lennysnewsletter.com/
resource: https://www.lennysnewsletter.com/
timestamp: 2026-05-09T07:23:23Z
status: active
related:
  - ai-engineering/ai-organization/from-hierarchy-to-intelligence.md
  - ai-engineering/agent-architecture/lethal-trifecta-and-agentic-patterns.md
  - ai-engineering/agent-architecture/twelve-factor-agents.md
---

# OpenClaw: A Personal Team of AI Agents

**Source:** Claire Vo, [Lenny's Newsletter](https://www.lennysnewsletter.com/) (March 2026)

Claire Vo's guide (via Lenny's Newsletter, March 2026) to building a multi-agent personal AI assistant setup using OpenClaw — an open-source framework that runs locally and exposes agents through chat channels (Telegram, WhatsApp, Slack).

## Summary

Claire Vo's OpenClaw setup runs nine specialised AI agents (PA, family, marketing, sales, helpdesk, course ops, podcast, developer, kids' tutor) on owned hardware, each with its own Markdown-based identity files and workspace, reachable via Telegram, WhatsApp, or Slack. The core finding is that narrow, specialised agents outperform a single omni-agent on both quality and trust — but the setup requires serious operational security awareness, since any tool access the agent has becomes an attack surface through prompt injection and untrusted input channels.

## Core Concepts

- **Local gateway** receives messages from any **channel** (terminal, Telegram, WhatsApp).
- **Agents** have their own identities, tools, and workspaces.
- Agents run on **cron jobs** plus a 30-minute **heartbeat**.
- **Skills, APIs, CLIs** are self-installable.
- Deployed on owned hardware (Mac Mini) or VPS — never on a daily-driver machine.

## Agent Identity Files

Each agent's workspace contains Markdown files that act as its operating system:

- `AGENTS.md` — core instructions and memory
- `SOUL.md` — persona, tone, boundaries
- `IDENTITY.md` — name, vibe, emoji
- `TOOLS.md` — tool usage notes
- `USER.md` — facts about the human

## The Multi-Agent Unlock

Vo's biggest insight: don't try to make one agent do everything. She runs **9 specialised agents** — Polly (PA), Finn (family), Max (marketer), Sam (sales), Holly (helpdesk), Sage (course ops), Howie (podcast producer), Kelly (developer), Q (kids' tutor). Narrower identity → better quality, more fun. Agents can spawn and surgically transfer memory/crons to each other.

## Security Cautions

Mirrors the [[lethal-trifecta-and-agentic-patterns|lethal trifecta]] problem:

- Anyone who can chat with the agent can instruct it — keep it private, not in group channels.
- Web/email access exposes it to prompt injection; reinforce rules in `SOUL.md`.
- Default to **read-only** API tokens until trust builds.
- Only install skills from official bundles or known developers.
- Calendar = location, email = financials, agents may know kids' schedules — think about worst-case operational security.

## Cost

Vo spends ~$1,000/month on direct API costs running max-capability models. Cheaper subscription tiers work for most users.

## Key Takeaways

- A multi-agent personal setup scales better than one omni-agent — narrower scope improves both quality and trust.
- The agent's workspace is just Markdown files — debuggable, editable, regeneratable from Claude Code if broken.
- This is one of the first agentic products that "feels like hiring a team" — concrete preview of [[from-hierarchy-to-intelligence|AI as coordination layer]].
- Security risk scales with tool access; the trifecta of private data + untrusted input + external send is unsolved.

## Related

- [[from-hierarchy-to-intelligence|From Hierarchy to Intelligence]] — broader thesis on AI replacing coordination
- [[lethal-trifecta-and-agentic-patterns|Lethal Trifecta and Agentic Engineering Patterns]] — security model behind the cautions
- [[twelve-factor-agents|Twelve-Factor Agents]] — production reliability principles these agents loosely follow

---
type: synthesis
title: Scaling Managed Agents — Decoupling Brain from Hands
description: "Anthropic's Managed Agents service solves a long-standing infrastructure problem: how to build a system for \"programs as yet unthought of.\" By virtualizing the three components of an agent — harness (brain), sandbox (hands), and session (log) — into independent interfaces, each can fail or be replaced without disturbing the others."
bucket: ai-engineering
topic: harness-engineering
tags: [agent-architecture, harness-engineering, long-running-agents, agent-memory]
source: https://www.anthropic.com/engineering/managed-agents
resource: https://www.anthropic.com/engineering/managed-agents
timestamp: 2026-04-29T21:24:51Z
status: active
related:
  - ai-engineering/harness-engineering/harness-design-long-running-apps.md
  - ai-engineering/harness-engineering/effective-harnesses-long-running.md
  - ai-engineering/claude-code-practice/claude-managed-agents-memory.md
  - ai-engineering/claude-code-practice/subagents-in-claude-code.md
---

# Scaling Managed Agents — Decoupling Brain from Hands

**Source:** [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents)
**Author:** Lance Martin, Gabe Cemaj, Michael Cohen (Anthropic)
**Published:** 2026-04-28

---

## Summary

Anthropic's Managed Agents service solves a long-standing infrastructure problem: how to build a system for "programs as yet unthought of." By virtualizing the three components of an agent — harness (brain), sandbox (hands), and session (log) — into independent interfaces, each can fail or be replaced without disturbing the others. The result is a meta-harness that accommodates future Claude models and harness implementations while delivering a 60% p50 TTFT improvement.

## The "Pet" Problem: Why Coupled Containers Were Limiting

The initial architecture placed all agent components — session, harness, and sandbox — inside a single container. This created what infrastructure engineers call a "pet": a named, hand-tended individual you can't afford to lose (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md).

The coupled design produced three concrete problems:

1. **Fragility:** If a container failed, the session was lost. If unresponsive, engineers had to nurse it back to health.
2. **Opaque debugging:** The only window in was the WebSocket event stream, which couldn't distinguish between a harness bug, a packet drop, or a container going offline. Shelling into the container was often impossible because user data lived there too.
3. **Connectivity assumption:** The harness assumed that whatever Claude worked on lived in the same container. Customers wanting to connect Claude to their VPC had to either peer their network with Anthropic's, or run the harness in their own environment — an assumption baked into the harness became a barrier to different infrastructure configurations (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md).

![[903404a4d0a295fd13056de991830b5e_MD5.png]]

## Decoupling Brain, Hands, and Session

The solution was to treat each component as an independent interface (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md):

**Brain (harness):** The harness left the container entirely. It calls the sandbox the same way it calls any other tool: `execute(name, input) → string`. The container became cattle. If it dies, the harness catches the failure as a tool-call error and passes it to Claude. A new container can be reinitialized with `provision({resources})`.

**Session (log):** Because the session log lives outside the harness, nothing in the harness needs to survive a crash. A new harness can be rebooted with `wake(sessionId)`, then use `getSession(id)` to retrieve the event log and resume from the last event. During the agent loop, the harness writes to the session with `emitEvent(id, event)` to maintain a durable record.

**Hands (sandboxes and tools):** Each execution environment is now just a tool — `execute(name, input) → string`. That interface supports any custom tool, any MCP server, and Anthropic's own tools. The harness is agnostic to whether the sandbox is a container, a phone, or any other execution environment.

![[051c683bcf24f6fe25b1f37f97f7e2b3_MD5.webp]]

## The Universal Tool Interface

The design collapses all hands into a single interface: `execute(name, input) → string`. A name and input go in; a string comes back (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md). This interface:

- Supports any custom tool, any MCP server, and Anthropic's own sandboxes.
- Allows brains to pass hands to one another — no coupling between any specific brain and any specific hand.
- Makes the harness agnostic to the nature of the execution environment behind the interface.

## Security: Credentials Stored Outside the Sandbox

In the coupled design, any untrusted code Claude generated ran in the same container as credentials. A successful prompt injection only had to convince Claude to read its own environment to obtain tokens capable of spawning unrestricted sessions (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md).

The decoupled architecture uses two patterns to keep credentials out of the sandbox:

- **Git token injection:** Each repository's access token is used to clone the repo during sandbox initialization and wired into the local git remote. `push` and `pull` work from inside the sandbox without the agent ever handling the token.
- **OAuth vault + MCP proxy:** For custom tools, OAuth tokens are stored in a secure vault outside the sandbox. Claude calls MCP tools via a dedicated proxy that fetches credentials from the vault at call time. The harness is never made aware of any credentials.

## `getEvents()` — Session Log as Context Object

Long-horizon tasks often exceed Claude's context window. Standard approaches (compaction, summarization, context trimming) all involve irreversible decisions about what to keep (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md).

The Managed Agents session addresses this differently: it serves as a durable context object that lives *outside* Claude's context window. The `getEvents()` interface allows the harness to interrogate context by selecting positional slices of the event stream — picking up from the last stop, rewinding before a specific moment, or rereading context before a specific action.

Fetched events can also be transformed in the harness before being passed to Claude's context window, enabling context organization for high prompt cache hit rates and arbitrary context engineering strategies — without encoding those strategies into the session layer itself.

![[ffb026fab2bc3630c6d39d5a23602d99_MD5.webp]]

## Many Brains, Many Hands: 60% p50 TTFT Improvement

**Many brains.** In the original architecture, each brain required its own container — every session paid full container setup cost up front, even sessions that would never touch a sandbox (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md). After decoupling:

- Containers are provisioned on demand via `execute(name, input) → string` only when actually needed.
- Inference starts as soon as the orchestration layer pulls pending events from the session log.
- Scaling to many brains means starting many stateless harnesses.

Result: **p50 TTFT dropped roughly 60%; p95 dropped over 90%** (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md).

**Many hands.** Each hand is now just a tool. Because no hand is coupled to any brain, brains can pass hands to one another. When a container fails, only that hand's state is affected — not every hand the brain was reaching into.

![[c17bf1251f8dd6d827bf343c5fd4579c_MD5.webp]]

## The Operating System Analogy

Managed Agents follows the same pattern as operating systems: virtualize the hardware into abstractions general enough for programs that don't exist yet. The `read()` system call is agnostic to whether it accesses a disk pack from the 1970s or a modern SSD — the abstraction outlasted the hardware (source: 2026-04-28-Scaling Managed Agents Decoupling the brain from the hands.md).

Managed Agents virtualizes the components of an agent: session, harness, and sandbox. The interfaces are designed to outlast any particular implementation. Managed Agents is a *meta-harness* — opinionated about the interfaces (Claude will need to manipulate state and perform computation; it will need many brains and many hands), but not about the specific number, location, or implementation of those brains and hands.

## Key Takeaways

- The "pet" problem with coupled containers caused fragility, opaque debugging, and hard-coded connectivity assumptions.
- Decoupling brain (harness), hands (sandboxes/tools), and session (log) lets each fail or be replaced independently.
- `execute(name, input) → string` is the universal tool interface — agnostic to what runs behind it.
- Git credentials are injected at clone time; OAuth tokens live in a vault accessed via proxy — the sandbox never touches credentials.
- `getEvents()` provides a durable, queryable context object outside Claude's context window, decoupling recoverable storage from context management strategy.
- On-demand container provisioning cut p50 TTFT by ~60% and p95 by over 90%.
- The OS analogy: stable interfaces outlast the implementations beneath them.

## Related

- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — GAN-inspired architecture this work evolved from
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — earlier two-agent pattern that informed the session-log design
- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — cross-session memory built on top of this managed infrastructure
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — many-brains pattern from the user perspective

---
type: synthesis
title: Claude Code Session Management and 1M Context
description: How you manage sessions, context, and compaction in Claude Code shapes results more than most users expect.
bundle: ai-engineering
topic: claude-code-practice
tags: [claude-code, context-engineering, multi-agent, long-running-agents, agent-workflows]
source: https://claude.com/blog/using-claude-code-session-management-and-1m-context
resource: https://claude.com/blog/using-claude-code-session-management-and-1m-context
timestamp: 2026-05-17T08:21:13Z
status: active
related:
  - ai-engineering/claude-code-practice/subagents-in-claude-code.md
  - ai-engineering/harness-engineering/scaling-managed-agents.md
  - ai-engineering/claude-code-practice/claude-code-quality-postmortem.md
  - ai-engineering/harness-engineering/effective-harnesses-long-running.md
---

# Claude Code Session Management and 1M Context

**Source:** [Using Claude Code: session management and 1M context](https://claude.com/blog/using-claude-code-session-management-and-1m-context)
**Author:** Thariq Shihipar (Anthropic)
**Published:** 2026-04-15

---

## Summary

How you manage sessions, context, and compaction in Claude Code shapes results more than most users expect. This practical guide covers context rot, the five turn-level options available after every agent response, when to start a new session versus continuing, and the decision table for choosing among continue, rewind, compact, clear, and subagent.

![[216c66a70848e375dcf1c90bfaa9d6d7_MD5.svg]]

## Context Rot and Compaction

The context window is everything the model can "see" at once: system prompt, conversation history, every tool call and output, and every file read. Claude Code's context window is one million tokens (source: 2026-04-28-Using Claude Code session management and 1M context.md).

**Context rot** is the observation that model performance degrades as context grows, because attention gets spread across more tokens and older, irrelevant content begins to distract from the current task (source: 2026-04-28-Using Claude Code session management and 1M context.md).

![[a89faa6c849f09a1986e9b879b4a3055_MD5.png]]

Context windows are a hard cutoff. When nearing the limit, the task is automatically summarized into a compact description and the model continues in a new context window — this is **autocompact**. Users can also trigger compaction manually (source: 2026-04-28-Using Claude Code session management and 1M context.md).

![[b6dc10488e4c666a8e6dee1ff1b8df88_MD5.png]]

## Five Options at Every Turn

After Claude finishes a response, there is a branching point with five options (source: 2026-04-28-Using Claude Code session management and 1M context.md):

1. **Continue** — send another message in the same session; the natural default.
2. **`/rewind` (esc esc)** — jump back to a previous message; all messages after that point are dropped from the context.
3. **`/clear`** — start a new session, typically with a brief written from what was just learned.
4. **`/compact [hint]`** — ask the model to summarize the conversation so far, replace history with that summary, and continue on top of it.
5. **Subagents** — delegate the next chunk of work to an agent with its own clean context window; only the result comes back to the parent.

![[38dbde41133ce220c105cae66ed98951_MD5.png]]

## When to Start a New Session

The general rule: start a new task, start a new session. The 1M context window means longer tasks are more reliable, but context rot may still occur over very long sessions (source: 2026-04-28-Using Claude Code session management and 1M context.md).

Exception: for related tasks where some context is still load-bearing (e.g., writing documentation for a feature just implemented), continuing the existing session avoids the cost of re-reading files (source: 2026-04-28-Using Claude Code session management and 1M context.md).

## Rewind for Correction

**Rewind is often better than correcting forward.** Example: Claude reads five files, tries an approach, and it fails. The instinct is to type "that didn't work, try X instead." The better move is often to rewind to just after the file reads and re-prompt with what was learned: "Don't use approach A, the foo module doesn't expose that — go straight to B." (source: 2026-04-28-Using Claude Code session management and 1M context.md)

![[205e99e3eff3f54ccf63bc91d2ea6b2e_MD5.png]]

This keeps the useful file reads in context while dropping the failed attempt. `/rewind` can also be used to have Claude generate a handoff summary — a message from its future self to its past self explaining what it tried and what it learned (source: 2026-04-28-Using Claude Code session management and 1M context.md).

## Compact vs. Clear

Once a session gets long, two ways to shed extraneous context are available (source: 2026-04-28-Using Claude Code session management and 1M context.md):

- **`/compact [hint]`**: the model summarizes the conversation and replaces history with that summary. It's lossy, but requires no user effort and Claude may be thorough about including important learnings. Steering with a hint (`/compact focus on the auth refactor, drop the test debugging`) improves the result.
- **`/clear`**: the user writes the brief manually ("we're refactoring the auth middleware, the constraint is X, the files that matter are A and B, we've ruled out approach Y") and starts clean. More work, but the resulting context is exactly what the user decided was relevant.

![[f189c082a0efa698fc39700b8bc89746_MD5.png]]

## What Causes Bad Autocompact

Bad autocompacts occur when the model can't predict the future direction of the work. Example: autocompact fires after a long debugging session. The next message is "now fix that other warning we saw in bar.ts." Because the session was focused on debugging, the unrelated warning may have been dropped from the summary (source: 2026-04-28-Using Claude Code session management and 1M context.md).

The compounding factor: due to context rot, the model is at its **least intelligent point** when autocompact fires. With 1M context, there is more runway to trigger `/compact` proactively — before rot degrades judgment — with a description of the intended next direction (source: 2026-04-28-Using Claude Code session management and 1M context.md).

## Subagents and Fresh Context Windows

Subagents work well when it is known in advance that a chunk of work will produce a lot of intermediate output that will not be needed again. The subagent gets its own fresh context window, does as much work as needed, and returns only the final report to the parent (source: 2026-04-28-Using Claude Code session management and 1M context.md).

![[5531b275fbc28a81cf09ee23386bead4_MD5.png]]

**The Anthropic mental test:** "Will I need this tool output again, or just the conclusion?" If only the conclusion is needed, a subagent keeps intermediate noise out of the parent context (source: 2026-04-28-Using Claude Code session management and 1M context.md).

Example explicit subagent directives:
- "Spin up a subagent to verify the result of this work based on the following spec file"
- "Spin off a subagent to read through this other codebase and summarize how it implemented the auth flow, then implement it yourself in the same way"
- "Spin off a subagent to write the docs on this feature based on my git changes"

## Decision Table

| Situation | Consider reaching for | Why |
|---|---|---|
| Same task, context still relevant | Continue | Everything in the window is still load-bearing; don't pay to rebuild it. |
| Claude went down a wrong path | Rewind (double-Esc) | Keep the useful file reads, drop the failed attempt, re-prompt with what you learned. |
| Mid-task but session is bloated with stale debugging/exploration | `/compact <hint>` | Low effort; Claude decides what mattered. Steer it with instructions if needed. |
| Starting a genuinely new task | `/clear` | Zero rot; you control exactly what carries forward. |
| Next step will generate lots of output you'll only need the conclusion from (codebase search, verification, doc writing) | Subagent | Intermediate tool noise stays in the child's context; only the result comes back. |

(source: 2026-04-28-Using Claude Code session management and 1M context.md)

## Key Takeaways

- Context rot is real: attention degrades as context grows; 1M tokens extends but does not eliminate the problem.
- Autocompact is a last resort — the model is least capable exactly when it fires. Prefer proactive `/compact` with a directional hint.
- Rewind often beats forward correction: keep useful reads, drop failed attempts, re-prompt with learnings.
- `/compact` is lossy but low-effort; `/clear` is lossless (user controls what survives) but requires writing the brief.
- Subagents are context firewalls: intermediate tool noise stays in the child; only the conclusion returns to the parent.
- The key subagent question: "Will I need this tool output again, or just the conclusion?"

## Related

- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — detailed coverage of subagent invocation patterns and when to skip them
- [[scaling-managed-agents|Scaling Managed Agents — Decoupling Brain from Hands]] — architectural view of session as durable context object outside the context window
- [[claude-code-quality-postmortem|Claude Code Quality Postmortem — April 2026]] — caching bug that dropped reasoning per turn, directly related to session/context management
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — multi-context-window harness patterns for tasks that exceed a single window

---
type: synthesis
title: Claude Code Quality Postmortem — April 2026
description: Over March–April 2026, three separate changes to Claude Code caused a wave of quality-degradation reports affecting Sonnet 4.6, Opus 4.6, and Opus 4.7.
bundle: ai-engineering
topic: claude-code-practice
tags: [claude-code, harness-engineering, evals, context-engineering, agent-memory]
source: https://www.anthropic.com/engineering/april-23-postmortem
resource: https://www.anthropic.com/engineering/april-23-postmortem
timestamp: 2026-05-17T08:21:13Z
status: active
related:
  - ai-engineering/claude-code-practice/opus-4-7-best-practices.md
  - ai-engineering/claude-code-practice/claude-code-session-management.md
  - ai-engineering/harness-engineering/scaling-managed-agents.md
  - ai-engineering/harness-engineering/skill-creator-evals.md
---

# Claude Code Quality Postmortem — April 2026

**Source:** [An update on recent Claude Code quality reports](https://www.anthropic.com/engineering/april-23-postmortem)
**Author:** Anthropic
**Published:** 2026-04-28

---

## Summary

Over March–April 2026, three separate changes to Claude Code caused a wave of quality-degradation reports affecting Sonnet 4.6, Opus 4.6, and Opus 4.7. Each change hit a different slice of traffic on a different schedule, making the aggregate effect look like broad, inconsistent degradation. All three were resolved by April 20 (v2.1.116); Anthropic reset usage limits for all subscribers on April 23.

## The Three Issues

### Issue 1 — Reasoning Effort Downgrade (Mar 4 → reverted Apr 7)

When Opus 4.6 launched in Claude Code, the default reasoning effort was `high`. Users reported that Opus 4.6 in high effort mode occasionally thought for too long, causing the UI to appear frozen and disproportionate latency and token usage (source: 2026-04-28-An update on recent Claude Code quality reports.md).

Internal evals showed medium effort achieved slightly lower intelligence with significantly less latency for the majority of tasks and avoided tail-latency spikes. Claude Code rolled out medium as the default effort on March 4, with an in-product explanation (source: 2026-04-28-An update on recent Claude Code quality reports.md).

![[01c4983f4caafbd8e878b18efc33e125_MD5.webp]]

After rollout, users immediately reported Claude Code felt less intelligent. Anthropic shipped UI iterations to surface the effort setting (startup notices, an inline effort selector, ultrathink option), but most users retained the medium default. After hearing further customer feedback, the decision was reversed on April 7. All users now default to `xhigh` effort for Opus 4.7 and `high` for all other models (source: 2026-04-28-An update on recent Claude Code quality reports.md).

![[9eb4469bb52dbf6fd5977adeae9cfb1d_MD5.webp]]

**Models affected:** Sonnet 4.6, Opus 4.6.

### Issue 2 — Caching Bug That Dropped Prior Reasoning (Mar 26 → fixed Apr 10)

When Claude reasons through a task, that reasoning is kept in the conversation history so subsequent turns can see why earlier edits and tool calls were made. On March 26, Anthropic shipped a caching optimization: if a session had been idle for more than an hour (a cache miss anyway), clear old thinking sections to reduce the number of uncached tokens sent to the API. The implementation used the `clear_thinking_20251015` API header with `keep:1` (source: 2026-04-28-An update on recent Claude Code quality reports.md).

**The bug:** instead of clearing thinking history once on session resume, it cleared thinking on every subsequent turn for the rest of the session. After crossing the idle threshold once, each request told the API to keep only the most recent block of reasoning and discard everything before it (source: 2026-04-28-An update on recent Claude Code quality reports.md).

This compounded: if a follow-up message arrived while Claude was mid-tool-use, that started a new turn under the broken flag, so even the reasoning from the current turn was dropped. Claude would continue executing but increasingly without memory of why it had chosen its current actions (source: 2026-04-28-An update on recent Claude Code quality reports.md).

![[d532c3b528b5e8b38ee027de34831591_MD5.webp]]

**User-visible symptoms:** forgetfulness, repetition, odd tool choices, and faster-than-expected usage limit drain (the continuous thinking drops caused cache misses on every subsequent request) (source: 2026-04-28-An update on recent Claude Code quality reports.md).

**Why it was hard to reproduce:** Two unrelated experiments masked the issue — an internal-only server-side experiment related to message queuing, and an orthogonal change in how thinking was displayed that suppressed the bug in most CLI sessions. The bug lived at the intersection of Claude Code's context management, the Anthropic API, and extended thinking. It passed multiple human and automated code reviews, unit tests, end-to-end tests, automated verification, and dogfooding. Combined with the corner-case trigger (stale sessions), it took over a week to discover and confirm (source: 2026-04-28-An update on recent Claude Code quality reports.md).

**What Code Review found:** As part of the investigation, Anthropic back-tested Code Review against the offending pull requests using Opus 4.7 with full repository context. **Opus 4.7 found the bug; Opus 4.6 did not.** Anthropic is now adding support for additional repositories as context for code reviews (source: 2026-04-28-An update on recent Claude Code quality reports.md).

Fixed April 10 in v2.1.101. **Models affected:** Sonnet 4.6, Opus 4.6.

### Issue 3 — Verbosity System Prompt Hurt Coding (Apr 16 → reverted Apr 20)

Claude Opus 4.7 tends to be verbose — a trait that helps on hard problems but produces more output tokens. In preparation for its release, Anthropic added the following to the Claude Code system prompt (source: 2026-04-28-An update on recent Claude Code quality reports.md):

> "Length limits: keep text between tool calls to ≤25 words. Keep final responses to ≤100 words unless the task requires more detail."

After multiple weeks of internal testing with no regressions detected, the change shipped alongside Opus 4.7 on April 16. During the broader post-incident investigation, ablations over a wider eval suite revealed a **3% drop in coding quality for both Opus 4.6 and Opus 4.7**. The prompt was reverted as part of the April 20 release (source: 2026-04-28-An update on recent Claude Code quality reports.md).

**Models affected:** Sonnet 4.6, Opus 4.6, Opus 4.7.

## Why the Aggregate Looked Like Broad Degradation

Each change hit a different user segment on a different schedule. The compound effect appeared as broad, inconsistent degradation. Reports began arriving in early March but were initially difficult to distinguish from normal feedback variation. Neither internal usage nor evals initially reproduced the issues identified (source: 2026-04-28-An update on recent Claude Code quality reports.md).

## Going Forward: Process Changes

Anthropic committed to several process changes to prevent recurrence (source: 2026-04-28-An update on recent Claude Code quality reports.md):

- **More internal staff on public build:** ensure a larger share of internal staff use the exact public build rather than internal test versions.
- **Broader evals per system prompt change:** run a broad per-model eval suite for every system prompt change, including ablations to understand the impact of each individual line.
- **New prompt-change tooling:** improved tooling to make prompt changes easier to review and audit; CLAUDE.md guidance to gate model-specific changes to the specific model they target.
- **Soak periods and gradual rollouts:** for any change that trades off against intelligence, add soak periods, broader eval coverage, and gradual rollouts.
- **Improved Code Review with multi-repo context:** ship the Opus 4.7-powered Code Review (which found the bug) with support for additional repository context.

## Key Takeaways

- Reasoning effort is a user-visible quality signal: a downgrade from `high` to `medium` is felt immediately even when internal evals show small intelligence differences.
- Caching bugs that touch extended thinking can compound turn-over-turn, causing forgetfulness and unexpected token drain — symptoms that look like model degradation.
- Corner-case bugs (idle > 1h threshold) plus display-level changes can mask issues through dogfooding and automated testing simultaneously.
- Opus 4.7 found a thinking-management bug that Opus 4.6 missed — model capability directly improves harness-level code review.
- System prompt verbosity constraints can cause measurable coding quality regressions; ablation coverage must be broad enough to catch cross-model effects.
- Gradual rollout + soak period + broad ablation suite is the minimum safe process for any system prompt change.

## Related

- [[opus-4-7-best-practices|Opus 4.7 Best Practices for Claude Code]] — effort levels (xhigh default), adaptive thinking, and harness adjustments for 4.6 → 4.7 upgrade
- [[claude-code-session-management|Claude Code Session Management and 1M Context]] — context management including compaction, which intersects with the caching bug
- [[scaling-managed-agents|Scaling Managed Agents — Decoupling Brain from Hands]] — session log as durable context object (architectural fix for context-drop failure modes)
- [[skill-creator-evals|Improving Skill-Creator — Test, Measure, Refine Skills]] — eval methodology that could have caught issue 3 earlier

---
type: synthesis
title: Cache Diagnostics — Diagnosing Prompt Cache Misses
description: Cache diagnostics is a beta Claude API feature that closes the gap between knowing a cache miss happened and knowing *why*.
bundle: ai-engineering
topic: agent-infrastructure
tags: [harness-engineering, context-engineering, prompt-caching]
source: https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics
resource: https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics
timestamp: 2026-05-25T00:17:36Z
status: active
related:
  - ai-engineering/agent-infrastructure/computer-browser-use-best-practices.md
  - ai-engineering/agent-infrastructure/scaling-managed-agents.md
  - ai-engineering/harness-engineering/skill-issue-harness-engineering.md
---

# Cache Diagnostics — Diagnosing Prompt Cache Misses

**Source:** [Cache diagnostics](https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics)

---

## Summary

Cache diagnostics is a beta Claude API feature that closes the gap between knowing a cache miss happened and knowing *why*. Pass the previous response `id` and the API compares the two requests, reporting the first point of divergence — model, system prompt, tools, or message history — so you can fix the root cause instead of guessing.

## How It Works

Without cache diagnostics, the only signal of a cache miss is `usage.cache_read_input_tokens` dropping to zero, with no indication of what changed. A reordered tool, a timestamp interpolated into the system prompt, or an edit to an earlier message can silently invalidate the cache (source: cache-diagnostics.md).

When the beta header `cache-diagnosis-2026-04-07` is present, the API stores a lightweight fingerprint of each request keyed by the response `id`. On the next request, include the previous `id` as `diagnostics.previous_message_id`. The API rebuilds the fingerprint, compares it, and attaches a `diagnostics` object describing the first point of divergence (source: cache-diagnostics.md).

Fingerprints contain only hashes and token-count estimates — never raw prompt content — and are scoped to your organization and workspace (source: cache-diagnostics.md).

Cache diagnostics is currently Claude API only — not available on Amazon Bedrock or Vertex AI (source: cache-diagnostics.md).

## Basic Usage

Send the beta header on every turn. First turn: `"previous_message_id": null` to opt in. Subsequent turns: pass the `id` from the previous response (source: cache-diagnostics.md).

In streaming responses, `diagnostics` appears on the `message_start` event (source: cache-diagnostics.md).

In a multi-turn loop, carry the latest response `id` forward as `previous_message_id` on every turn (source: cache-diagnostics.md).

## Response Format and Miss Reason Types

The `diagnostics` field has four possible states:

| Value | Meaning |
|---|---|
| field absent | Beta header missing, or `diagnostics` not included in request |
| `null` | First turn (nothing to compare) OR comparison ran and found no divergence |
| `{"cache_miss_reason": null}` | Comparison still running; treat as inconclusive |
| `{"cache_miss_reason": {...}}` | Divergence found — `type` identifies the cause |

Cache miss reason types (source: cache-diagnostics.md):

| Type | Root cause | Fix |
|---|---|---|
| `model_changed` | Different model selected (router, A/B test, fallback) | Hold model constant within a cached conversation |
| `system_changed` | System prompt differs — typically a timestamp or request ID was interpolated | Move dynamic data into the first `user` message after the cache breakpoint |
| `tools_changed` | Tools added/removed/reordered or schemas serialized non-deterministically | Fixed tool order, deterministic schema serialization (e.g. sorted keys) |
| `messages_changed` | History truncated/edited/reordered rather than appended to, or tool results re-serialized differently | Treat history as append-only; echo assistant content and tool results verbatim |
| `previous_message_not_found` | No fingerprint found for the `previous_message_id` (missing beta header on prior request, different workspace, or fingerprint expired) | Send beta header on every turn; keep consecutive turns close in time |
| `unavailable` | Comparison not available — request parameters changed (`tool_choice`, `thinking`, `context_management`, etc.) or divergence is beyond comparison horizon | Keep prompt-affecting parameters constant for the lifetime of a cached conversation |

The four `*_changed` types also carry `cache_missed_input_tokens`: an estimate of how many tokens fell after the divergence point. Treat as a magnitude indicator, not a billing number (source: cache-diagnostics.md).

## Reading Diagnostics Alongside Usage

`diagnostics` answers "did my request change?" while `usage.cache_read_input_tokens` answers "did the cache hit?" (source: cache-diagnostics.md):

| Diagnostics | Cache read tokens | Interpretation |
|---|---|---|
| `null` | high | Working as expected |
| `null` | low/zero | Requests match but cache entry expired — consider 1-hour TTL or shorter turn gaps |
| `*_changed` | low/zero | Your bug — request changed; fix per `type` |
| `*_changed` | high | Change occurred late; an earlier breakpoint still hit — low priority |

## Limitations

- Beta: field names and semantics may change before GA
- Claude API only (not Bedrock or Vertex AI)
- Fingerprints expire after a short period — run comparisons between closely spaced requests
- Same organization and workspace scope required
- Very long conversations may return `unavailable` if divergence is beyond comparison horizon
- Diagnostics are best-effort and never block requests (source: cache-diagnostics.md)

## Key Takeaways

- Enable with the `cache-diagnosis-2026-04-07` beta header on every turn
- Pass the previous response `id` as `diagnostics.previous_message_id` to get a comparison
- The most common silent cache killers: timestamps in system prompts, non-deterministic tool serialization, history truncation
- Use the `*_changed` type to identify the first divergence point, then fix that layer first
- Combine with `usage.cache_read_input_tokens` for full picture

## Related

- [[computer-browser-use-best-practices|Computer and Browser Use Best Practices]] — context management and cache breakpoint strategy for long-running CU sessions
- [[scaling-managed-agents|Scaling Managed Agents]] — context engineering at scale where caching is critical
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — broader harness context where caching is one optimization layer

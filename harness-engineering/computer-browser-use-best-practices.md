---
type: synthesis
title: Computer and Browser Use Best Practices
description: Anthropic's comprehensive guide for developers building computer and browser use integrations with Claude 4.6/4.7.
bundle: ai-engineering
topic: harness-engineering
tags:
- agent-architecture
- harness-engineering
- agent-workflows
- computer-use
resource: https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude
sources:
- id: best-practices-for-computer-and-browser-use-with-claude
  resource: https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-25T00:17:36Z'
status: stable
related:
- ai-engineering/harness-engineering/advisor-strategy.md
- ai-engineering/harness-engineering/effective-harnesses-long-running.md
- ai-engineering/harness-engineering/sandcastle-afk-agent-orchestration.md
- ai-engineering/harness-engineering/cache-diagnostics.md
---

# Computer and Browser Use Best Practices

**Source:** [Best practices for computer and browser use with Claude](https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude)
**Author:** Lucas Gonzalez and Luca Weihs (Anthropic)

---

## Summary

Anthropic's comprehensive guide for developers building computer and browser use integrations with Claude 4.6/4.7. The core thesis is that click accuracy, context management, and prompt injection defense are the three pillars of a reliable computer use harness — and that pre-downscaling screenshots before API submission is the single highest-impact optimization available.

## Resolution and Scaling

The API has internal processing limits; images exceeding them are silently downscaled before the model sees them, causing coordinate mismatches (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

**Resolution limits by model family:**

| Model | Max long edge | Max total pixels |
|---|---|---|
| Claude 4.6 family | 1568 px | 1.15 MP |
| Opus 4.7 | 2576 px | 3.75 MP |

**Recommended starting points (source: 2026-05-25-Best practices for computer and browser use with Claude.md):**
- Start at **1280×720** for Claude 4.6 models — safe, within budget, broadly trained.
- Start at **1080p** for Opus 4.7 — meaningful quality lift.
- Avoid native/unscaled images (primary cause of poor accuracy), resolutions below 960×540 (too lossy), and 1920×1080+ on 4.6 family (silently downscaled).
- Use `compute_max_api_fit()` to maximize fidelity while preserving aspect ratio.

After resizing, scale API-returned coordinates back to native screen space before executing any click (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

## Content Ordering

Place the text instruction *before* the image in the `content` array. This lets the model know what to look for as it processes the screenshot, improving click accuracy (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

## Model Selection

- **Sonnet 4.6** — most mechanically precise clicks; best balance of accuracy, reasoning, and cost.
- **Opus 4.7** — closes the gap with Sonnet 4.6 on click precision; higher resolution budget reduces downscaling; best for complex reasoning tasks.
- **Haiku 4.5** — best for latency-sensitive work.
- **Orchestrator + sub-agent pattern** — reasoning model plans, Sonnet/Haiku executes mechanical clicks (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

## Handling Small Targets

For checkboxes, icons, toggles, and other tiny elements (source: 2026-05-25-Best practices for computer and browser use with Claude.md):
- Enable `enable_zoom: True` in the tool config for dense UIs.
- Increase target size in the UI itself (lower DPI, browser zoom, display scaling).
- Use keyboard alternatives (Tab navigation, keyboard shortcuts) for very small elements.
- Use Opus 4.7 for 4K+ source images; 4.6 models degrade more with heavy compression.

**Approaches that did not help in testing:** tiling screenshots into quadrants, overlaying grid coordinates, choice of resize algorithm (PIL LANCZOS vs sips vs other — all identical) (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

## Adaptive Thinking / Effort Tuning

**Opus 4.7 recommendations (source: 2026-05-25-Best practices for computer and browser use with Claude.md):**
- Default: `high` — best accuracy-to-token ratio for complex multi-step tasks.
- Cost-sensitive: `low` — still better than Opus 4.6 at any effort.
- One-shot high-stakes: `max`.
- Latency priority: Sonnet 4.6 instead.

**Claude 4.6 recommendations (source: 2026-05-25-Best practices for computer and browser use with Claude.md):**
- Default: `medium` — sweet spot; with retries, matches `high` at half the tokens.
- Cost-sensitive: `low` — actually uses fewer tokens than no-thinking (fewer errors = fewer retries).
- Latency priority: thinking disabled.
- One-shot hard tasks: `high`.
- Do **not** use `max` on 4.6 — no accuracy gain, just added cost.

## Prompt Injection Defense

Computer use agents interact with untrusted content by design; every screenshot could contain adversarial instructions (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

Anthropic's multi-layer defense:
1. **Training-time RL** — Claude learns to identify and refuse injected instructions in simulated UIs.
2. **Real-time classifiers** — probes scan entering content for adversarial commands; run automatically at zero added latency when using the official `computer_20251124` tool type.
3. **Continuous red teaming** — ongoing adversarial evaluation.

Best practices regardless of classifiers (source: 2026-05-25-Best practices for computer and browser use with Claude.md):
- **Human-in-the-loop** for irreversible actions (form submit, purchase, send email).
- **Scope agent permissions** — minimize blast radius.
- **Log all actions** including screenshots at each step.
- **Treat all web content as untrusted** — system prompt should distinguish user instructions from encountered content.

## Context Management for Computer Use

Screenshots accumulate at 1,000–1,800 tokens each; a 200k window fills in under 100 screenshots (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

**Three-layer strategy:**

**1. Cache breakpoints**
- One breakpoint on stable prefix (system prompt / tool definitions).
- Up to three trailing breakpoints on recent `tool_result` blocks — advance each turn, clear previous iteration's breakpoints.

**2. Cache-aware rolling buffer**
- Keep N most recent screenshots; replace older ones with `[Image omitted]` placeholders.
- Prune in **batches** (not one-at-a-time) to preserve cache-stable prefix — default: `keep_n=3`, `interval=25`.

**3. LLM-based compaction**
- Use a structured summarization prompt with sections: User Instructions, Task Template, Constraints, Actions Taken, Errors and Fixes, Progress Tracking, Current State, Next Step.
- Use server-side compaction (`compact-2026-01-12` beta) with custom `instructions` parameter.
- After server-side compaction, truncate local message array to match (`truncate_to_last_compaction`).

## Experimental Features

**Batch tools** (`computer_batch` / `browser_batch`): combine sequential independent actions into a single tool call — reduces round-trips but risks compounding errors if intermediate state changes (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

**Advisor tool** (`advisor_20260301` beta): Sonnet executor consults Opus 4.7 advisor mid-generation for strategic planning. Useful for long-horizon tasks where most turns are mechanical but occasional moments require deep reasoning. Note: orphaned `server_tool_use`/`advisor_tool_result` blocks cause 400 errors if the advisor tool is later removed — strip them with a pre-send pass (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

**Teaching / Teach Mode**: record a demonstration (screenshots + actions + optional voice narration), replay as context during task execution. Claude uses the demo as a guide while adapting to live UI state. Supports strict, adaptive, and goal-oriented playback strictness levels (source: 2026-05-25-Best practices for computer and browser use with Claude.md).

## Key Takeaways

- Pre-downscale all screenshots before sending — this single fix is worth more than almost any other optimization.
- Always place text instruction before image in the content array.
- Use `medium` thinking effort for Claude 4.6 and `high` for Opus 4.7 by default.
- Classify web content as untrusted; use human-in-the-loop for irreversible actions.
- Manage context with cache breakpoints + batch pruning + LLM compaction as three complementary layers.
- The advisor tool and batch tools are experimental but promising for long-horizon tasks.

## Related

- [[advisor-strategy|The Advisor Strategy — Opus Boost for Sonnet/Haiku]] — the advisor tool pattern applies to computer use too
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — context management patterns for multi-session agents
- [[sandcastle-afk-agent-orchestration|Sandcastle — AFK Software Factory]] — Docker-isolated agentic execution environment
- [[cache-diagnostics|Cache Diagnostics]] — debug prompt cache misses that undermine computer use cost management

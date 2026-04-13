# Harnessing Claude's Intelligence — 3 Key Patterns for Building Apps

**Source:** [claude.com/blog/harnessing-claudes-intelligence](https://claude.com/blog/harnessing-claudes-intelligence)
**Author:** Lance Martin (Claude Platform team, Anthropic)
**Published:** 2026-04-02

---

## Summary

Harnesses encode assumptions about what Claude *can't* do on its own — and those assumptions go stale as models improve. Lance Martin proposes three patterns for building apps that keep pace with Claude's evolving intelligence while balancing latency and cost: **use what Claude already knows**, **ask what you can stop doing**, and **set boundaries carefully**.

## 1. Use What Claude Knows

- Build with tools Claude already understands well — bash, text editor — rather than inventing new ones
- Claude 3.5 Sonnet hit (then-SOTA) 49% on SWE-bench Verified with *only* bash + text editor tools
- [[skill-issue-harness-engineering|Skills]], programmatic tool calling, and the memory tool are all compositions built on top of bash + text editor
- A strong coding model is a strong *general* agent, because code is a universal orchestration language

## 2. Ask "What Can I Stop Doing?"

Three sub-patterns, all about letting Claude take over decisions the harness used to make for it.

### Let Claude orchestrate its own actions
- **Old assumption:** every tool result flows through the context window as tokens
- **Problem:** Claude pays the token cost for rows/columns it doesn't need; the harness is making an orchestration decision Claude is better positioned to make
- **Fix:** give Claude a code execution tool (bash or REPL) so it can filter, pipe, and compose tool calls in code — only the final output reaches context
- **Impact:** on BrowseComp, Opus 4.6 jumped from 45.3% → 61.6% by filtering its own tool outputs

### Let Claude manage its own context
- **Old assumption:** system prompts should be hand-crafted with task-specific instructions
- **Problem:** pre-loading rarely-used instructions depletes Claude's attention budget
- **Fix:** [[skill-issue-harness-engineering|Skills]] let Claude progressively disclose task-relevant context via YAML frontmatter descriptions
- **Related:** context editing removes stale tool results / thinking blocks; [[subagents-in-claude-code|sub-agents]] isolate work in fresh context windows (Opus 4.6 gained +2.8% on BrowseComp via sub-agents)

### Let Claude persist its own context
- **Old assumption:** memory systems need retrieval infrastructure
- **Fix:** simple primitives Claude controls — compaction, memory folders
- **Impact on BrowseComp compaction:** Sonnet 4.5 flat at 43% → Opus 4.5 at 68% → Opus 4.6 at 84% (same setup)
- **Memory folder + Sonnet 4.5 on BrowseComp-Plus:** 60.4% → 67.2%
- Pokémon run illustration: Sonnet 3.5 wrote memory as transcript (near-duplicate Caterpie/Weedle files); Opus 4.6 wrote a `learnings.md` with tactical notes distilled from failures

## 3. Set Boundaries Carefully

### Design context to maximize cache hits
Cached tokens are **10% the cost** of base input tokens. Principles:

| Principle | Description |
| --- | --- |
| **Static first, dynamic last** | Stable content (system prompt, tools) comes first |
| **Messages for updates** | Append `<system-reminder>` in messages instead of editing the prompt |
| **Don't change models** | Caches are model-specific. Need a cheaper model? Use a sub-agent |
| **Carefully manage tools** | Tools sit in the cached prefix — adding/removing invalidates. Use **tool search** for dynamic discovery (appends without breaking cache) |
| **Update breakpoints** | Move breakpoint to latest message for multi-turn apps (use auto-caching) |

### Use declarative tools for UX, observability, security boundaries
- A bash tool gives the harness only a command string — the same shape for every action
- Promoting actions to dedicated tools gives the harness **typed arguments** it can intercept, gate, render, audit, log, trace, and replay
- Good candidates: hard-to-reverse actions (external API calls), actions needing user confirmation, write tools that need staleness checks
- Counter-pattern: Claude Code's **auto-mode** (research) uses a second Claude to judge bash command safety, *limiting* the need for dedicated tools — but only for tasks users trust the general direction of

## Looking Forward

> "The context resets we built to compensate had become dead weight in the agent harness."

Assumptions about what Claude *can't* do need to be re-tested with each capability step-change. Sonnet 4.5's "context anxiety" (wrapping up prematurely near context limit) was fixed by adding resets; Opus 4.5 eliminated the behavior, making those resets dead weight. Harness structure should be **pruned** against the question: *what can I stop doing?*

## Key Takeaways

- **Build on tools Claude already knows** — bash + text editor are universal primitives
- **Orchestration, context management, and memory are decisions Claude can increasingly make for itself** — move them out of the harness when capability allows
- **Cache-friendly context design is worth 10x** — static prefix first, dynamic last, don't switch models mid-session
- **Promote actions to typed tools when you need UX/security/observability hooks**, but continually re-evaluate whether the harness boundary is still earning its place
- **Prune dead weight**: the harness you built for Sonnet 4.5 is probably wrong for Opus 4.6

## See Also

- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — context isolation via fresh context windows
- [[advisor-strategy|The Advisor Strategy]] — Opus as advisor to a Sonnet/Haiku executor
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — practitioner guide on configuration surfaces
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's initializer/coder pattern
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — GAN-inspired multi-agent harness
- [[twelve-factor-agents|Twelve-Factor Agents]] — foundational reliability principles

# Best Practices for Claude Opus 4.7 with Claude Code

**Source:** [Anthropic — Best practices for using Claude Opus 4.7 with Claude Code](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code) (April 2026)

---

## Summary

Opus 4.7 is Anthropic's strongest generally available model for coding and agentic tasks. Two changes from 4.6 affect token usage and require tuning: an updated tokenizer and a tendency to reason more at higher effort levels, especially on later turns in long sessions. A few harness and prompt adjustments restore efficiency while capturing the quality gains.

---

## Interactive vs Async Sessions

Opus 4.7 behaves differently depending on how it's deployed:

| Mode | Pattern | Token behaviour |
|---|---|---|
| **Async / single-turn** | Fully specified task in one prompt, model runs autonomously | More efficient; fewer reasoning cycles |
| **Interactive / multi-turn** | Multiple user turns with progressive guidance | More reasoning per turn → more tokens, but better coherence and instruction-following |

**Recommendation:** treat Claude more like a capable engineer you delegate to than a pair programmer you guide line-by-line.

Practical adjustments for interactive sessions:
- **Specify the task fully in the first turn** — include intent, constraints, acceptance criteria, and relevant file locations
- **Batch questions** — avoid a stream of short turns; each user turn adds reasoning overhead
- **Use auto mode** when you trust the model to run unsupervised (research preview, Max plan, toggle with Shift+Tab)
- **Set up completion notifications** — ask Claude to play a sound or create a hook-based notification when done

---

## Effort Levels

A new level `xhigh` sits between `high` and `max` and is now the default.

| Level | When to use |
|---|---|
| `low` / `medium` | Cost- or latency-sensitive work; tightly scoped tasks |
| `high` | Concurrent sessions; cost-conscious without large quality drop |
| `xhigh` *(default)* | Most agentic coding — best intelligence/cost balance; avoids runaway token usage |
| `max` | Genuinely hard problems; evals; intelligence-sensitive, non-cost-sensitive; diminishing returns beyond `xhigh` |

**If upgrading from 4.6:** experiment with effort level rather than porting the old setting. Toggling effort during a task is supported.

---

## Adaptive Thinking

Fixed thinking budget (Extended Thinking from 4.6) **is not supported** in Opus 4.7. Replaced by adaptive thinking:
- Model decides when to invoke thinking at each step
- Simple queries → fast response, no thinking
- Complex steps → thinking budget used where it helps most
- Opus 4.7 is less prone to overthinking than 4.6

**To prompt for more thinking:**
> "Think carefully and step-by-step before responding; this problem is harder than it looks."

**To prompt for less thinking:**
> "Prioritize responding quickly rather than thinking deeply. When in doubt, respond directly."

---

## Behaviour Changes from 4.6 → 4.7

Three defaults have changed — harnesses tuned for 4.6 may need adjustment:

| Behaviour | 4.6 | 4.7 | Harness adjustment if needed |
|---|---|---|---|
| Response length | Verbose by default | Calibrated to task complexity | State length/style explicitly; use positive examples, not "don't do X" |
| Tool use | Calls tools readily | Calls tools less; reasons more instead | Explicitly describe when and why a tool should be used |
| Subagent spawning | More aggressive | More judicious | Spell out when parallel subagents are beneficial |

**Example subagent guidance to add to harness:**
```
Do not spawn a subagent for work you can complete directly in a single response.
Spawn multiple subagents in the same turn when fanning out across items or reading multiple files.
```

---

## Best Fit Use Cases

Opus 4.7 outperforms prior models most noticeably on:
- Complex multi-file changes
- Ambiguous debugging
- Code review across a service
- Multi-step agentic work where human supervision was the bottleneck

---

## Key Takeaways

- **`xhigh` is the new default effort** — the right setting for most agentic coding; `max` shows diminishing returns and is prone to overthinking
- **Front-load context** — full task specification in the first turn is the single highest-leverage prompt change for interactive sessions
- **Adaptive thinking replaces fixed budgets** — the model self-regulates; prompt for more/less thinking if needed
- **Three defaults changed from 4.6**: shorter responses, fewer tool calls, fewer subagents — explicitly instruct where the old behaviour is needed
- **Harnesses that worked for 4.6 need re-testing** — this aligns with the cross-cutting principle: *"re-examine the harness when the model changes"* (see [[agentic-engineering-approaches-compared]])

## See Also

- [[agentic-engineering-approaches-compared]] — cross-cutting principle: harness assumptions expire with model upgrades
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — CLAUDE.md, sub-agents, back-pressure; configuration layer this article tunes
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — when Opus 4.7's reduced default spawning needs explicit override
- [[advisor-strategy|The Advisor Strategy — Opus Boost for Sonnet/Haiku]] — complementary pattern for mixing model tiers

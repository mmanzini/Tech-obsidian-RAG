---
type: synthesis
title: Claude Code Effort Level and Model Selection
description: Anthropic's mental model for the two "make it better" dials in Claude Code — model selection swaps which frozen weights answer (capability), effort level controls how much work Claude does per turn (thoroughness) — and the diagnostic question "did it not know enough, or not try hard enough?".
bundle: ai-engineering
topic: claude-code-practice
tags:
- claude-code
- harness-engineering
- context-engineering
resource: https://claude.com/blog/claude-model-and-effort-level-in-claude-code
sources:
- id: 2026-07-29-claude-code-effort-level-and-model-selection-claude
  resource: Resources/web-clippings/2026-07-29-Claude Code effort level and model selection  Claude.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-05T09:00:00Z'
status: stable
related:
- ai-engineering/claude-code-practice/opus-4-7-best-practices.md
- ai-engineering/claude-code-practice/claude-code-quality-postmortem.md
- ai-engineering/harness-engineering/steering-claude-code-instruction-methods.md
- ai-engineering/harness-engineering/advisor-strategy.md
- ai-engineering/model-fundamentals/compression-is-intelligence.md
---

# Claude Code Effort Level and Model Selection

**Source:** [Claude Code effort level and model selection](https://claude.com/blog/claude-model-and-effort-level-in-claude-code) (Anthropic, Claude Code blog)
**Author:** Lydia Hallie, Claude Code team

---

## Summary

Claude Code has two settings that appear to "make the answer better": model and effort. Model selection swaps which set of frozen weights handles the request — the overall capability range and knowledge base, fixed at training time. Effort means more than thinking time: it controls how much work Claude does on the request overall — files read, verification performed, and how far it pushes through a multi-step task before checking back in (source: 2026-07-29-Claude Code effort level and model selection  Claude.md). The operating heuristic: with clear context provided, if Claude clearly tried and still got it wrong, pick a more capable model; if it got it wrong by skipping a file, not running tests, or bailing on a refactor partway, pick a higher effort level (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).

## How model selection works (the mechanics)

- On enter, Claude Code assembles prompt + system prompt + tool definitions + CLAUDE.md + history + files into one API request; server-side tokenization maps text to integers from a fixed vocabulary (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- The model computes a probability for every vocabulary token and picks from the top; what turns input into probabilities is the **weights** — billions of parameters set during training and read-only at inference. Nothing in the prompt, CLAUDE.md, or context changes them (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- Context *steers* prediction (putting real code or docs in front of Claude works well) but doesn't *teach* — a library absent from training isn't in the weights, and a hallucinated API call is the weights producing a plausible-looking sequence, not a failed lookup (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- Generation is one token per pass, re-reading the whole sequence each time — this loop is where most wait time and output cost come from. The model setting decides which weights run and what each output token costs; it does not decide how many tokens get generated (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).

## How effort works

- All output — thinking, tool calls, text to the user — is ordinary output tokens from the same loop, billed at the same rate; earlier reasoning stays in context for the rest of the turn (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- The effort level is sent as part of the request; behaviour per level is trained into the weights. It sets how thorough and certain Claude needs to be before considering the task done — considered on every turn (source: 2026-07-29-Claude Code effort level and model selection  Claude.md). High effort can generate roughly 7× more tokens on the same prompt to reach a higher-confidence answer (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- Plans aren't frozen: when step 1 of a three-hypothesis debugging plan finds the bug, higher-effort Claude still skips the now-unnecessary checks — training specifically targets "overthinking" because it degrades effectiveness (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- At lower effort, Claude would rather ask for more context than spend tokens figuring things out on its own (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).

## Picking the settings

- **Use the model's default effort for most tasks**; treat effort as a general preference tied to your domain, not a task-by-task decision (source: 2026-07-29-Claude Code effort level and model selection  Claude.md). Opus 4.8 at default effort produces better results for about the same tokens as Opus 4.7 at default (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- **First response to a miss is not a knob** — examine the context: vague prompt, wrong tools, missing skills. If you're raising effort on a task that shouldn't need it, the fix is upstream (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- **Larger model** for genuinely hard problems: subtle bugs, unfamiliar domains, architecture decisions, ambiguity; smaller model for routine, precisely describable work — specific instructions are the better recipe on smaller models (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).

### The specialist, the expert, and the generalist

Fable is a specialist who's seen problems almost no one else has; Opus is the expert; Sonnet is a really good generalist — effort decides how much time any of them spends. Opus at low effort = five minutes with an expert (pattern recognition, quick read); Sonnet at high effort = a generalist with the whole afternoon (reads everything, understands *your* code thoroughly); Fable even at low effort spots the thing no one else would — that recognition is what you pay for, so save it for tasks that need it (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).

## Token economics

- Routine work: both models get it right; the larger one burns more tokens at a higher price — drop down for routine stretches at no quality cost (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- Hard multi-step work: the smaller model grinds toward its limit burning iterations while the larger reaches the bar in fewer steps — total cost per task can come out *lower* on the larger model, and Fable finishes jobs Opus and Sonnet can't reach at any effort level (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).
- Effort shapes consumption but doesn't cap it; the only hard cap is `max_tokens` (blunt, mid-stream truncation). Task budgets and "keep it brief" prompts are advisory guidance the model is trained to follow (source: 2026-07-29-Claude Code effort level and model selection  Claude.md).

## Key Takeaways

- Model = *how capable* (which frozen weights); effort = *how thorough* (how much work per turn). Most real tasks need some of both.
- Diagnostic: didn't **know** enough → bigger model; didn't **try** hard enough (skipped file, no tests, bailed mid-refactor) → higher effort.
- Context is steering, not teaching — check prompt/tools/skills before touching either dial.
- Defaults first; tune as a standing preference per type of work, not per task.
- On genuinely hard work, the pricier model can be the cheaper one per task.

## Related

- [[opus-4-7-best-practices]] · [Opus 4.7 Best Practices](../claude-code-practice/opus-4-7-best-practices.md) — the per-model effort ladder (low→max, xhigh default) this article generalises
- [[claude-code-quality-postmortem]] · [Claude Code Quality Postmortem](../claude-code-practice/claude-code-quality-postmortem.md) — what happens when effort is silently downgraded: the April 2026 degradation
- [[steering-claude-code-instruction-methods]] · [Steering Claude Code](../harness-engineering/steering-claude-code-instruction-methods.md) — the seven instruction methods; steering is the third dial beside model and effort
- [[advisor-strategy]] · [The Advisor Strategy](../harness-engineering/advisor-strategy.md) — mixing model tiers instead of choosing one
- [[compression-is-intelligence]] · [Compression is Intelligence](../model-fundamentals/compression-is-intelligence.md) — why next-token prediction over frozen weights is the machinery underneath both dials

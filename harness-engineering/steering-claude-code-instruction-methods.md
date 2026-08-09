---
type: synthesis
title: Steering Claude Code — When to Use CLAUDE.md, Rules, Skills, Hooks, and Subagents
description: Anthropic's decision framework for the seven instruction methods in Claude Code — CLAUDE.md, rules, skills, subagents, hooks, output styles, and system-prompt appends — compared by load timing, compaction behaviour, context cost, and authority.
bundle: ai-engineering
topic: harness-engineering
tags:
- harness-engineering
- claude-code
- context-engineering
- skills-and-hooks
resource: https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more
sources:
- id: 2026-07-29-steering-claude-code-when-to-use-claude-md-skills-hooks-and-subagents
  resource: Resources/web-clippings/2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-08-05T09:00:00Z'
status: stable
related:
- ai-engineering/harness-engineering/skill-issue-harness-engineering.md
- ai-engineering/harness-engineering/hooks-for-deterministic-cli-enforcement.md
- ai-engineering/claude-code-practice/claude-code-effort-and-model-selection.md
- ai-engineering/claude-code-practice/subagents-in-claude-code.md
- ai-engineering/harness-engineering/verification-loops-in-claude-code.md
---

# Steering Claude Code — When to Use CLAUDE.md, Rules, Skills, Hooks, and Subagents

**Source:** [Steering Claude Code: when to use CLAUDE.md, skills, hooks, and subagents](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more) (Anthropic, Claude Code blog)
**Author:** Michael Segner, Anthropic

---

## Summary

Claude Code offers seven methods for instructing behaviour — CLAUDE.md files, rules, skills, subagents, hooks, output styles, and appending the system prompt. Each method controls when an instruction loads into context, whether it persists through compaction, and how much authority it carries; each trades context cost against authority (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md). These methods influence behaviour, while two separate dials — model and effort level — control how capable Claude is and how hard it works (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).

## The comparison table

| Method | When loaded | Compaction behaviour | Context cost | Use for |
|---|---|---|---|---|
| CLAUDE.md (root) | Session start, whole session | Memoized; re-read after compaction | High — every line costs tokens whether relevant or not | Build commands, directory layout, monorepo structure, conventions, team norms |
| CLAUDE.md (subdirectory) | On-demand when Claude reads a file under it | Lost until subdirectory touched again | Low | Subdirectory-specific conventions |
| Rules | Session start (unscoped) or when matching files touched (path-scoped) | Re-injected on compaction | Medium; always-on unless path-scoped | Specific constraints (e.g. all API handlers validate input with Zod) |
| Skills | Name+description at start; body on invocation | Invoked skills re-injected up to a shared budget, oldest dropped first | Low | Procedural workflows (deploy/release checklists) |
| Subagents | Name/description/tools at start; body only when called | Only the final message returns to the main session | Low; isolated context window | Parallel or side tasks returning only a summary (deep search, log analysis, dependency audit) |
| Hooks | Fire on lifecycle events | Bypass compaction entirely | Low; config lives outside context | Deterministic automation: linters, Slack posts, blocking commands |
| Output styles | Session start, injected into system prompt | Never compacted | High; overwrites default system prompt | Significant role changes |
| Append system prompt | CLI flag, per invocation | Never compacted | Moderate (cached after first request) | Tone, response length, formatting |

(source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md)

## Method notes

- **CLAUDE.md** grows like any unowned config file in a shared repo: every team appends, nothing gets deleted, every line loads for every engineer and dilutes adherence to the instructions that matter. Keep it under 200 lines, give it an owner, review changes like code; push team conventions into path-scoped rules and procedures into skills (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md). In monorepos, per-team subdirectory CLAUDE.md files plus the `claudeMdExcludes` setting keep loads scoped; org-wide standards can ship as a centrally managed CLAUDE.md via MDM that individual settings cannot exclude (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **Rules** live in `.claude/rules/`; a `paths:` frontmatter field (e.g. `src/api/**`) keeps a rule out of context during unrelated sessions. An unscoped rule is mechanically identical to putting the content in CLAUDE.md (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **Skills** load name+description at session start and the full body only on invocation (slash command or auto-match) — procedural content belongs here, not in CLAUDE.md (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **Subagents** are the isolation tool: the body never enters the parent conversation, work runs in a fresh context window, and only the final message plus metadata returns. They nest up to five levels deep; dynamic workflows orchestrate tens to hundreds with the plan held in script variables. Use a subagent when intermediate results would clutter the main thread; use a skill when you want to see and steer each step (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **Hooks** come in five types — command, HTTP, mcp_tool (deterministic execution) and prompt, agent (Claude-judged output) — all deterministically triggered on lifecycle events, registered in settings.json, managed policy, or skill/agent frontmatter. Most hook output never enters context unless explicitly returned (a blocking hook's stderr is the exception, so Claude knows why a call was denied) (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **Output styles** carry the highest instruction-following weight and by default *replace* Claude Code's software-engineering system prompt (scoping, comments, security, verification habits) unless `keep-coding-instructions: true` — check the built-in Proactive/Explanatory/Learning styles before writing one (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **`append-system-prompt`** is additive-only and per-invocation; adherence has diminishing returns as appended instructions grow or contradict (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).

## Anti-pattern smells (the decision framework)

- **"Every time X, always do Y" in CLAUDE.md** → that's a hook. A model choosing to run a formatter is different from the formatter running automatically (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **"Never do this" in CLAUDE.md** → an instruction is the wrong tool for a hard guardrail; under pressure, in long sessions, or under prompt injection the model can fail to follow it. Real guardrails are hooks (PreToolUse exit code 2) and permissions; managed settings are the only org-wide, non-overridable enforcement (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **A 30-line procedure in CLAUDE.md** → belongs in a skill; CLAUDE.md is for always-held facts (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **An API-specific rule without `paths:`** → scope it; unscoped is always-loaded (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).
- **Personal preferences in project-level CLAUDE.md** → every file-based method has a user-level counterpart; keep project files for team-wide conventions (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).

Once several are working, bundle skills, subagents, hooks, and output styles as a plugin to share a coherent setup (source: 2026-07-29-Steering Claude Code when to use CLAUDE.md, skills, hooks, and subagents.md).

## Key Takeaways

- Pick methods by three axes: when it loads, whether it survives compaction, and how much authority it carries — every method trades context cost against authority.
- Reliability boundary: instructions (CLAUDE.md, rules) are probabilistic; hooks, permissions, and managed settings are the deterministic layer for anything that must always or never happen.
- Cost boundary: always-on content (root CLAUDE.md, unscoped rules, output styles) is expensive; path-scoping, skills, and subagents make instructions pay-per-relevance.
- Steering (these seven methods) is orthogonal to capability (model choice) and diligence (effort level).

## Related

- [[skill-issue-harness-engineering]] · [Skill Issue — Harness Engineering](../harness-engineering/skill-issue-harness-engineering.md) — practitioner counterpart covering the same levers with field lessons (context firewalls, back-pressure)
- [[hooks-for-deterministic-cli-enforcement]] · [Hooks for Deterministic CLI Enforcement](../harness-engineering/hooks-for-deterministic-cli-enforcement.md) — deep-dive on the "never do this belongs in a hook" rule
- [[claude-code-effort-and-model-selection]] · [Claude Code Effort Level and Model Selection](../claude-code-practice/claude-code-effort-and-model-selection.md) — the two companion dials this article explicitly defers to
- [[verification-loops-in-claude-code]] · [Verification Loops in Claude Code](../harness-engineering/verification-loops-in-claude-code.md) — skills-as-verification, the workflow layer built on these methods

# How and When to Use Subagents in Claude Code

**Source:** [claude.com/blog/subagents-in-claude-code](https://claude.com/blog/subagents-in-claude-code)
**Published:** 2026-04-07

---

## Summary

A sub-agent is an **isolated Claude instance with its own context window**. It takes a task, works independently, and returns only the result. Think of sub-agents as the **browser tabs** of a Claude Code session — a place to chase a tangent without losing the main thread. This article maps when sub-agents pay off, how to invoke them, and when the overhead isn't worth it.

## What Is a Sub-Agent?

- Self-contained agent with its own context window, unburdened by the main conversation's history
- Can run in parallel; each can have different permissions (read-only research agent vs. full-edit implementation agent)
- Claude Code ships with built-in types: **general-purpose**, **Plan**, **Explore** (fast read-only code search)
- Claude often spawns sub-agents on its own, but explicit direction and reusable custom specialists make the feature much more effective

## When to Use Sub-Agents

| Signal | Benefit |
|---|---|
| **Research-heavy** — gathering context requires reading dozens of files | Main conversation stays clean; synthesized findings arrive instead of raw content |
| **Multiple independent tasks** — sub-tasks have no dependencies | 3 sub-agents in parallel ≈ time of 1 |
| **Fresh perspective needed** — unbiased review without conversation history | Cleaner, more objective feedback (better than `/clear` because the main thread stays intact) |
| **Verification before commit** — second opinion to catch edge cases / overfitting to tests | Catches issues that familiarity obscures |
| **Pipeline workflows** — sequential phases with clear handoffs (design → implement → test) | Each stage stays focused without bleed from others |

**Rule of thumb:** exploring 10+ files or 3+ independent pieces of work → reach for sub-agents.

## How to Direct Sub-Agent Usage

### 1. Conversational invocation
Most flexible. Effective prompts:
- Scope tasks clearly ("explore how payments work" > "explore everything")
- Request parallelization explicitly ("these can run in parallel")
- Specify output format (summary, findings, recommendations)
- Ask for fresh context when unbiased analysis matters

Example structure:
```markdown
Use sub-agents to explore this codebase in parallel:
1. Find all API endpoints and summarize their purposes
2. Identify the database schema and relationships
3. Map out the authentication flow

Return a summary of each, not the full file contents.
```

**Ctrl+B** backgrounds a running sub-agent; `/tasks` shows what's running.

### 2. Custom Sub-Agents (reusable specialists)
Markdown files in `.claude/agents/` (project) or `~/.claude/agents/` (user). Each has its own system prompt, tool permissions, and optionally its own model. Example:

```markdown
---
name: security-reviewer
description: Reviews code changes for security vulnerabilities, injection
  risks, auth issues, and sensitive data exposure. Use proactively before
  commits touching auth, payments, or user data.
tools: Read, Grep, Glob
model: sonnet
---

You are a security-focused code reviewer. Analyze the provided changes for:
- SQL injection, XSS, and command injection risks
- Authentication and authorization gaps
- Sensitive data in logs, errors, or responses
- Insecure dependencies or configurations

Return a prioritized list of findings with file:line references and a
recommended fix for each. Be critical. If you find nothing, say so
explicitly rather than inventing issues.
```

**Pro-tip:** the `description` field is how Claude decides when to delegate — describe *trigger conditions*, not just capability. "Reviews code for security issues before commits" routes better than "security expert."

### 3. CLAUDE.md instructions
Custom sub-agents define *who* the specialists are; CLAUDE.md defines *when* to reach for them. Good fit for:
- Always routing code reviews through read-only sub-agents
- Project-specific research patterns
- Team-consistent behavior across sessions

### 4. Skills
Reusable multi-step workflows, loaded on demand. Contrast with CLAUDE.md (always loaded). A skill is a directory, not a single file — can bundle SKILL.md, templates, example outputs, or scripts.

```markdown
# .claude/skills/deep-review/SKILL.md
---
name: deep-review
description: Comprehensive code review that checks security, performance,
  and style in parallel. Use when reviewing staged changes before a
  commit or PR.
---

Run three parallel sub-agent reviews on the staged changes:
1. Security review — vulnerabilities, injection, auth, sensitive data
2. Performance review — N+1 queries, memory leaks, blocking ops
3. Style review — consistency with /docs/style-guide.md

Synthesize into a single priority-ranked summary with file:line refs.
```

### 5. Hooks
Most automated. Shell commands, HTTP endpoints, or LLM prompts executed on Claude Code lifecycle events. Good for:
- Auto-review every commit before it's created
- CI-like quality gates in local dev
- Security checks that shouldn't depend on anyone remembering

Example Stop-hook that blocks ending the turn until tests pass (with `stop_hook_active` guard against infinite loops). Start with conversational or CLAUDE.md; graduate to hooks as patterns mature.

## Practical Patterns

- **Research before implementing** — delegate codebase exploration first, then plan with synthesized findings (see [[research-plan-implement|RPI]])
- **Parallel modifications** — one sub-agent per file following a pattern established elsewhere
- **Independent review** — fresh sub-agent with read-only access, explicitly excluded from prior discussion
- **Pipeline workflow** — design sub-agent → implementation sub-agent → test sub-agent, using files as handoff

## When NOT to Use Sub-Agents

- **Sequential dependent work** — step 2 needs full output of step 1 → single session is cleaner than a file-based relay
- **Same-file edits** — parallel writers conflict; keep tightly coupled changes in one context
- **Small tasks** — delegation overhead > benefit
- **Too many specialists** — flooding Claude with custom agents degrades auto-delegation; most teams settle on a small, well-scoped roster
- **Agents needing to coordinate with each other** — sub-agents can't talk to one another; for that, use **Agent Teams** (separate sessions, heavier, more expensive)

## Key Takeaways

- A sub-agent is a **context firewall**: isolated context window, returns only results to the parent
- Reach for sub-agents on: research, parallel independent tasks, fresh-perspective review, verification, pipeline handoffs
- **Start conversational, automate later** — graduate to custom agents → CLAUDE.md → skills → hooks as patterns emerge
- Custom sub-agent `description` fields should name **trigger conditions**, not capabilities
- Overhead is real — don't delegate small, sequential, or tightly-coupled work
- For true agent-to-agent coordination, use **Agent Teams**, not sub-agents

## See Also

- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence — 3 Key Patterns]] — sub-agents as a way to let Claude manage its own context
- [[advisor-strategy|The Advisor Strategy]] — inverts the orchestrator-delegates-to-workers pattern
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — "context firewall" framing, warning against role-playing sub-agents
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — initializer/coder sub-agent pattern
- [[research-plan-implement|Research Plan Implement]] — workflow that naturally maps to sub-agent pipelines
- [[adversarial-review|Adversarial Review]] — fresh-perspective review as a first-class sub-agent use case
- [[twelve-factor-agents|Twelve-Factor Agents]] — small focused agents principle

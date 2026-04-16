# Claude Code Routines

**Source:** [Anthropic — Automate work with routines](https://code.claude.com/docs/en/routines) (2026, research preview)

---

## Summary

Routines are saved Claude Code configurations that run autonomously on Anthropic-managed cloud infrastructure — no local machine required. A routine packages a prompt, repositories, and connectors, and attaches one or more trigger types. Each run is a full autonomous Claude Code session with no approval prompts.

---

## Trigger Types

| Trigger | How it fires |
|---|---|
| **Scheduled** | Recurring cadence: hourly, daily, weekdays, weekly (minimum 1-hour interval) |
| **API** | HTTP POST to a per-routine endpoint with a bearer token; optional `text` field passes run-specific context |
| **GitHub** | Reacts to repository events: PR opened/closed, push, issues, releases, workflow runs, etc. |

A single routine can combine all three trigger types.

---

## Key Use Cases

- **Backlog maintenance** — nightly run labels and assigns issues opened since last run, posts summary to Slack
- **Alert triage** — monitoring tool calls API endpoint; routine correlates stack trace with recent commits, opens a draft PR
- **Bespoke code review** — GitHub trigger on `pull_request.opened`; applies team checklist, leaves inline comments
- **Deploy verification** — CD pipeline calls API endpoint post-deploy; routine runs smoke checks and posts go/no-go
- **Docs drift** — weekly scheduled scan of merged PRs; flags docs that reference changed APIs
- **Library port** — GitHub trigger on merged PRs; ports changes to a parallel SDK

---

## What a Routine Can Do

- Run shell commands
- Use skills committed to cloned repositories
- Call any included MCP connectors (Slack, Linear, Google Drive, etc.)
- Create commits and pull requests (under `claude/` branch prefix by default)
- Interact with connected services under the account owner's identity

---

## Scope and Constraints

- Routines are per-account (not shared with teammates)
- Branch push restriction: `claude/` prefix by default; unrestricted push can be enabled per-repository
- Connector scope: all connected connectors included by default — trim to only what the routine needs
- Usage: draws down subscription usage like interactive sessions; daily run cap applies

---

## Creating and Managing Routines

- **CLI**: `/schedule` to create a scheduled routine conversationally; `/schedule list`, `/schedule update`, `/schedule run` to manage
- **Web**: `claude.ai/code/routines` for full management including API tokens and GitHub triggers
- **Desktop**: New task → New remote task (distinct from local Desktop scheduled tasks)

---

## API Trigger Reference

```bash
curl -X POST https://api.anthropic.com/v1/claude_code/routines/{routine_id}/fire \
  -H "Authorization: Bearer {token}" \
  -H "anthropic-beta: experimental-cc-routine-2026-04-01" \
  -H "Content-Type: application/json" \
  -d '{"text": "optional context"}'
```

Returns: `{ "claude_code_session_id": "...", "claude_code_session_url": "..." }`

---

## Key Takeaways

- Routines externalise autonomous Claude Code work from the local machine — runs continue when the laptop is closed
- The three trigger types cover time-based, event-driven, and on-demand automation patterns
- **Scope discipline is critical**: limit repositories, branches, and connectors to what the routine actually needs — each action appears as the account owner
- API trigger is the bridge between Claude Code and external systems (alerting, CI/CD, internal tools)
- Research preview: API shapes, rate limits, and token semantics may change

## See Also

- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — the broader harness configuration that routines operate within
- [[claude-code-desktop-parallel|Claude Code Desktop Redesign for Parallel Agents]] — the desktop UX for managing multiple sessions/routines
- [[hooks-for-deterministic-cli-enforcement|Hooks for Deterministic CLI Enforcement]] — deterministic rules that complement routine prompts

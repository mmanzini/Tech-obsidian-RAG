# Claude Code GitHub Actions

**Source:** [code.claude.com/docs/en/github-actions](https://code.claude.com/docs/en/github-actions) (Anthropic docs, 2026-04-11)

GitHub Action that runs Claude Code inside your workflows. Responds to `@claude` mentions in PRs and issues, or runs headless on scheduled/triggered events. Built on top of the [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk/overview); defaults to Sonnet unless you set `--model claude-opus-4-6`.

## Setup paths

- **Quick setup:** inside a local Claude Code session, run `/install-github-app` — guides through app install and secret setup. Requires repo-admin and direct Claude API (not Bedrock/Vertex).
- **Manual setup:**
  1. Install the [Claude GitHub app](https://github.com/apps/claude) (Contents + Issues + PRs read/write).
  2. Add `ANTHROPIC_API_KEY` to repo secrets.
  3. Copy [examples/claude.yml](https://github.com/anthropics/claude-code-action/blob/main/examples/claude.yml) into `.github/workflows/`.
- Enterprise: Bedrock and Vertex AI supported via OIDC + workload identity federation (no API key in secrets).

## v1.0 migration from beta

Breaking changes all beta users must apply:

| Old beta input | New v1.0 input |
|---|---|
| `mode: "tag"` / `"agent"` | *Removed — auto-detected* |
| `direct_prompt` | `prompt` |
| `custom_instructions` | `claude_args: --append-system-prompt` |
| `max_turns`, `model`, `allowed_tools` | `claude_args: --max-turns` etc. |
| `claude_env` | `settings` JSON |

Mode is now auto-detected: with a `prompt` present, the action runs headless; without one, it responds to `@claude` mentions in comments.

## Core parameters

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    prompt: "Your instructions here"        # optional
    claude_args: "--max-turns 5 --model claude-sonnet-4-6"
    trigger_phrase: "@claude"               # optional override
    use_bedrock: false                      # or use_vertex
```

`claude_args` is raw CLI passthrough — any Claude Code flag works, including `--mcp-config`, `--allowedTools`, `--debug`, `--append-system-prompt`.

## Typical use cases

- **Interactive:** `@claude implement this feature`, `@claude fix the TypeError in the dashboard`, `@claude how should I handle auth for this endpoint?` in any PR/issue comment.
- **PR review on every push:** prompt-driven review on `pull_request: [opened, synchronize]`.
- **Scheduled reports:** cron workflow that drafts daily commit/issue summaries.
- **Skills invocation:** name an installed [skill](https://code.claude.com/docs/en/skills) directly in the prompt.

## Cost & reliability controls

- GitHub Actions minutes on top of Anthropic API token cost — scales with task complexity.
- Set `--max-turns` to cap runaway iterations.
- Use workflow `timeout-minutes` and GitHub concurrency controls to prevent parallel runs.
- For CI triggered by Claude's own commits: use the GitHub App (not the default Actions user) so workflows fire on its pushes.

## Customization surface

Two places to shape behavior:

1. **`CLAUDE.md`** at the repo root — coding standards, review criteria, project rules. See [[harness-engineering/skill-issue-harness-engineering|harness engineering]].
2. **`prompt` parameter** — workflow-specific override per job.

Never hardcode API keys; always use `${{ secrets.ANTHROPIC_API_KEY }}` and limit action permissions.

## Key Takeaways

- `@claude` in a comment is the default interactive path; a `prompt:` field flips the action into headless/automation mode.
- v1.0 consolidates beta config: mode is auto-detected, most flags move under `claude_args`.
- `CLAUDE.md` drives standards; `prompt` drives workflow-specific behavior — same duality as [[ci-integrations/claude-code-code-review|Code Review]].
- Cost hygiene: cap `--max-turns`, set workflow timeouts, prefer specific `@claude` commands over broad prompts.
- For self-hosted or managed PR review, see [[ci-integrations/claude-code-code-review|Code Review]] instead of rolling your own.

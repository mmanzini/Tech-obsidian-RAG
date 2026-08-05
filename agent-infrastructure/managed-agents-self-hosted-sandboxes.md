---
type: synthesis
title: Self-Hosted Sandboxes — Bring-Your-Own Execution Environment for Managed Agents
description: Self-hosted sandboxes let you keep Managed Agents orchestration on Anthropic's side while moving tool execution into infrastructure you control.
bundle: ai-engineering
topic: agent-infrastructure
tags: [agent-architecture, harness-engineering, long-running-agents, ai-security]
source: https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes
resource: https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes
timestamp: 2026-05-25T00:17:36Z
status: active
related:
  - ai-engineering/agent-infrastructure/scaling-managed-agents.md
  - ai-engineering/agent-infrastructure/managed-agents-dreams.md
  - ai-engineering/claude-code-practice/claude-managed-agents-memory.md
---

# Self-Hosted Sandboxes — Bring-Your-Own Execution Environment for Managed Agents

**Source:** [Self-hosted sandboxes](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes)

---

## Summary

Self-hosted sandboxes let you keep Managed Agents orchestration on Anthropic's side while moving tool execution into infrastructure you control. The agent's code, filesystem, and network egress never leave your environment. The right fit when agents must operate on data that cannot leave your network boundary, reach internal services that aren't publicly routable, or run under your organization's own compliance and audit controls.

## Cloud vs. Self-Hosted

|  | Cloud environment | Self-hosted sandbox |
|---|---|---|
| Where tools run | Anthropic-managed containers | Your infrastructure |
| Network reach | Anthropic's egress controls | Your network policy |
| File and GitHub repo mounting | Managed by Anthropic | Managed by you |
| Lifecycle | Managed by Anthropic | Managed by you |

Self-hosted sandboxes are not yet available on Claude Platform on AWS (source: managed-agents-self-hosted-sandboxes.md).

## MCP Tunnels Relationship

Self-hosting controls *where the agent's code executes*. MCP tunnels control *how Anthropic reaches MCP servers in your network*. They are independent: a cloud-container session can still reach private MCP servers through a tunnel; a self-hosted session can use tunneled or public MCP servers. Use both when you want both execution and tool access to stay inside your boundary (source: managed-agents-self-hosted-sandboxes.md).

## Environment Worker Architecture

An **environment worker** is a process you run on your own infrastructure that receives tool execution requests from Anthropic and runs them locally. The `self_hosted` environment is a work queue connecting Anthropic's orchestration to your worker (source: managed-agents-self-hosted-sandboxes.md):

1. Anthropic enqueues a session as a work item when assigned to the environment
2. Your worker claims items from the queue
3. Worker spawns an execution context for each session
4. Worker downloads the agent's skills, runs tool calls locally, posts results back

Two worker patterns (source: managed-agents-self-hosted-sandboxes.md):
- **Always-on worker**: polls continuously (CLI and SDK)
- **Webhook-triggered handler**: wakes on `session.status_run_started` and starts polling (SDK only)

SDK helpers require `/bin/bash` at that exact path. TypeScript SDK additionally requires `unzip`, `tar`, and Node.js 22+. These are resolved at fixed paths and do not respect `PATH` overrides (source: managed-agents-self-hosted-sandboxes.md).

## Filesystem Layout

- **`/workspace`**: default working directory for tool execution and skill download. Skills download to `/workspace/skills/<name>/`
- **`/mnt/session/outputs`**: agent writes final output files here. Mount a host directory here to retrieve them (source: managed-agents-self-hosted-sandboxes.md)

## Setup Steps

1. Create a self-hosted environment via the Console or API
2. Generate an environment key in the Console and export `ANTHROPIC_ENVIRONMENT_KEY` and `ANTHROPIC_ENVIRONMENT_ID` on the worker host
3. Install the `ant` CLI on the worker machine
4. Run the worker with `ant beta:worker poll --workdir "/workspace"`

**Container-per-session isolation**: for stronger isolation (fresh filesystem, resource limits, network controls per session), build an image with `ant` installed and `ant beta:worker run` as the entrypoint. Write a spawn script that forwards session details into a fresh container; start the poller with `--on-work ./spawn.sh` (source: managed-agents-self-hosted-sandboxes.md).

Platform-specific guides are available for Cloudflare, Daytona, Modal, and Vercel (source: managed-agents-self-hosted-sandboxes.md).

## Starting a Session

Create a session targeting the environment's `environment_id`. Pass session metadata to help your orchestration layer mount the relevant files before execution (source: managed-agents-self-hosted-sandboxes.md):

```python
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
    metadata={"input_file": "s3://my-bucket/data.csv"},
)
```

Memory is not yet supported with self-hosted sandboxes (source: managed-agents-self-hosted-sandboxes.md).

## Monitoring and Operations

Key `work` API calls (authenticated with org API key, called from outside the worker host):

- `work.stats`: returns queue `depth` (waiting), `pending` (claimed, in-flight), `oldest_queued_at`, and `workers_polling` (liveness signal over last 30 seconds) — use for scaling and alerting
- `work.stop`: stop a specific session gracefully (finishes in-flight tool call) or with `force: true` to interrupt immediately (source: managed-agents-self-hosted-sandboxes.md)

Do not set `ANTHROPIC_API_KEY` on the worker host — this exposes an org-scoped credential to agent tool calls (source: managed-agents-self-hosted-sandboxes.md).

## Worker CLI Reference

| Flag | Description |
|---|---|
| `--environment-id` | Environment to poll (or `ANTHROPIC_ENVIRONMENT_ID`) |
| `--environment-key` | Worker authentication (or `ANTHROPIC_ENVIRONMENT_KEY`) |
| `--workdir` | Skill download and tool R/W directory. Defaults to `/workspace` |
| `--on-work` | Script to call per work item instead of in-process execution |
| `--unrestricted-paths` | Allow tool calls to access paths outside `--workdir` |
| `--max-idle` | Idle time before shutdown after `end_turn`. Defaults to 60s |
| `--log-format` | `text` or `json` for structured log ingestion |

## Key Takeaways

- Self-hosted sandboxes keep Anthropic's orchestration but move execution inside your network — the right choice for data residency and internal service access requirements
- Two isolation levels: in-process (simpler) or container-per-session (stronger isolation)
- Use `work.stats` for queue depth monitoring and fleet scaling
- MCP tunnels and self-hosted sandboxes are orthogonal and composable
- Memory stores not yet supported in self-hosted sandboxes

## Related

- [[scaling-managed-agents|Scaling Managed Agents]] — broader Managed Agents architecture including the cloud container default
- [[managed-agents-dreams|Managed Agents Dreams — Async Memory Curation]] — memory feature context (not yet supported with self-hosted)
- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — the memory stores that self-hosted sandboxes don't yet support

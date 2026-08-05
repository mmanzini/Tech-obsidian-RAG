# Agent Infrastructure

The execution substrate agents run on, as opposed to the harness design that sits above it: managed-agent services and sandboxes, multi-agent orchestration runtimes, and the tool surfaces (browser/computer use, MCP apps, cache diagnostics) an agent reaches through. Split out of `harness-engineering` in August 2026 once that topic passed 25 articles — theory, skills, and instruction mechanics stayed there; anything about *where and on what* agents execute moved here.

## Articles

- [[scaling-managed-agents|Scaling Managed Agents — Decoupling Brain from Hands]] — brain/hands/session decoupled as independent interfaces; OS-analogy meta-harness; TTFT −60% p50 (Anthropic engineering)
- [[managed-agents-self-hosted-sandboxes|Self-Hosted Sandboxes — Bring-Your-Own Execution Environment]] — keep orchestration on Anthropic's side; move tool execution into your own infrastructure (Anthropic)
- [[managed-agents-dreams|Managed Agents Dreams — Async Memory Curation via Session Reflection]] — async job that deduplicates, merges, and reorganizes memory stores from past session transcripts (Anthropic)
- [[agent-view-multi-agent-management|Agent View — Managing Multiple Claude Code Sessions from One Screen]] — `claude agents` TUI: dispatch, peek, attach, worktree isolation, supervisor process (Anthropic)
- [[sandcastle-afk-agent-orchestration|Sandcastle — AFK Software Factory]] — composable `sandcastle.run` primitive; planner/implementer/reviewer/merger pipeline in Docker sandboxes (Matt Pocock)
- [[graph-engineering-verification|Graph Engineering — Multi-Agent Graphs and the Verification Problem]] — nodes/edges, diamond and fan-in shapes; one broken node corrupts output; the judge model matters most (AI LABS)
- [[computer-browser-use-best-practices|Computer and Browser Use Best Practices]] — screenshot scaling, thinking effort tuning, prompt injection defense, rolling buffer + compaction for CU sessions (Anthropic)
- [[browser-mcp-visual-feedback|Browser MCP — Visual Feedback Loops]] — Browser MCP servers (Chrome DevTools, Playwriter, Dev Browser) give agents visual feedback for frontend QA (Matt Pocock)
- [[mcp-apps-interactive-ui|MCP Apps — Interactive UI Inside MCP Hosts]] — interactive HTML interfaces rendered in chat via sandboxed iframe; bidirectional data flow over MCP
- [[cache-diagnostics|Cache Diagnostics — Diagnosing Prompt Cache Misses]] — pass previous response ID to identify the first divergence point: model, system, tools, or messages (Anthropic)
- [[excalidraw-plugin-external-edit-gotcha|Excalidraw Plugin — External-Edit Merge Gotcha]] — open Excalidraw tabs merge cached scenes over external rewrites; close tab, verify, prefer new filenames

## Related Topics

- [[../harness-engineering/index|Harness Engineering]] — the design layer above this substrate: long-running patterns, skills, hooks, instruction methods
- [[../claude-code-practice/index|Claude Code Practice]] — the client-side workflows that drive these runtimes
- [[../agent-architecture/index|Agent Architecture]] — 12-factor principles for the agents that run on this infrastructure
- [[../ai-dev-tools/index|AI Dev Tools]] — IDE-level features that complement this tooling surface

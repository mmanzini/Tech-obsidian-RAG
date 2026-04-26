# MCP Apps — Interactive UI Inside MCP Hosts

**Source:** https://modelcontextprotocol.io/extensions/apps/overview
**Date captured:** 2026-04-25

---

## What MCP Apps Are

MCP Apps let MCP servers return interactive HTML interfaces (data visualisations, forms, dashboards) that render directly inside the chat. They extend traditional MCP tools (which return text/images/structured data) by allowing tools to declare a reference to an interactive UI in their tool description.

## Why Not Just Build a Web App?

| Property | Standalone web app | MCP App |
|---|---|---|
| Context | Breaks conversation flow | Lives inside the conversation |
| Data flow | Needs own API + auth + state | Bidirectional via existing MCP patterns |
| Tool integrations | Must implement and maintain separately | Delegates to host's connected capabilities |
| Security | Developer-managed | Host-controlled sandboxed iframe |

## How It Works

1. **UI preloading** — tool description includes `_meta.ui.resourceUri` pointing to a `ui://` resource; host can preload before tool is called
2. **Resource fetch** — host fetches HTML page from server (often self-contained with JS + CSS)
3. **Sandboxed rendering** — HTML rendered inside a sandboxed iframe; restricts access to parent DOM, cookies, local storage, navigation
4. **Bidirectional communication** — JSON-RPC protocol (a dialect of MCP); transport is `postMessage` instead of stdio or HTTP
   - App can call tools, send messages, update model context, receive data from host
   - Shared methods with core MCP: `tools/call`
   - App-specific: `ui/initialize`, and `ui/` prefixed notifications

## Security Model

- Sandboxed iframe: cannot access parent window's DOM, cookies, local storage, or execute scripts in parent context
- All communication via `postMessage` API
- Host controls which tools and capabilities the app can access
- `_meta.ui.permissions` for requesting additional capabilities (microphone, camera); `_meta.ui.csp` to control external origins

## When to Use MCP Apps

- **Complex data exploration** — interactive maps, drill-down charts instead of text lists
- **Multi-option configuration** — forms with validation and interdependent choices (e.g., deployment setup)
- **Rich media viewing** — PDF viewers, 3D models, image previews embedded in conversation
- **Real-time monitoring** — persistent-connection dashboards with live data
- **Multi-step workflows** — expense approvals, code review triage with stateful navigation

## Client Support

Currently supported by: Claude, Claude Desktop, VS Code GitHub Copilot, Goose, Postman, MCPJam.

## Framework Support

- **`@modelcontextprotocol/ext-apps`** — `App` class as convenience wrapper (not required)
- Direct `postMessage` protocol implementation always valid
- Framework starters: React, Vue, Svelte, Preact, Solid, vanilla JS
- Host integration: `@mcp-ui/client` (React components) or **App Bridge** SDK module (handles iframe rendering, message passing, tool call proxying, security policy)

## Example Use Cases from the Repository

- 3D / visualisation: CesiumJS globe, Three.js scenes, shader effects
- Data exploration: cohort heatmaps, customer segmentation, wiki explorer
- Business: scenario modeller, budget allocator
- Media: PDF viewer, video player, sheet music, text-to-speech
- Utilities: QR code, system monitor, speech-to-text transcript

## Key Takeaways

- MCP Apps close the gap between conversational AI and interactive tooling without leaving the chat context
- Security is strong by design (sandboxed iframe + host-controlled permissions) — hosts can safely render third-party apps
- Bidirectional data flow via existing MCP patterns means no additional auth or state infrastructure
- The `ui://` resource pattern + `_meta.ui.resourceUri` in tool descriptions is the minimal integration surface
- Real-time monitoring and multi-step stateful workflows are the highest-value use cases vs. plain text responses

## Related

- [[harness-engineering/_index|Harness Engineering]] — MCP server configuration and integration
- [[browser-mcp-visual-feedback|Browser MCP — Visual Feedback Loops]] — browser-based visual feedback vs. MCP App embedded UI
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — MCP server setup in the harness

# Glossary

Key terms used in the Design in AI topic.

**AGENTS.md** — Repository-level markdown file providing cross-tool context for AI coding agents (build commands, test procedures, code style). Stewarded by the Agentic AI Foundation (Linux Foundation).

**Atomic Design** — Brad Frost's methodology: atoms → molecules → organisms → templates → pages. Predates Design.md; influences its component hierarchy thinking.

**Brownfield** — Existing codebase being modified. Design must anchor to current state before proposing changes. Contrast with greenfield.

**Design.md** — Plain-text markdown file encoding a design system (colours, typography, spacing, components, guardrails) for AI agent consumption. Originated from [[google-stitch|Google Stitch]]. See [[what-is-design-md]].

**Design Tokens** — Named values representing design decisions. W3C published a stable JSON spec (October 2025). Design.md encodes similar information in markdown. See [[historical-context-design-tokens]].

**Greybox Module** — Deep module where the human designs the interface and the AI controls the implementation. Tests lock down behaviour at the interface boundary. See [[deep-modules-codebase-for-ai]].

**Guardrails** — Explicit "do's and don'ts" in Design.md preventing contextually wrong choices. See [[writing-effective-design-docs]].

**Kiro** — AWS's AI development IDE structuring projects around requirements.md, design.md and tasks.md. Most mature SDD platform. See [[kiro-specs]].

**Living Architecture** — Structured architecture documentation template (10 core sections, 3 depth levels) using Mermaid C4 diagrams.

**MCP (Model Context Protocol)** — Standardised protocol for AI tool integration with external services. Used by Stitch, Figma, Penpot for agent connectivity.

**OpenSpec** — Lightweight, open-source SDD framework (20+ tools). Includes delta markers for brownfield projects.

**Progressive Disclosure** — Providing agents high-level information first, then allowing drill-down into details. Reduces context bloat. See [[deep-modules-codebase-for-ai]].

**RPI (Research Plan Implement)** — Lightweight agentic workflow. See [[research-plan-implement]].

**SDD (Spec-Driven Development)** — Tool-agnostic standard treating the specification as the durable artefact. See [[sdd-overview]].

**SKILL.md** — Markdown file with YAML frontmatter encoding reusable procedures with progressive disclosure.

**Spec Kit** — GitHub's open-source SDD toolkit. `/specify`, `/plan`, `/tasks`, `/implement` commands.

**Stitch** — Google Labs' AI-native design canvas. See [[google-stitch]].

**Style Dictionary** — Amazon's open-source build system for design tokens. Transforms definitions into platform-specific outputs.

**TypeUI** — Open-source CLI + registry of design skills for AI agents. Generates Design.md via interactive questionnaire.

**Vibe Coding** — Conversational, exploratory prompting with minimal planning. Fast for prototyping; fragile for production.

**W3C Design Tokens** — Vendor-neutral JSON format for design token interchange (stable 2025.10). See [[historical-context-design-tokens]].

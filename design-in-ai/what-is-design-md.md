# What is Design.md

A plain-text markdown file encoding an entire design system for AI coding agent consumption. Tagline: "README.md but for design systems."

## Core Purpose

- AI agents default to generic Tailwind components without design context
- Design.md provides the same constraints a human designer would — colours, fonts, spacing, component patterns, guardrails
- No special tooling, no build system, no configuration — just a markdown file in your repository

## Origins

- Introduced by Google as part of [[google-stitch|Stitch]] (2025-2026)
- Gained broader momentum through open-source: [VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md) (66+ brands) and [TypeUI](https://www.typeui.sh/) (50+ design skills)

## Structure

A well-formed Design.md contains:

1. **Visual Theme and Atmosphere** — Design philosophy, mood, density, aesthetic direction
2. **Colour Palette and Roles** — Semantic colour names, hex values and functional roles (`Primary: #1A73E8`, not "a trustworthy blue")
3. **Typography Rules** — Font families, type scale, weights, line heights
4. **Spacing** — Base unit (e.g. 8px), common intervals, breakpoints
5. **Component Styling** — Buttons, cards, inputs with all interactive states (hover, active, disabled, focus)
6. **Layout Principles** — Grid system, whitespace philosophy, responsive breakpoints
7. **Guardrails** — Explicit do's and don'ts ("Never use drop shadows on cards", "Never mix more than two typefaces")

### Format Characteristics

- Pure markdown, human-readable and machine-parseable
- Version-controllable (lives in Git alongside code)
- No proprietary tooling required

### Authoring Approaches

| Approach | Time | Method |
|----------|------|--------|
| Auto-generated | ~2 min | Stitch extracts from URL or image |
| Template-based | ~15 min | TypeUI CLI questionnaire |
| Manual | 30+ min | Write from scratch using templates |

## How It Works

```
Prompt + Design.md context → AI Agent → Consistent, on-brand UI code
```

The full Design.md is passed as context alongside generation requests. The model uses specific values (hex colours, font sizes, spacing tokens) instead of making arbitrary choices.

Key insight: constrained AI produces more consistent output than unconstrained AI.

## Ecosystem

### Tools Supporting Design.md

- **[Google Stitch](https://stitch.withgoogle.com)** — Originating platform, native Gemini integration
- **[TypeUI](https://www.typeui.sh/)** — Open-source CLI + registry of 50+ design skills
- **[VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md)** — 66+ real-world Design.md files
- **Claude Code, Cursor, Gemini CLI, Codex** — Read Design.md as context
- **[DesignMD.ai](https://designmd.ai/)** — Generative Design.md creation tool
- **Figma Plugin** — "Design MD Skills" plugin for export workflows

### Relationship to Other Agent Files

| File | Purpose |
|------|---------|
| **DESIGN.md** | Visual design system (colours, typography, spacing, components) |
| **AGENTS.md** | Execution guide (project conventions, code style, build commands) |
| **REQUIREMENTS.md** | Product specification (user stories, business rules) |
| **TASKS.md** | Work breakdown (specific tasks for agents) |
| **SKILL.md** | Reusable procedures (step-by-step guides with progressive disclosure) |

See [[adopting-design-md|Adopting Design.md]] for integration patterns.

## Limitations

- **No formal schema** — No published specification independent of Google Stitch
- **Visual tokens only** — Cannot encode [[interaction-design-gap|interaction design]] (states, animations, validation)
- **Accessibility gaps** — Cannot represent WCAG constraints beyond colour values (see [[accessibility-and-ai-design]])
- **Consistency is partial** — LLMs may still make unexpected choices within constraints
- **Figma sync is manual** — No automated bidirectional sync
- **Governance unclear** — No established team ownership or change review model

## What Design.md Does Not Replace

- User research and discovery
- Information architecture
- Interaction design and prototyping
- Accessibility auditing
- Usability testing
- Professional design review

It replaces the specific step of "communicating design tokens to an AI agent" — nothing more.

## Key Takeaways

- Design.md is a portable, tool-agnostic convention — not Stitch-specific
- Format optimised for LLM consumption (34-38% more token-efficient than JSON equivalents)
- Three authoring paths: auto-generate (~2 min), CLI template (~15 min), manual (~30+ min)
- Complementary to W3C Design Tokens (markdown for LLMs vs JSON for build systems)
- Does not replace design process — only the communication step between designer and AI agent

## Sources

- [Google Stitch Design.md Overview](https://stitch.withgoogle.com/docs/design-md/overview/)
- [MindStudio: What is Design.md?](https://www.mindstudio.ai/blog/what-is-google-stitch-design-md-file)
- [VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md)
- [TypeUI Official](https://www.typeui.sh/)
- [DesignMD.ai](https://designmd.ai/what-is-design-md)

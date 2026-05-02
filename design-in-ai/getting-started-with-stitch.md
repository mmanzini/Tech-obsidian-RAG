# Getting Started with Google Stitch and Design.md

**Source:** [Getting Started with Stitch.md](../../../Resources/documents/frameworks/Design in AI/Guides/Getting Started with Stitch.md)

---

## Summary

A practical hands-on guide to using Google Stitch and the Design.md convention. Covers first-generation in five minutes, extracting a Design.md from an existing site, using Design.md with Stitch and other tools (Claude Code, Cursor, Gemini CLI), Figma export, generation quotas, and honest expectations about what Stitch is and is not good at.

## First Generation (5 Minutes)

1. Navigate to [stitch.withgoogle.com](https://stitch.withgoogle.com) and sign in with a Google account — no waitlist or enterprise plan required (source: Getting Started with Stitch.md).
2. Type a prompt describing the interface you want. Be specific about layout, components, and visual style.
3. Wait approximately 45 seconds for generation.
4. Review the output. If it is close but not right, refine the prompt and regenerate.

**Standard Mode** (Gemini 2.5 Flash) gives 350 generations/month and supports Figma export. **Experimental Mode** (Gemini 2.5 Pro) gives 50 generations/month with higher quality but no Figma export (source: Getting Started with Stitch.md).

## Creating a Design.md

The fastest path is to extract one from an existing website (source: Getting Started with Stitch.md):

1. In Stitch, use the URL extraction feature — paste the URL of a site whose design system you want to capture.
2. Stitch analyses the site and generates a Design.md with colours, typography, spacing, and component tokens.
3. Download or copy the Design.md.
4. Review and customise: remove tokens you do not need, add guardrails specific to your project.

Alternatively, [TypeUI](https://www.typeui.sh/) (`npx typeui`) generates a Design.md through an interactive CLI in approximately 15 minutes. The CLI asks about product basics, visual style, typography preferences, colours, spacing, components, and accessibility (source: Getting Started with Stitch.md).

## Using Design.md with Stitch

Upload or paste your Design.md into Stitch's design system panel. All subsequent generations will use your specific hex values, fonts, and spacing tokens. The key insight: encode the design system once; every subsequent generation inherits those constraints without re-specifying them (source: Getting Started with Stitch.md).

## Using Design.md with Other Tools

Design.md is not Stitch-specific. It works in any AI coding workflow (source: Getting Started with Stitch.md):

- **Claude Code** — include Design.md in the project root or pass it via MCP.
- **Cursor** — place Design.md in the project root or reference it in `.cursor/rules/`.
- **Gemini CLI** — native support through Google's ecosystem.

## Export to Figma

In Standard Mode, click "Copy to Figma" to export the generated design. The export preserves component hierarchy, layers, and basic styling. From Figma you can refine spacing and alignment, add interaction prototypes, build responsive variants, apply accessibility attributes, and share with the design team (source: Getting Started with Stitch.md).

## Generation Quotas

| Mode | Model | Generations/Month |
|------|-------|-------------------|
| Standard | Gemini 2.5 Flash | 350 |
| Experimental | Gemini 2.5 Pro | 50 |

Stitch is free during the experimental phase; no pricing has been announced for post-beta (source: Getting Started with Stitch.md).

## Honest Expectations

**Stitch is good at:** rapid ideation, variant exploration, quick prototyping of multi-screen flows, establishing visual direction (source: Getting Started with Stitch.md).

**Stitch is not good at:** responsive layouts, accessible output, pixel-perfect control, interaction design, collaboration, production-ready code — see [[google-stitch|Google Stitch Overview]] for full limitations.

## Key Takeaways

- No account friction: any Google account works; generation takes ~45 seconds.
- The fastest Design.md source is URL extraction from an existing site.
- Design.md is tool-agnostic; the same file works in Claude Code, Cursor, and Gemini CLI.
- Standard mode is the default; switch to Experimental only when output quality matters more than quota.
- Stitch is a prototyping and ideation tool, not a production design system.

## Related

- [[google-stitch|Google Stitch Overview]] — capabilities, limitations, and MCP integration in depth
- [[what-is-design-md|What is Design.md]] — format definition, structure, and ecosystem
- [[integrating-design-with-specs|Integrating Design with Specs]] — how Design.md fits alongside other spec files in a workflow
- [[design-principles|Design Principles for AI-Assisted Development]] — the reasoning behind maintaining a Design.md

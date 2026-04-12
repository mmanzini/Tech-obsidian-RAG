# Writing Effective Design Docs

Practical guidance for authoring design documents that AI agents consume reliably.

## Core Principles

- **Be specific, not descriptive** — Use `Primary: #1A73E8`, not "a blue that feels trustworthy". Agents apply values mechanically; vague descriptions produce inconsistent results
- **Use semantic naming** — Name colours by intent (Primary, Error, Success), not appearance (Light Blue, Red)
- **Structure for parsing** — Use headers, bullet points and consistent formatting. Dense paragraphs are harder for models to parse
- **Lists over ranges** — Discrete values reduce hallucination. "Spacing scale: 4, 8, 12, 16, 24, 32, 48" beats "spacing between 8px and 32px"
- **Include negative constraints** — What the agent should *not* do matters as much as what it should

## Recommended Structure

1. **Visual Theme and Atmosphere** — 2-3 sentences on mood, density, aesthetic
2. **Colour Palette** — Every colour with hex value and semantic role
3. **Typography** — Font families, full type scale, weights, line heights
4. **Spacing** — Base unit and scale (e.g. base 4px: 4, 8, 12, 16, 24, 32, 48, 64)
5. **Component Patterns** — Key components with all states (hover, active, disabled, focus)
6. **Guardrails** — Explicit anti-patterns ("Never use more than 2 typefaces", "Never create colour variants not in this palette")

## Accessibility Section

Include at minimum:
- Minimum text contrast ratio: 4.5:1 (WCAG AA)
- Minimum large text contrast ratio: 3:1
- Minimum touch target: 44x44px
- Font size minimum for body text
- Semantic HTML preferences

These are not enforced by the format itself but providing them as context improves the likelihood of accessible output. See [[accessibility-and-ai-design]] for detail.

## Common Mistakes

- **Too vague** — "Use a modern, clean aesthetic" gives the model nothing actionable
- **Too verbose** — Long brand philosophy narratives waste context tokens. Include only what agents need to apply mechanically
- **Missing states** — Documenting a button without hover/active/disabled guarantees inconsistency
- **No guardrails** — Without negative constraints, agents make contextually wrong but technically sound choices
- **Ignoring accessibility** — At minimum include contrast targets and font size floors

## Key Takeaways

- Design.md reads like a reference table, not a narrative essay
- Specific values > descriptive language (hex codes, px values, concrete scales)
- Guardrails (explicit don'ts) are as important as positive specifications
- Always document component states — a button without hover/disabled states is incomplete
- Include accessibility constraints even though enforcement is inconsistent

## Sources

- [O'Reilly: How to Write a Good Spec for AI Agents](https://www.oreilly.com/radar/how-to-write-a-good-spec-for-ai-agents/)
- [Anthropic: Effective Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md)

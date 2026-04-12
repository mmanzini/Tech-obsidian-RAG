# Interaction Design Gap

Design.md handles visual tokens effectively. It cannot represent interaction design — the behaviour, states, transitions and logic that make interfaces functional.

## What Design.md Cannot Encode

- **State machines and conditional logic** — A login form has states (empty, validating, error, success). Design.md describes what the error state *looks like* but not *when* it triggers or *how* states transition
- **Animation and motion** — Timing functions, easing curves, durations, enter/exit transitions require visual tools or code. Markdown cannot represent motion
- **Validation rules** — "Email must be valid format" or "password 8+ characters" are interaction requirements, not visual tokens
- **Event handlers and triggers** — Click, hover, focus, blur, scroll, gesture — live in code, not documentation
- **Responsive behaviour changes** — A sidebar collapsing into a hamburger at 768px involves layout, animation and state management simultaneously

## Why This Matters

A button component is not a design. A button that validates input, shows a loading spinner, disables on submit, displays success/error states and transitions smoothly between them *is* a design. Design.md handles each state's appearance but not the logic connecting them.

Design.md is necessary but not sufficient for production UI work.

## Supplements Required

- **Feature-level design documents** — [[kiro-specs|Kiro]]-style `design.md` with sequence diagrams and data flow
- **BDD-style scenarios** — "WHEN user clicks submit AND form is invalid THEN show error state"
- **Figma prototypes** — Interaction flows, state transitions for complex interactions
- **Component Storybook** — Documented interactive states (hover, focus, disabled, loading, error, success)

## Current Market Split

| Phase | Tools | Design.md Coverage |
|-------|-------|--------------------|
| **Visual design** | Figma, Penpot, Framer | Yes — this is Design.md's layer |
| **Interaction design** | ProtoPie, Framer motion, code | No — least standardised and least automated |
| **Code implementation** | React, Vue, Angular | No — agents operate here |

The interaction design layer between visual tokens and code implementation is the gap.

## Emerging Approaches

- **Penpot UX2Doc** — Generates technical/functional UI/UX specs from designs (proprietary format)
- **MCP-based design reading** — Penpot and Figma MCP integrations let agents read design context directly, sidestepping the markdown spec problem
- **BDD scenarios in specs** — Teams embedding interaction specifications as WHEN/THEN scenarios in requirement documents

## Key Takeaways

- Design.md covers the visual layer only — do not expect it to solve interaction design
- Supplement with feature-level specs (Kiro/SDD), BDD scenarios and Storybook for interactive states
- The interaction design layer is the least standardised part of the AI-assisted workflow
- MCP integrations (Figma, Penpot) may eventually bridge this gap by giving agents direct access to design tool context
- Accept the boundary and plan your toolchain accordingly

## Sources

- [IxDF: UI Animation](https://ixdf.org/literature/topics/ui-animation)
- [Microsoft Fluent 2: Motion](https://fluent2.microsoft.design/motion)
- [Penpot AI Whitepaper](https://penpot.app/blog/penpot-ai-whitepaper/)
- [Figma Make](https://www.figma.com/make/)

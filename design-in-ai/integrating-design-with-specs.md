# Integrating Design with Specs — The Multi-File Ecosystem

**Source:** [Integrating Design with Specs.md](../../../Resources/documents/frameworks/Design in AI/Guides/Integrating Design with Specs.md)

---

## Summary

How Design.md fits alongside REQUIREMENTS.md, TASKS.md, AGENTS.md, and other specification artefacts in a multi-file AI-assisted workflow. Covers the three progressive levels of integration (repository baseline, feature-level specs, full SDD), conflict resolution order, and how Design.md complements existing design infrastructure (Figma, Tailwind, Storybook, W3C Design Tokens).

## The Multi-File Ecosystem

Each spec file answers a different question (source: Integrating Design with Specs.md):

| File | Question | Scope |
|------|----------|-------|
| **DESIGN.md** | How should it look? | Visual system: colours, typography, spacing, components, guardrails |
| **AGENTS.md** | How should we build it? | Project conventions: build commands, test procedures, code style, architectural constraints |
| **REQUIREMENTS.md** | What should we build? | Product spec: user stories, acceptance criteria, business rules |
| **TASKS.md** | What work items exist? | Implementation breakdown: discrete, trackable tasks |
| **SKILL.md** | What reusable procedures exist? | Step-by-step how-tos with progressive disclosure |
| **CLAUDE.md / .cursorrules** | Tool-specific context | IDE or agent-specific configuration and rules |

## Three Levels of Integration

### Level 1: Repository Baseline

Place DESIGN.md and AGENTS.md at the project root. Both are read automatically by most AI agents for every task. DESIGN.md handles visual consistency; AGENTS.md handles code quality. Together they cover the two most common sources of AI output drift (source: Integrating Design with Specs.md).

### Level 2: Feature-Level Specs

For each significant feature, create a spec directory with requirements, design, and tasks. The feature-level `design.md` (lowercase) is different from the root-level `DESIGN.md` (uppercase): the root file defines the visual system; the feature-level file defines technical architecture for a specific feature (source: Integrating Design with Specs.md).

### Level 3: Full SDD Workflow

Add boundary declarations (`BOUNDARIES.md`), validation gates, and phase enforcement. Phase transitions (requirements → design → tasks → implementation → validation) are enforced by team discipline, IDE tooling (Kiro), or deterministic workflow engines (source: Integrating Design with Specs.md).

## Conflict Resolution

When files contradict each other, the resolution order is (source: Integrating Design with Specs.md):

1. **Requirements** (what to build) take precedence over design (how to build).
2. **Feature-level specs** take precedence over global baselines.
3. **DESIGN.md** (visual system) takes precedence over agent defaults.
4. **AGENTS.md** (code conventions) takes precedence over model assumptions.

If a conflict is discovered during implementation, the correct response is to update the specs — not to silently override them in code.

## Integration with Existing Design Infrastructure

Design.md is a documentation-first format that complements rather than replaces existing design infrastructure (source: Integrating Design with Specs.md):

- **Figma → Design.md.** Extract visual tokens from Figma manually or via plugins. Design.md serves as the agent-readable representation of the same tokens. No automated bidirectional sync exists yet — this is a known gap.
- **Tailwind → Design.md.** `tailwind.config.js` defines tokens in JavaScript; Design.md documents them in markdown for agent consumption. A build script could generate one from the other.
- **Storybook → Design.md.** Storybook documents components with interactive examples for developers; Design.md documents the design system for agents. They serve different audiences.
- **W3C Design Tokens → Design.md.** The W3C spec uses JSON for machine interoperability; Design.md uses markdown for LLM consumption. Both encode similar information for different consumers. See [[historical-context-design-tokens|Historical Context — Design Tokens]] for detail.

## Traceability

When Design.md and spec files are version-controlled, you get a traceability chain (source: Integrating Design with Specs.md):

```
Business requirement → Feature requirement → Technical design → Task → Code commit
```

Six months later, "Why did we use Redis instead of SQS?" can be traced from code commit → task ID → design document → architecture rationale. This matters for compliance, audits, and onboarding.

## Key Takeaways

- Start with just DESIGN.md + AGENTS.md at the root (Level 1); add feature-level specs as complexity grows.
- Root DESIGN.md sets the visual system; feature-level design.md sets technical architecture.
- When files conflict, requirements beat design, and feature specs beat global baselines.
- Design.md complements Figma, Tailwind, and Storybook — it doesn't replace them.
- Version-controlled specs create a traceability chain from business requirement to code commit.

## Related

- [[design-first-vs-requirements-first|Design-First vs Requirements-First]] — choosing where to start in the spec workflow
- [[writing-effective-design-docs|Writing Effective Design Docs]] — authoring guidance for the DESIGN.md file specifically
- [[design-principles|Design Principles for AI-Assisted Development]] — underlying principles for why specs matter
- [[historical-context-design-tokens|Historical Context — Design Tokens]] — W3C spec lineage and how Design.md fits in

# Adopting Design.md

How to move from unstructured AI-assisted design toward structured design documents. The transition is incremental.

## The Adoption Spectrum

| Level | What | Overhead | Outcome |
|-------|------|----------|---------|
| **0** | No design context | None | Visual inconsistency is the norm |
| **1** | Design.md only | ~15 min setup | Immediate consistency improvement |
| **2** | Design.md + guardrails | ~30 min more | Reduces most common visual drift |
| **3** | Full spec workflow | Hours per feature | Design is a reviewable phase before implementation |

Most teams should start at Level 1 and graduate as the pain of skipping a level becomes obvious.

## Getting Started

### Creating Your First Design.md

- **Auto-generate from URL (~2 min)** — Stitch or DesignMD.ai extracts tokens from an existing website
- **Generate via CLI (~15 min)** — `npx typeui` from [TypeUI](https://www.typeui.sh/), answer interactive questionnaire
- **Manual authoring (~30+ min)** — Start from a VoltAgent template, replace with your tokens (see [[writing-effective-design-docs]])

### Placement

Put `DESIGN.md` at the project root. Most AI agents (Claude Code, Cursor, Gemini CLI) discover and read it automatically.

For teams using multiple agent-guidance files:
```
project-root/
├── DESIGN.md          # Visual design system
├── AGENTS.md          # Build/test/code conventions
├── README.md          # Project overview
├── requirements/      # Specs (if using SDD)
```

### Testing

Generate a simple UI component ("Create a login form"). Check output uses your specified colours, fonts and spacing. If drift occurs, refine the Design.md: add more specific hex values, component patterns with all states, guardrails for observed drift.

### Version Control

Design.md lives in Git alongside code — changes go through PRs. This gives audit trail, reviewable design changes and rollback capability.

## Integrating Design.md with Spec Workflows

### The Multi-File Ecosystem

| File | Question |
|------|----------|
| **DESIGN.md** | How should it look? (Visual system) |
| **AGENTS.md** | How should we build it? (Code conventions) |
| **REQUIREMENTS.md** | What should we build? (Product spec) |
| **TASKS.md** | What work items exist? (Implementation breakdown) |
| **SKILL.md** | What reusable procedures exist? |

### Level 2: Feature-Level Specs

For significant features, create spec directories. Note: root `DESIGN.md` (uppercase, visual system) differs from feature-level `design.md` (lowercase, technical architecture per Kiro/SDD convention).

### Conflict Resolution

1. **Requirements** take precedence over design
2. **Feature-level specs** take precedence over global baselines
3. **DESIGN.md** takes precedence over agent defaults
4. **AGENTS.md** takes precedence over model assumptions

If a conflict is discovered during implementation, update the specs — not the code.

### Integration with Existing Infrastructure

- **Figma → Design.md** — Extract visual tokens (manual or via plugins). No automated bidirectional sync yet
- **Tailwind → Design.md** — `tailwind.config.js` defines tokens in JS; Design.md documents the same in markdown. A build script could generate one from the other
- **W3C Design Tokens → Design.md** — JSON for machine interoperability vs markdown for LLM consumption. Complementary formats (see [[historical-context-design-tokens]])

## The Hybrid Approach

The emerging best practice is "structured exploration with living specs":
- Use vibe coding for discovery and prototyping (1-4 weeks)
- Formalise discovered insights into Design.md and specs
- Execute production implementation against specifications
- Keep Design.md as living documentation

The "vibe coding vs SDD" framing is a false dichotomy — teams use both at different lifecycle phases.

## Key Takeaways

- Start at Level 1 (just a Design.md file); graduate when the pain requires it
- Design.md is a communication layer between designer and AI agent, not a design process replacement
- Version-control your Design.md like code for audit trail and reviewability
- Triple-maintenance (Figma + tokens + Design.md) is the main practical challenge
- Traceability chain: business requirement → feature requirement → technical design → task → code commit

## Sources

- [Augment Code: Vibe Coding vs Spec-Driven Development](https://www.augmentcode.com/guides/vibe-coding-vs-spec-driven-development)
- [GitHub Blog: Spec-Driven Development with AI](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [TypeUI Official](https://www.typeui.sh/)
- [GitHub Blog: How to Write a Great AGENTS.md](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/)

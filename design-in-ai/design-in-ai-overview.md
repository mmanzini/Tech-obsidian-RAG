# Design in AI — Overview

Design in AI is the practice of encoding visual and architectural design intent into structured, version-controlled artefacts that AI coding agents can read and apply during code generation.

## The Problem

- AI agents produce functional code but generate generic-looking interfaces
- Without explicit design context, every AI-generated app converges on the same default Tailwind components, spacing, colour palette
- Design intent traditionally lives in Figma files, Slack conversations and verbal reviews — none machine-readable
- When an agent lands cold on a codebase, it has no access to any of this

## The Solution: Design as a Specification

- **Design.md** is a plain-text markdown file capturing a complete design system in a format AI agents understand natively
- Encodes colours, typography, spacing, component patterns and guardrails
- When a prompt is submitted, the full Design.md is passed as context, constraining the model to use specific values instead of making arbitrary choices
- Part of a broader movement toward [[sdd-overview|specification-driven development]] where the spec — not the code, not the chat — is the durable artefact

## Two Pillars

### Google Stitch

- [[google-stitch|Stitch]] is Google Labs' AI-native design canvas converting natural language prompts, sketches and screenshots into high-fidelity UIs using Gemini 2.5
- Core innovation: native Design.md support — encode your design system once, then every generation inherits those constraints
- Best understood as a pre-Figma ideation tool (45 seconds per screen, 3 minutes for a 5-screen flow)
- Lacks collaboration, responsive design and accessible output by default

### The Design.md Convention

- [[what-is-design-md|Design.md]] is not Stitch-specific — works across Claude Code, Cursor, Gemini CLI, Codex and other agents
- The [VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md) repository curates 66+ design systems (Stripe, Figma, Spotify, Apple)
- Sits alongside other agent-guidance files:
  - **DESIGN.md** → "How should it look?"
  - **AGENTS.md** → "How should we build it?"
  - **REQUIREMENTS.md** → "What should we build?"
  - **SKILL.md** → "What reusable procedures do we have?"

## Where Design Fits in AI Workflows

- In [[design-first-vs-requirements-first|spec-driven workflows]] (SDD, Kiro, RPI), design is a discrete, reviewable phase — not an implicit detail buried in implementation
- The design document is produced after requirements and before implementation
- It is reviewed before any code is written
- If reality disagrees with the spec, the spec gets updated first — drift from the design must trigger a return to the design phase

## Current Limitations

- **Accessibility failures** — AI-generated UIs consistently fail WCAG for colour contrast, touch targets and semantic HTML (see [[accessibility-and-ai-design]])
- **Interaction design** — Design.md handles visual tokens but cannot represent state machines, animations, validation logic or responsive breakpoints (see [[interaction-design-gap]])
- **Consistency partially unsolved** — LLMs may still make unexpected choices within constraints
- **No formal standard** — No published schema, governance model or versioning spec independent of Google Stitch
- **Enterprise adoption unvalidated** — Virtually all evidence comes from startups and individual developers

## What Is New and What Is Old

- Design.md is not the first design-as-code attempt (see [[historical-context-design-tokens]])
- Lineage: living style guides (2010s) → Pattern Lab (Atomic Design) → design tokens (Salesforce 2017) → W3C Design Tokens (stable October 2025) → Design.md
- W3C tokens standardise *what* design decisions are (JSON format, strict syntax)
- Design.md standardises the *narrative* — values plus rationale and guardrails, in markdown optimised for LLM consumption

## Key Takeaways

- Design.md solves "how do I communicate design constraints to an AI agent" — nothing more, nothing less
- Constrained AI produces more consistent output than unconstrained AI
- Design.md is necessary but not sufficient — supplement with feature-level specs, BDD scenarios and professional design review
- The format is promising but unproven at enterprise scale; success depends on organisational adoption, not format quality
- Design.md and W3C Design Tokens are complementary (markdown for LLMs, JSON for build systems)

## Sources

- [Google Stitch Documentation](https://stitch.withgoogle.com/docs/learn/overview/)
- [VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md)
- [OSS Insight: Design.md Protocol 2026](https://ossinsight.io/blog/design-md-protocol-2026)
- [MindStudio: What is Google Stitch's Design.md File?](https://www.mindstudio.ai/blog/what-is-google-stitch-design-md-file)
- [Fifty Five and Five: Design.md and AI-Assisted Design Patterns](https://fiftyfiveandfive.com/resources/design-md-files-and-the-foundational-patterns-of-ai-assisted-design/)

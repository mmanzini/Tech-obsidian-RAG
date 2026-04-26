# RPI Agentic Harness Methodology

**Source:** RPI Agentic Harness Methodology v0.1 (April 2026) — a structured, critically-examined reference for the Research-Plan-Implement framework originally developed by HumanLayer (Dexter Horthy, 2024), adopted by Block's Goose and Kilo.ai, and evolved into CRISPY/QRSPI.

This topic is the **deep reference** for RPI. For the one-page overview see [[rpi-methodology/research-plan-implement|Research-Plan-Implement]] in agent-workflows.

## Standard

- [[principles|Principles]] — Phase separation, fresh contexts, validation gates as failure-prevention (with honest "advocated not validated" caveat)
- [[workflow|Workflow]] — The three-phase lifecycle in detail: research sub-agents, plan structure, implementation loop, iteration, `thoughts/` directory
- [[validation-gates|Validation Gates — FAR & FACTS]] — Research and plan quality scales with critical assessment of their limits
- [[context-engineering|Context Engineering]] — Instruction budget, context rot, sub-agent firewalls, Anthropic's correctness → completeness → size hierarchy
- [[glossary|Glossary]] — Key terms: ACE, atomic task, CRISPY, context rot, instruction budget, Ralph loop, reverse prompting, workability
- [[tool-agnosticism|Tool Agnosticism]] — RPI across Goose (native), Claude Code, Cursor, Copilot, Kiro

## Guides

- [[getting-started|Getting Started]] — Minimal setup, naming convention, worked example (user-service logging)
- [[when-to-use|When to Use RPI]] — Decision tree, over-engineering risks, break-even analysis
- [[team-adoption|Adopting RPI in Teams]] — Cognitive load, plan-reading illusion, scaling to 3/10/30-person teams
- [[vs-other-frameworks|RPI vs Other Frameworks]] — Comparison with SDD, Kiro, Quick Dev, Vibe, Anthropic harness patterns

## Emerging Patterns

- [[post-rpi-evolution|Post-RPI Evolution]] — CRISPY/QRSPI, Ralph Loop, distributed RPI, instruction budget decomposition
- [[humanlayer-and-hitl|HumanLayer and Human-in-the-Loop]] — ACE framework, 12-factor agents, BAML case study, "no vibes allowed" philosophy, conflict-of-interest note
- [[industry-landscape|Industry Landscape]] — Anthropic, OpenAI, Google ADK, Microsoft, CrewAI, McKinsey QB, AWS Kiro positioning

## Related Topics

- [[agent-workflows/_index|Agent Workflows]] — lightweight workflow overviews (RPI, Quick Dev, Adversarial Review)
- [[harness-engineering/_index|Harness Engineering]] — runtime configuration RPI operates within
- [[spec-driven-development/_index|Spec-Driven Development]] — the alternative/complementary spec-first approach
- [[agent-architecture/_index|Agent Architecture]] — twelve-factor agents, foundational design principles

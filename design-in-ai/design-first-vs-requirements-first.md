---
type: synthesis
title: Design-First vs Requirements-First
description: Requirements-first (requirements → design → tasks → implementation) suits greenfield features where intent is clear but structure is open; design-first (design → requirements → tasks → implementation) suits refactors and infrastructure work where the target architecture is already known.
bundle: ai-engineering
topic: design-in-ai
tags:
- design-in-ai
- spec-driven-development
- agent-workflows
- harness-engineering
sources:
- id: synthesis-design-in-ai-corpus
  resource: synthesis — design-in-ai corpus
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-09T07:33:19Z'
status: stable
related:
- ai-engineering/design-in-ai/design-in-ai-overview.md
- ai-engineering/design-in-ai/ai-design-tool-comparison.md
- ai-engineering/design-in-ai/emerging-practices-design.md
- ai-engineering/design-in-ai/competing-approaches-design.md
- ai-engineering/design-in-ai/design-in-ai-examples.md
- ai-engineering/design-in-ai/design-in-ai-glossary.md
---

# Design-First vs Requirements-First

**Source:** (synthesis — design-in-ai corpus)

Two valid entry points for spec-driven feature work. The choice depends on what is known and what needs to be discovered.

---

## Summary

Requirements-first (requirements → design → tasks → implementation) suits greenfield features where intent is clear but structure is open; design-first (design → requirements → tasks → implementation) suits refactors and infrastructure work where the target architecture is already known. Regardless of entry point, the design document becomes an enforceable contract, and both approaches require explicit boundary declarations (Always/Ask/Never) as design decisions rather than afterthoughts.

## Requirements-First

**Flow:** Requirements → Design → Tasks → Implementation

**When to use:**
- Greenfield features where the problem is clear but the solution is open
- User-facing features where acceptance criteria drive the shape
- Teams where product owners define intent and architects design solutions
- When user research or discovery has already been completed

## Design-First

**Flow:** Design → Requirements → Tasks → Implementation

**When to use:**
- Refactors or migrations where the target architecture is known
- Platform work where technical constraints shape the user experience
- Infrastructure features where the design *is* the requirement
- Constrained environments (performance, security, regulatory)

## Both Are Valid

- Requirements-first clarifies *intent* before committing to *structure*
- Design-first documents *structure* when intent is already clear
- Most teams oscillate between both — the spec is a checkpoint, not a gate
- SDD explicitly states: "Phases are checkpoints, not gates" — loop back when you learn something

## Design as Enforceable Contract

- Regardless of entry point, the design document becomes the reference frame for implementation
- [[sdd-overview|SDD]] makes this explicit: "Drift from the spec must trigger a return to Specify or Design"
- If reality disagrees with the design, the spec is updated first
- [[mckinsey-agentic-workflows|McKinsey's agentic workflows]] operationalise this through rule-based workflow engines enforcing phase transitions

## Boundary Declarations

Both approaches require explicit boundary declarations as part of design intent:

- **Always** — Actions the agent may take without confirmation (run tests, format files)
- **Ask** — Actions requiring human confirmation (modify database schema, touch files outside scope)
- **Never** — Actions the agent must refuse (push to main, call paid APIs, delete production data)

These boundaries are per-spec, not global. Each design document encodes the trust envelope for the work it governs.

## Key Takeaways

- Neither approach is universally superior — they address different problem shapes
- The spec is a living checkpoint, not a one-time gate
- Design documents become enforceable contracts regardless of entry point
- Boundary declarations (Always/Ask/Never) are design decisions, not afterthoughts
- Confirmed by research in [[sdd-workflow|SDD Workflow]], [[kiro-specs|Kiro Specs]] and [[mckinsey-agentic-workflows|McKinsey Agentic Workflows]]

## Sources

- [[kiro-specs|Kiro Specs]] (Research Wiki)
- [[sdd-workflow|SDD Workflow]] (Research Wiki)
- [[sdd-roles-and-boundaries|SDD Roles and Boundaries]] (Research Wiki)
- [[mckinsey-agentic-workflows|McKinsey Agentic Workflows]] (Research Wiki)

## Related

- [[design-in-ai-overview|Design in AI — Overview]] — the foundational context for why design specs matter in AI workflows
- [[ai-design-tool-comparison|AI Design Tool Comparison]] — side-by-side evaluation of tools that support these two entry approaches
- [[emerging-practices-design|Emerging Practices in AI-Assisted Design]] — evolving community conventions on which entry point teams actually prefer
- [[competing-approaches-design|Competing Approaches to AI-Assisted Design]] — alternative frameworks that resolve the design-first vs requirements-first tension differently
- [[design-in-ai-examples|Design in AI — Examples]] — concrete worked examples of both entry routes
- [[design-in-ai-glossary|Design in AI — Glossary]] — definitions for spec terminology used in this article

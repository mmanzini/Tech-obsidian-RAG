---
type: synthesis
title: SDD Workflow
description: The SDD workflow moves through six phases — Brief, Specify, Design, Task, Implement, Validate — each producing a concrete artifact and ending with a human checkpoint, where looping back is the correct behaviour when reality diverges from the spec.
bundle: ai-engineering
topic: spec-driven-development
tags: [spec-driven-development, agent-workflows, agent-architecture]
source: synthesis — spec-driven-development corpus
resource:
timestamp: 2026-05-09T07:23:23Z
status: active
related:
  - ai-engineering/spec-driven-development/sdd-overview.md
  - ai-engineering/spec-driven-development/sdd-roles-and-boundaries.md
  - ai-engineering/ai-dev-tools/kiro-specs.md
  - ai-engineering/agent-workflows/research-plan-implement.md
  - ai-engineering/agent-workflows/quick-dev.md
---

# SDD Workflow

**Source:** (synthesis — spec-driven-development corpus)

The SDD workflow has **six phases**. Each phase produces a concrete artifact and ends with a checkpoint. Phases are not gates — looping back is normal and expected.

## Summary

The SDD workflow moves through six phases — Brief, Specify, Design, Task, Implement, Validate — each producing a concrete artifact and ending with a human checkpoint, where looping back is the correct behaviour when reality diverges from the spec. The anatomy of a compliant spec includes frontmatter, context, scope/non-goals, requirements, design summary, Always/Ask/Never boundaries, a task checklist, and a validation log — making it tool-agnostic and Kiro-compatible by design.

## The Six Phases

1. **Brief** — One page. The problem, the user, the constraint, the definition of success. Written by a human; the agent may help refine it.
2. **Specify** — Expand the brief into a structured spec: scope, non-goals, assumptions, requirements, acceptance criteria, boundaries. This is the artifact that survives.
3. **Design** — Produce a design document covering architecture, data model, key interfaces, and the changes to existing code. Reviewed before any implementation.
4. **Task** — Decompose the design into a checklist of small, independently verifiable tasks. Each task names its acceptance criterion.
5. **Implement** — The agent executes tasks one at a time, marking each complete. Drift from the spec must trigger a return to Specify or Design.
6. **Validate** — Run the acceptance criteria. Update the spec if reality disagreed with it. Archive the spec alongside the merged code.

## Anatomy of a Spec

A compliant spec contains, at minimum:

- **Frontmatter:** id, status, owner, related specs
- **Context:** the brief, restated; links to steering documents
- **Scope & Non-goals:** what's in, what's explicitly out
- **Requirements:** numbered, testable
- **Design summary:** the decisions and their rationale
- **Boundaries:** Always / Ask / Never (see [[sdd-roles-and-boundaries]])
- **Tasks:** a checklist with acceptance criteria
- **Validation log:** what was tested, what passed, what surprised you

The format is intentionally compatible with [[kiro-specs|Kiro Specs]] so the same file works across tools.

## Looping

If implementation reveals that a requirement was wrong, **stop and update the spec** before continuing to code. The spec is the source of truth; silently diverging from it is the failure mode SDD exists to prevent.

## Key Takeaways

- Six phases, each with a named artifact and a checkpoint.
- Specs include boundaries and a validation log, not just requirements.
- Looping back is the workflow working correctly, not a failure.
- The spec format is tool-agnostic and Kiro-compatible by design.

## Related

- [[sdd-overview]] — Why SDD exists and how to adopt it
- [[sdd-roles-and-boundaries]] — The boundary framework that lives inside every spec
- [[kiro-specs|Kiro Specs]] — Reference spec format
- [[research-plan-implement|Research Plan Implement (RPI)]] — Lighter sibling workflow with three phases
- [[quick-dev|Quick Dev]] — Even lighter "just ship it" alternative for low-stakes work

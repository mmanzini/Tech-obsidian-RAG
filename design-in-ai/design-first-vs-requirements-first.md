# Design-First vs Requirements-First

Two valid entry points for spec-driven feature work. The choice depends on what is known and what needs to be discovered.

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

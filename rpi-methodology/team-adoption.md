---
type: synthesis
title: Adopting RPI in Teams
description: Team adoption of RPI requires genuine artifact review (not rubber-stamping), clear governance (single approver, SLAs), and ceremony discipline — the plan-reading illusion, where a coherent 1,000-line plan masks technical assumptions it doesn't actually validate, is the silent failure mode that most undermines the methodology.
bundle: ai-engineering
topic: rpi-methodology
tags: [agent-workflows, ai-org-design, spec-driven-development, evals]
source: self-authored
resource:
timestamp: 2026-04-29T21:54:50Z
status: active
related: []
---

# Adopting RPI in Teams

**Source:** (self-authored)

## Summary

Team adoption of RPI requires genuine artifact review (not rubber-stamping), clear governance (single approver, SLAs), and ceremony discipline — the plan-reading illusion, where a coherent 1,000-line plan masks technical assumptions it doesn't actually validate, is the silent failure mode that most undermines the methodology. The article covers scaling playbooks for teams of 3–5, 6–15, and 20+, and HumanLayer's evolved approach of front-loading alignment through design discussions and structure outlines before producing detailed plans.

Individual adoption is easy. **Team adoption** introduces organisational friction, coordination challenges, and the **plan-reading illusion** that quietly breaks the methodology.

## The Cognitive Load Problem

RPI requires humans to review two artifacts before work begins:

1. **Research document** — verify it accurately reflects the codebase ([[validation-gates|FAR]])
2. **Plan document** — verify feasibility, atomicity, clarity, testability, scoping ([[validation-gates|FACTS]])

**Hidden cost:** This is genuine cognitive work, not rubber-stamping.

### The Time Tax

- Research review: ~10 min
- Plan review: ~15 min
- Implementation monitoring: varies
- **Total human time: 25-45 min per RPI project**

A team of 10 running 3-4 RPI projects/week = **2-3 engineer-hours weekly** in review work.

**If review is rushed or skipped, validation gates fail and you lose RPI's reliability benefit.**

## The Plan-Reading Illusion

HumanLayer's evolution from RPI → CRISPY/QRSPI was driven partly by this:

> Engineers often do not read plans carefully. They skim a 1,000-line plan, see that it looks coherent, and approve it without verifying assumptions. This creates an illusion of validation without actual review.

A well-written plan can be a coherent narrative **without actually understanding the codebase's real constraints or dependency patterns**.

**Symptom:** Plan looks good. Implementation starts. Halfway through, an assumption proves wrong. Rework cycle that RPI was supposed to prevent.

### How to Prevent This

1. **Use FACTS as a real checklist, not a scan:**
   - Feasible — can you trace the dependency path? Are tools available?
   - Atomic — can you explain the phase in one sentence? Dependencies clear?
   - Clear — could a new engineer execute this without asking questions?
   - Testable — exactly how do you verify success?
   - Scoped — does it stay in reasonable bounds?
2. **Domain experts review.** The engineer most familiar with the affected code reviews the plan. Rotation misses context.
3. **Clarifying questions are mandatory.** Unclear phase → send back. Plan must be self-contained.
4. **Escalate large plans.** 20+ phases → two reviewers.

## Team Friction Points

### 1. Ceremony Burden

**Risk:** engineers start with RPI, abandon mid-research because "this is taking too long," revert to unstructured prompting.

**Mitigation:**
- Keep ceremony light — Research 5-15 min, Plan 5-10 min. Longer → scope too large
- Use tools (Goose slash commands) to automate session management
- Skip RPI for trivial work — reserve for multi-file changes

### 2. Disagreement on Plans

**Risk:** plan approval becomes a debate. Work grinds to a halt.

**Mitigation:**
- Single approver per project (usually most senior or most familiar)
- Approver is tiebreaker; alternative is paralysis
- Document decisions so next attempt incorporates feedback

### 3. Cascade Delays

**Risk:** one approver = bottleneck. Engineers lose context/momentum waiting.

**Mitigation:**
- Rotate approver role
- SLAs: plan reviews within 4 working hours
- Automate research/plan generation via tooling; human review is the only bottleneck

### 4. Over-Engineering Simple Changes

**Risk:** 30 minutes of RPI for a one-line fix. Morale suffers.

**Mitigation:** codify the decision tree (see [[when-to-use|When to Use RPI]]); skip RPI for single-file changes, obvious bug fixes, prototypes.

## Evolution: From 1,000-Line Plans → Design Discussions

HumanLayer's response to scale-breakdown was **front-loading alignment** before detailed planning:

1. **Design discussion (~15 min, ~200 lines):** approach, tradeoffs, patterns to reuse
2. **Design decision doc (~200 lines):** what we're doing, why, what patterns we're following
3. **Structure outline (~2 pages):** how implementation breaks into phases, what verification looks like
4. **Only then the detailed plan** — which is now shorter because alignment is established

This front-loads human engagement to high-leverage points and produces better plans. See [[post-rpi-evolution|Post-RPI Evolution]] for CRISPY/QRSPI details.

## Scaling Playbook

### Small Team (3-5)

- One engineer approves all research/plans (most senior)
- Async review, not meetings
- Cadence: 3-4 RPI projects/week
- **Governance:** approver = decision maker; no committees

### Growing Team (6-15)

- Rotate approver across 2-3 senior engineers
- Review SLAs (4 hours)
- Full-team design discussions for contentious architecture
- Pair younger engineers with experienced reviewers
- **Governance:** defined approval process, published FACTS checklist

### Large Team (20+)

- Delegate by domain (frontend/backend/infra approvers)
- Stricter SLAs (2 hours) for unblocking
- Design discussions as standing meetings for large refactors
- RPI documented in team practices; new-engineer training
- Consider restricting RPI to senior engineers; juniors use lighter approaches
- **Governance:** RPI as development standard, enforced in code review

## Training and Knowledge Transfer

### Individual (1-2 weeks)

1. "Getting Started" walkthrough
2. One end-to-end RPI project (small, 3-4 files)
3. Review a peer's research + plan (observe FACTS validation)
4. First solo RPI project

### Team Adoption (~4 weeks)

- **Week 1:** Technical overview, show case study
- **Week 2:** Small pilots with 2-3 experienced engineers
- **Week 3:** Formalise decision tree
- **Week 4:** Set governance (approvers, SLAs, checklist)
- **Ongoing:** Rotate approver, document common issues

## Measuring Success

Track weekly:
- RPI projects completed
- Average project size (files affected)
- **Rework cycles (target: 0)**
- Review time (should stabilise at 30-45 min/project)
- Team sentiment (retrospective)

**Increasing rework cycles or review time → team is struggling with the methodology.**

## Handling Ceremony Fatigue

If you hear *"RPI is slowing us down":*

1. Measure where time is actually spent (research vs plan vs implementation)
2. Reduce the bloated phase (often over-detailed plans)
3. Check team is using the decision tree and skipping trivial work
4. Adopt HumanLayer's evolved approach (design discussion + structure outline)

## Common Pitfalls

| Pitfall | Sign | Fix |
|---|---|---|
| **RPI as bureaucracy** | *"Really Painful Implementation"* jokes | Reduce ceremony, ensure reviews add value |
| **Not reading plans carefully** | Rework cycles continue despite RPI | Shorter plans, design discussions, explicit FACTS checklist |
| **RPI for everything** | Engineers resent as wasteful | Enforce the decision tree |
| **Approver bottleneck** | Plan approval wait > 2 hours | Rotate approver, set SLAs, async review |

## When to Abandon RPI Mid-Project

- Problem is fundamentally different from research
- Plan's assumptions are wrong
- Scope tripled

**Pause, iterate, revise.** Do not force mechanical execution.

## Key Takeaways

- Team adoption requires **genuine review** (not rubber-stamping), **ceremony discipline**, **clear governance**, **continuous iteration**, **willingness to evolve**
- **Plan-reading illusion** is the silent killer — FACTS checklist must be a real gate
- **Front-load alignment** (design discussion + structure outline) before detailed planning
- Scale by rotating approvers, setting SLAs, and restricting RPI to the work where it pays off
- **Unresolved disagreement kills momentum** — good plan today > perfect plan never

## See Also

- [[when-to-use|When to Use RPI]] — the decision tree teams should enforce
- [[post-rpi-evolution|Post-RPI Evolution]] — CRISPY/QRSPI as a response to team-scale failures
- [[humanlayer-and-hitl|HumanLayer and HITL]] — ACE framework origins
- [[validation-gates|Validation Gates]] — FAR and FACTS that must be real, not rubber-stamped

# When to Use RPI

RPI works well for some problems and actively hurts for others. This is the decision framework.

## Quick Decision Tree

```
Is the problem well-understood and deterministic?
├─ No (exploratory, debugging, optimization)
│  └─ Skip RPI. Use lightweight prompting with iteration.
└─ Yes
   ├─ Single file?
   │  └─ Skip. Overhead exceeds benefit.
   ├─ 2-3 files with clear patterns?
   │  └─ Skip. Direct prompting is faster.
   └─ 4+ files OR complex dependencies?
      └─ Use RPI for reliability and zero rework.
```

## RPI Works Well For

### Multi-File Refactors (5-50 files)

- Rename a class across the codebase
- Migrate between patterns
- Remove a scattered feature

**Why:** research identifies all affected files; plan ensures coordinated changes; verification gates prevent incomplete refactors.

### Feature Additions (Brownfield)

New features in existing systems where you need to understand current architecture first.

**Why:** research maps existing patterns; plan replicates them; result feels native.

**Not for:** greenfield (no patterns to research).

### Migrations (DB, Framework, Language)

REST → GraphQL, database schema upgrades across services, Webpack → Vite in a monorepo.

**Why:** research documents current state; plan breaks migration into atomic testable steps; checkboxes enable recovery on long runs.

**Risk:** if migration exposes unknown issues (perf cliffs, compatibility gaps), RPI becomes rigid — pair with iteration.

### Complex Bug Fixes

Race conditions, state inconsistencies, logic bugs scattered across layers.

**Why:** research maps control flow; plan isolates root cause; verification prevents reintroduction.

**Not for:** obvious bugs ("return value was wrong").

### Incident Cleanup

Security patches across services, coordinated rollbacks, post-outage monitoring additions.

**Why:** systematic scope documentation, coordinated fixes, verification that incident is fully resolved.

## RPI Works Poorly For

### Exploratory Work

Debugging, perf optimisation, security hardening — where the solution is unknown.

**Why it fails:** RPI assumes you know what you're trying to do. The Plan phase cannot exist until you've investigated.

**Use instead:** lightweight iteration. Hypothesis → test → adjust.

**Example:** slow perf. You don't know if it's the DB query, serialisation, or frontend rendering. RPI would force you to plan before investigating.

### Non-Deterministic Work

ML tuning, hyperparameter search, A/B testing.

**Why:** correctness depends on empirical results, not logic. Can't be validated through tests and gates.

**Use instead:** loops with structured feedback.

### Greenfield Projects

New systems from scratch, no existing patterns.

**Why:** the Research phase has nothing to research. Skip Research → RPI becomes just "Plan and Implement," which isn't RPI.

**Use instead:** [[spec-driven-development/_index|SDD]] or loop-based execution. Spec-first.

### Trivial Changes

Single-file, obvious bug fixes, small additions.

**Why:** 5+ minute research phase for 10-minute total work. Overhead > benefit.

**Use instead:** direct prompting.

### Prototype / Throwaway Code

**Why:** optimising for learning velocity, not code quality. RPI trades speed for correctness — wrong tradeoff.

**Use instead:** vibe coding.

## The Over-Engineering Risk

RPI introduces ceremony. **If ceremony is too heavy, engineers skip steps and return to unstructured prompting** — the very problem RPI was meant to solve.

### When RPI Becomes Harmful

1. **Rigid exploration** — a plan becomes a straightjacket; if implementation reveals a better approach, mechanical execution discourages it
2. **Context starvation** — atomic tasks lose sight of the bigger picture
3. **Over-specification** — simple features get 1,000-line plans; edge cases ignored because "they weren't in the plan"
4. **False confidence** — coherent research + detailed plan creates an illusion of understanding; assumptions mask risks

### When to Abandon RPI Mid-Stream

Stop and iterate if:
- Research uncovers the problem is fundamentally different from what you thought
- Plan reveals work is much larger than expected (50+ phases)
- Implementation discovers a better architectural approach
- Verification reveals plan assumptions were wrong

**Do not force mechanical execution of a bad plan.**

## Comparison Table

| Scenario | Use RPI? | Why |
|---|---|---|
| Refactor spanning 20 files | Yes | Coordination + zero rework |
| Single-file bug fix | No | Overhead > benefit |
| Add feature to existing system | Yes | Research maps patterns |
| Build a new system | No | No patterns to research |
| Debug a race condition | Yes | Research maps control flow |
| Performance optimisation | No | Solution unknown |
| Database schema migration | Yes | Atomic steps prevent inconsistency |
| Documentation overhaul | Maybe | Large: yes; single page: no |
| Security patch deployment | Yes | Systematic verification |
| Prototype new UI | No | Learning velocity > quality |

## Cost Considerations

- **Time:** Research (5-15 min) + Plan (5-10 min) = 15-30 min before implementation
- **Tokens:** Multiple sub-agent queries + detailed plans → higher than direct prompting
- **Cognitive load:** Humans review artifacts before proceeding

### Break-Even Analysis

RPI breaks even when:
- Work would otherwise require 2+ rework cycles (30-60 min each)
- Change spans 5+ files with coordination-critical dependencies
- Code is production-critical; mistakes are expensive

**3-file change, low error cost:** direct prompting is faster.
**20-file refactor in production code:** RPI pays for itself.

## Team Adoption Note

Individual adoption is easy. Team scaling introduces friction — see [[team-adoption|Adopting RPI in Teams]].

## Summary

**Use RPI if:**
- Deterministic, well-understood
- 4+ files or complex dependencies
- High cost of mistakes
- You can afford 20-30 min upfront validation

**Skip RPI if:**
- Exploratory / non-deterministic
- Trivial or single-file
- Greenfield (no patterns)
- Prototyping or learning

**Be ready to abandon RPI if:**
- Research uncovers a fundamentally different problem
- Plan feels rigid and blocks better approaches
- Problem scope is much larger than expected

**RPI is a tool, not a law.** Adapt to the problem.

## See Also

- [[principles|Principles]] — why RPI works when it works
- [[getting-started|Getting Started]] — setup + worked example
- [[team-adoption|Team Adoption]] — scaling ceremony without burnout
- [[vs-other-frameworks|RPI vs Other Frameworks]] — when to use SDD, Quick Dev, Vibe instead
- [[quick-dev|Quick Dev]] — faster alternative for well-understood work

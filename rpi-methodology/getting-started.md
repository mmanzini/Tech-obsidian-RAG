# Getting Started

Minimal setup, naming conventions, and a worked example. For ergonomic native support see the Goose section of [[tool-agnosticism|Tool Agnosticism]].

## What You Need

- An AI assistant (Claude, Copilot, Cursor, Goose)
- A `thoughts/` folder in your project root
- Basic familiarity with your codebase

That's it.

## Directory Setup

```
your-project/
├── thoughts/
│   ├── research/   ← RPI research documents
│   └── plans/      ← RPI plan documents
└── (rest of code)
```

Commit `thoughts/` to version control.

## Naming Convention

`YYYY-MM-DD-HHmm-topic.md` — chronological, multi-artifact-per-day support.

- `2026-04-11-1430-auth-refactor.md` (research)
- `2026-04-11-1445-oauth-migration.md` (plan)

## Three Phases — Quick Overview

### Phase 1: Research (5-15 min)

**Goal:** Document what exists. **No opinions. No planning.**

Ask for:
- Relevant files + purposes
- How it currently works
- Patterns used throughout the codebase
- Open questions

**Review against [[validation-gates|FAR]]:** Factual, Actionable, Relevant. If vague, iterate clarifying questions. Save to `thoughts/research/`.

### Phase 2: Plan (5-10 min)

**Goal:** Break change into atomic steps with success criteria.

- Share research, state goal
- Ask for numbered plan: each phase = single focused task, exact file paths, code snippets, checkboxes, success criteria

**Review against [[validation-gates|FACTS]]:** Feasible, Atomic, Clear, Testable, Scoped. If 20+ phases → consolidate or reduce scope. Save to `thoughts/plans/`.

### Phase 3: Implement

**Goal:** Execute mechanically with verification.

- Fresh session, share the plan
- Execute phase by phase
- Verify after each (tests, build, linter)
- Update checkboxes
- If verification fails: stop and diagnose

**Three validation strategies:** task-by-task (max control), phase-by-phase (balanced), full plan (high confidence).

## Worked Example: Add Logging to User Service

### Research — `thoughts/research/2026-04-11-1430-user-logging.md`

```markdown
# Research: User Service Logging

## Current State
- User service in `services/user.ts`
- 8 endpoints (getUser, createUser, updateUser, etc.)
- No structured logging, only console.log
- Tests in `tests/user.test.ts`

## Relevant Patterns
- Database layer already uses Winston logger (`db/connection.ts`)
- API middleware uses the same logger (`middleware/logging.ts`)
- Config pattern: logger passed as dependency

## Questions
- Log at entry AND exit of each endpoint?
- Sensitive data filtering — which fields to redact?
- Debug vs info level for routine operations?
```

### Plan — `thoughts/plans/2026-04-11-1440-user-logging-add.md`

```markdown
# Plan: Add Structured Logging to User Service

## Phase 1: Import Logger
- [ ] File: `services/user.ts`
- [ ] Add: `import logger from '../logger'`
- [ ] Success: No errors on build

## Phase 2: Log User Retrieval
- [ ] File: `services/user.ts` line 15
- [ ] Add: `logger.info('Fetching user', { userId: id })`
- [ ] Run: `npm test -- user.test.ts`
- [ ] Success: All tests pass, no new warnings

## Phase 3: Log User Creation
- [ ] File: `services/user.ts` line 35
- [ ] Add: `logger.info('Creating user', { email: data.email })`
- [ ] Note: Omit password from logs
- [ ] Run: `npm test`
- [ ] Success: Tests pass

## Phase 4: Verify No Sensitive Data
- [ ] Grep: `grep -n "password\|token\|secret" services/user.ts`
- [ ] Ensure: No such data in log statements
- [ ] Run: `npm run lint`
- [ ] Success: No warnings

## Phase 5: Manual Test
- [ ] Start dev server: `npm run dev`
- [ ] Create a user via API
- [ ] Check logs: structured JSON output, no sensitive data
- [ ] Success: Logs appear correctly formatted
```

### Implementation

Fresh session. Share plan. Prompt: *"Execute this plan step by step. After each phase, run the verification. Update the checkboxes."* Monitor for failures.

## Goose (Automated)

```bash
/research_codebase "logging in the user service"
/create_plan "add structured logging to user service"
/implement_plan "thoughts/plans/2026-04-11-HHmm-user-logging.md"
```

## When RPI Pays Off

**Use RPI when:**
- Change spans 3+ files
- Multiple dependencies between changes
- High cost of mistakes (production, financial)
- Complex refactors or migrations

**Skip RPI when:**
- Single-file change
- Obvious bug fix
- Quick prototype
- Trivial additions

See [[when-to-use|When to Use RPI]] for the full decision framework.

## Troubleshooting

- **Research too vague?** Ask clarifying questions one at a time ("Which functions call the authentication handler?")
- **Plan has 20+ phases?** Ask AI to consolidate or reduce scope (target 5-15)
- **Implementation fails verification?** Stop. Diagnose the specific error. Often the plan needs a small adjustment (missing file, wrong path)

## Key Principles (Recap)

1. **One goal per session** — start fresh between phases
2. **Research has strict rules** — document only, no planning
3. **Plans must be self-contained** — a fresh AI session executes without your help
4. **Checkboxes enable recovery** — if context fills mid-implementation
5. **Implementation should feel boring** — if creative, upstream is weak

## Next Steps

1. Create `thoughts/` folder
2. Pick a small multi-file change
3. Run Research → review FAR → Plan → review FACTS → Implement
4. Once comfortable, scale to larger changes

## Key Takeaways

- Minimal setup: a `thoughts/` directory, three phases (Research → Plan → Implement), and fresh context between each phase.
- Research is documentation only — no opinions, no planning; validate with FAR (Factual, Actionable, Relevant).
- Plans must be self-contained and atomic; validate with FACTS (Feasible, Atomic, Clear, Testable, Scoped); target 5–15 phases.
- Implementation should feel mechanical — if it requires creativity, the upstream phase was weak.
- RPI pays off when a change spans 3+ files, involves complex dependencies, or carries a high cost of mistakes.

## See Also

- [[workflow|Workflow]] — the full lifecycle
- [[validation-gates|Validation Gates]] — FAR and FACTS reference
- [[when-to-use|When to Use RPI]] — decision framework
- [[tool-agnosticism|Tool Agnosticism]] — implementation across tools

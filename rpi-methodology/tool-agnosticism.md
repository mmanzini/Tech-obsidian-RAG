# Tool Agnosticism

RPI is **tool-agnostic in principle** — the workflow runs with any structured-prompt-capable AI assistant. But **implementation ergonomics vary dramatically**. Goose is the only tool with native RPI support; everything else is manual.

## Comparison Matrix

| Tool | Research | Planning | Implementation | Checkpoints | Native RPI Commands | Ease |
|---|---|---|---|---|---|---|
| **Goose** | 3 parallel sub-agents (`/research_codebase`) | `/create_plan` | `/implement_plan` + auto-verify | Built-in | Yes | High |
| **Claude Code** | Manual prompting | Manual | Manual | Manual markdown | No | Medium |
| **Cursor** | Manual w/ `@file` refs | Manual | Inline edits | Manual | No | Medium |
| **GitHub Copilot** | Very limited | Via extended context | In-editor | Separate tracking | No | Low-Medium |
| **Kiro (AWS)** | Spec-based (SDD, not RPI) | IDE-native | IDE-native | IDE-native | No (uses SDD) | N/A |

## Goose: Native RPI Support

Most ergonomic. Purpose-built slash commands + sub-agent orchestration:

```bash
/research_codebase "user authentication system in our API"
# Spawns find_files, analyze_code, find_patterns in parallel
# → thoughts/research/2026-04-11-1430-user-authentication.md

/create_plan "refactor authentication to use OAuth2"
# Reads research, asks clarifications
# → thoughts/plans/2026-04-11-1445-auth-refactor.md

/implement_plan "thoughts/plans/2026-04-11-1445-auth-refactor.md"
# Executes with automatic verification, updates checkboxes

/iterate_plan "thoughts/plans/2026-04-11-1445-auth-refactor.md" "Feedback: Use OpenID instead"
# Researches only what changed, updates plan surgically
```

**Advantages:** Explicit, reproducible workflow. Automatic sub-agent orchestration. Built-in verification gates.

## Claude Code: Manual RPI

No `/research` or `/plan` commands. Each phase is manual prompting:

1. **Research session:** "Analyze X. Explore dirs Y. No opinions, just facts." → save to `thoughts/research/`
2. **Plan session** (fresh): upload research → "Design a plan with 6-10 atomic phases, file paths, code snippets, success criteria, checkboxes" → save to `thoughts/plans/`
3. **Implement session** (fresh): upload plan → "Execute mechanically, phase by phase, verify after each, update checkboxes, do not deviate"
4. **Iterate** (if needed): upload plan + feedback → "Research only what changed, update surgically"

**Disadvantage:** discipline required. No sub-agent parallelism. Manual checkbox editing.
**Advantage:** works within Claude Code's existing feature set; no new tool to learn.

## Cursor: Editor-Based RPI

- Use codebase search to find relevant files
- `@thoughts/research/...` to reference research in planning
- Implementation via Cursor's inline edits with integrated terminal for verification
- Manual checkbox tracking

**Advantage:** IDE integration, direct edits, built-in terminal.
**Disadvantage:** less structured than Goose, no sub-agent parallelism.

## GitHub Copilot: Limited RPI Support

Designed for in-editor tactical code generation, not structured workflows.

- **Research:** very limited — use separate grep/semantic search, feed Copilot a summary
- **Planning:** comments or separate doc
- **Implementation:** in-editor strength
- **Checkpoint tracking:** manual separate file

**Verdict:** possible but awkward. Copilot is better for quick dev than structured RPI.

## Manual RPI — Prompt Templates

### Research (Session 1)

```
Analyze the [system name] in this codebase.
Explore: [directories].
Document every file involved, how they work together, key patterns, open questions.
Structure:
- Git metadata
- File reference list
- Component descriptions
- Flow diagrams (ASCII or descriptions)
- Patterns observed
- Open questions
Provide only facts. No opinions or solutions.
```

**Review:** apply [[validation-gates|FAR scale]].

### Plan (Session 2, fresh)

```
You are planning [goal]. Here is the research:
[paste research]

Ask any clarifying questions. Then design a phased plan:
- 6-15 numbered phases
- Each phase: exact file paths, line numbers, code snippets
- Success criteria per phase
- Atomic tasks (single focused unit)
- Checkboxes for tracking

Format:
## Phase 1: [description]
- [ ] Specific task (file path, line numbers)
  Success: [how to verify]
```

**Review:** apply [[validation-gates|FACTS scale]].

### Implement (Session 3, fresh)

```
You are implementing this plan:
[paste plan]

Execute phase by phase. After each phase:
1. Run verification (tests, build, linter)
2. Update checkboxes: mark - [x] when complete
3. Report success or failure
4. Do not proceed if verification fails

Work mechanically. Do not deviate. Do not add features not in the plan.
If a task is impossible, stop and ask why before skipping.
```

### Iterate (Session 4, optional)

```
Here is the plan you were executing:
[paste plan with completed checkboxes]

And here is feedback:
[paste feedback]

Research only what changed. Update the plan surgically:
- Keep completed phases marked [x]
- Modify only affected phases
- Re-estimate effort
```

## The Honest Truth

RPI claims tool-agnosticism — technically true. Practically, **RPI is dramatically more ergonomic with Goose**.

**Manual RPI friction points (Claude Code, Cursor, Copilot):**
- Explicit session creation between phases
- Manual prompting each phase
- Careful file organisation in `thoughts/`
- Human-driven checkpoint tracking

**Observed degraded behaviours on manual RPI teams:**
- Skip phases when deadlines press
- Combine phases to reduce session overhead (losing isolation)
- Neglect checkpoint tracking (losing recovery)
- Under-specify plans to save prompting effort (making implementation unpredictable)

**Verdict:** RPI is theoretically tool-agnostic, **practically Goose-optimised**. Manual implementation is viable with discipline, but Goose removes the discipline requirement through native tooling.

## Key Takeaways

- **Only Goose has native RPI commands** (`/research_codebase`, `/create_plan`, `/implement_plan`, `/iterate_plan`)
- Claude Code, Cursor, Copilot all require **manual phase management** and hand-crafted prompts
- **Tool choice materially affects whether validation discipline is maintained** — friction leads to skipping gates
- Copilot is a bad fit; Kiro uses SDD rather than RPI
- Manual RPI works with discipline; Goose works without requiring it

## See Also

- [[Research/Wiki/rpi-methodology/workflow|Workflow]] — the lifecycle being implemented across tools
- [[getting-started|Getting Started]] — manual worked example
- [[vs-other-frameworks|RPI vs Other Frameworks]] — Kiro and other IDE choices
- [[subagents-in-claude-code|How and When to Use Subagents in Claude Code]] — closest Claude Code primitive to Goose's research sub-agents

# Validation Gates — FAR & FACTS

The two scales RPI uses as human-review checkpoints. Both are **useful heuristics, not proven metrics** — see the critical assessment below.

## FAR Scale — Research Validation

Evaluated before proceeding from research to planning.

### Factual (target ≥ 4.0)

- Claims grounded in actual code, not assumptions
- Every significant claim has file path + line number
- No "probably" / "likely" without evidence
- Example: *"Auth uses JWT (see `auth/tokens.ts:47`)"* ≠ *"auth probably uses OAuth"*
- **Prevents:** hallucination, the primary failure mode of early AI assistance

### Actionable (target ≥ 3)

- Clear what must be built; scope explicit
- Dependencies identified (what changes, in what order)
- Implementation hooks clear (where changes occur)
- **Prevents:** unfocused planning → implementation drift

### Relevant (target ≥ 3)

- Directly addresses the user's stated need
- Research focuses on the actual problem, not peripheral systems
- **Prevents:** "we researched deeply but answered the wrong question"

## FACTS Scale — Plan Validation

Evaluated before proceeding from planning to implementation.

### Feasible (≥ 3)

- All tasks accomplishable with available tools and patterns
- No circular / forward-reference dependencies
- Team has skills to execute
- **Prevents:** plans that require non-existent APIs or patterns that don't fit

### Atomic (≥ 3)

- Each task is a single focused unit of work
- No compound operations ("refactor module AND write tests")
- Completes in one context window without context switching
- **Prevents:** context overflow mid-implementation, broken recovery

### Clear (≥ 3)

- Instructions unambiguous; exact file paths, line numbers, snippets
- Before/after examples for non-trivial changes
- Objective success criteria
- **Prevents:** interpretation-driven rework

### Testable (≥ 3)

- Explicit success criteria per task
- Automated checks where possible (tests, build, linter)
- Manual verification documented if needed
- **Prevents:** untestable work, subjective acceptance

### Scoped (≥ 3)

- Work boundaries defined; phase boundaries prevent infinite loops
- Total effort estimate provided
- Plan doesn't spawn new phases during implementation
- **Prevents:** runaway expansion

## Scoring Guide (0–5, applies to both scales)

- **5** — Fully meets criterion; no gaps
- **3** — Generally meets, with some qualifications
- **1** — Major deficiencies

## What FAR and FACTS Do Well

- Make implicit quality criteria explicit
- Prevent obviously bad plans from proceeding
- Encourage breaking down complex work
- Create checkpoints where humans catch errors early

## What FAR and FACTS Do **Not** Do (Honest Assessment)

- **No inter-rater reliability testing** — two reviewers may score the same document differently; no published study quantifies disagreement
- **No threshold justification** — why Factual ≥ 4.0 but Actionable ≥ 3? Pragmatic defaults, not data-driven
- **No validation data** — no study shows projects passing FAR have fewer rework cycles than those skipping it
- **No false positive/negative analysis** — how often do plans passing FACTS still fail? Unknown
- **Subjective application** — "clear," "actionable," "relevant" depend on reviewer judgment; not objective like "X% test coverage"

## Practical Use

Treat FAR/FACTS as **checklists to catch obvious gaps**, not as guarantees:

- Use to catch: research with no file references, plans with compound tasks
- Adjust thresholds by task risk (higher for risky work; looser for exploratory)
- Expect reviewer disagreement — discuss rather than deferring to single-reviewer verdicts
- Document deviations ("this research is inherently speculative, accepting Actionable = 2")

The scales' **real value** is forcing explicitness about what "good research" and "good planning" mean — then checking whether the artifacts meet those criteria before proceeding.

## Key Takeaways

- **FAR gates research** (Factual, Actionable, Relevant) — target ≥ 4.0 / 3 / 3
- **FACTS gates plans** (Feasible, Atomic, Clear, Testable, Scoped) — target ≥ 3 across the board
- Each criterion maps to a **specific failure mode** the gate is supposed to prevent
- Scales are **heuristics, not metrics** — no validated inter-rater reliability or outcome correlation
- Real value: forcing explicitness about what "good" means, then checking against it

## See Also

- [[Intelligence/Tech research/Wiki/rpi-methodology/principles|Principles]] — why these gates exist
- [[workflow|Workflow]] — where FAR and FACTS sit in the lifecycle
- [[team-adoption|Team Adoption]] — the plan-reading illusion that degrades FACTS in practice
- [[adversarial-review|Adversarial Review]] — forced-finding review that counteracts rubber-stamping

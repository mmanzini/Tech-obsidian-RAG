# Principles

The three foundational convictions underlying RPI. Each principle is paired with its mechanism, evidence, and — crucially — its stated limits.

## 1. Phase Separation Works

- RPI runs Research, Plan, and Implement in **separate fresh context windows** — not bureaucratic overhead but a direct response to how LLMs function
- Without separation, research file contents pollute working memory → planning happens with that noise → implementation inherits both → **instruction adherence degrades**
- The mechanism: *context is the only lever*. A model cannot improve from within a cluttered context window

## 2. Fresh Contexts Matter (Because Upstream Review Beats Post-Hoc Fixes)

- Humans reviewing intermediate artifacts (research doc, plan doc) catches errors **before** code is written — cheaper than undoing implementation
- Case study (Goose feature removal, 32 files): 9 min research + 4 min plan + 39 min implementation = **52 min total, zero rework**
- **Limitation (acknowledged):** advantage is strongest for well-understood changes (refactors, removals, migrations). Exploratory work (performance tuning, security, debugging) may discover mid-research that the original problem statement was wrong

## 3. Validation Gates Prevent Specific Failure Modes

The [[validation-gates|FAR and FACTS scales]] are explicit failure-prevention mechanisms, not checkboxes:

| Gate | Prevents |
|---|---|
| FAR / Factual | Hallucination — research must cite actual code |
| FAR / Relevant | Wrong problem solved — human catches "we researched auth but the real issue is sessions" |
| FACTS / Atomic | Context overflow mid-implementation |
| FACTS / Testable | Untestable work, subjective acceptance criteria |
| FACTS / Scoped | Runaway scope creep |

## The Honest Caveat: Advocated Not Validated

These principles are advocated by RPI creators (HumanLayer, Block, Kilo, Anthropic) and **lack independent validation**:

- No peer-reviewed research comparing RPI to alternatives on controlled projects
- No published *failure* case studies — only successes documented
- FAR/FACTS **inter-rater reliability untested** — do two humans score the same research identically?
- 30-50x speed improvement figure inferred from a single data point
- Adoption count ("10,000 people downloaded our RPI prompts") doesn't tell us retention or satisfaction

Unvalidated particularly for: large refactors (100+ files) where plan size exceeds human comprehension, exploratory work, teams where validation discipline breaks down to rubber-stamping.

## The Mindset Shift

> **Traditional:** "How do I craft the perfect initial prompt so the AI gets it right the first time?"
> **RPI:** "How do I structure work so the AI executes reliably, with humans validating where judgment matters most?"

Not about the model being stupid — about recognising that context is the only lever and using it strategically.

## Key Takeaways

- **Phase separation = context hygiene**, not bureaucracy
- **Upstream review > post-hoc code review** for predictable changes
- **Validation gates prevent specific failure modes**, but RPI's creators freely acknowledge the gates themselves have not been rigorously validated
- Treat RPI as a useful heuristic framework, not a proven methodology

## See Also

- [[Research/Wiki/rpi-methodology/workflow|Workflow]] — how the three phases operate in detail
- [[validation-gates|Validation Gates]] — FAR and FACTS scales
- [[context-engineering|Context Engineering]] — the instruction budget constraint behind phase separation
- [[when-to-use|When to Use RPI]] — where these principles pay off and where they don't
- [[twelve-factor-agents|Twelve-Factor Agents]] — foundational reliability principles RPI builds on

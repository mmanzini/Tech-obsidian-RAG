# Context Engineering

Language model performance is determined entirely by input context quality. **The contents of your context window are the only lever you have.** Expanding window size (100k → 1M) does not automatically improve capability — the real constraint is **instruction budget**, not window size.

## The Instruction Budget Hypothesis

### Claim

Frontier LLMs can reliably follow **~150-200 instructions**, regardless of context window size. Larger/reasoning models exceed this, but it remains a fundamental constraint separate from window size.

### Partial Academic Validation

- **IFScale benchmark (2507.11538):** reasoning models (o3, Gemini 2.5 Pro) maintain near-perfect performance through **250-300** instructions before degradation
- **Standard frontier models** (Claude Sonnet, GPT-4o) show linear-to-exponential degradation starting ~100-150 instructions
- Exact threshold varies by model architecture, instruction complexity, and whether instructions are duplicated at prompt boundaries (primacy/recency helps)

### Context Window ≠ Instruction Capacity

- Extended-context models (1M windows) use mathematical scaling (YaRN) that stretches sequence length **without** increasing instruction capacity
- Doubling window size does **not** double instruction-following capacity

### Why This Drove RPI → CRISPY Evolution

HumanLayer discovered their planning prompt contained **85+ instructions** — 56% of a 150-instruction budget consumed before core work. Solution: decompose the monolithic 3-phase prompt into [[post-rpi-evolution|7-phase CRISPY/QRSPI]], each stage < 40 instructions.

## Context Rot and Degradation

Quality degrades as context fills. Effectiveness begins declining past **40-60% utilisation**, substantially degraded past **70%**.

### Mechanisms

- **Position bias** — models weight beginning (system msg) and end (recent content) more heavily than middle
- **Attention dilution** — each token receives less attention as context grows
- **Instruction adherence loss** — critical mid-context instructions more likely to be ignored
- **Recall degradation** — model loses track of information provided early

### Practical Target: 40-60% Utilisation

For a 200k model: aim 80-120k, preserve 80-120k as buffer. When approaching 70-80%, compact or reset context.

## Session Isolation: Why Fresh Contexts Per Phase

| Phase | Fresh context benefit |
|---|---|
| **Research** | Read codebase without planning/implementation noise; produce a clean fact document |
| **Plan** | Read research without implementation details; design cleanly on facts |
| **Implement** | Read plan without research noise (which may contradict the plan due to code evolution); execute mechanically |

### Cost

- Token usage to reload research/plan documents
- Explicit context setup; no implicit carryover
- Clear handoffs (artifacts must be self-contained)

**Benefit justification:** context pollution that would degrade all downstream work is prevented. Net win.

## Sub-Agent Pattern: Context Firewalls

Sub-agents are autonomous sessions spawned for specific tasks. Each receives a fresh context window; returns only a summary.

| Pattern | Use Case |
|---|---|
| **Research sub-agents** | find_files, analyze_code, find_patterns work in parallel without polluting each other's contexts |
| **Phase sub-agents** | Large plans delegate each phase to a sub-agent, preventing single-context accumulation |
| **Task sub-agents** | High-risk tasks in isolation; only result summary returns to parent |

Rule of thumb: exploring 10+ files OR 3+ independent pieces of work → reach for sub-agents.

## Anthropic's Three Priorities (Strict Order)

1. **Correctness** (highest) — invalid info causes widespread failures; exclude it even if it wastes tokens
2. **Completeness** — missing context forces assumptions; include supporting info if it helps avoid hallucination
3. **Size** (lowest) — excessive tokens dilute signal; trim if quality is unaffected

> **Including slightly more correct information to ensure accuracy is preferable to omitting to save tokens. Incorrect information is categorically worse than either.**

## Why Bigger Context Windows Don't Solve the Problem

Expanding 100k → 1M appears to be a solution but introduces new problems:

- **Instruction budget problem:** 1M tokens with same 150-instruction budget is *worse* than 100k — haystack is 10x larger relative to the needle
- **Position bias problem:** instructions in middle of 1M context receive even less attention than in 100k
- **Context rot problem:** degradation compounds

**Solution:** break work into focused sub-agent operations. Multiple 50k windows > one 500k window.

## Implementation Strategy

- **Fresh sessions per phase** — research spawns sub-agents; plan reads research only; implementation reads plan only
- **Compact artifacts** — research = facts only; plan = executable; both fit in remaining budget with 40-60% buffer
- **Small focused context beats large mixed** — 50k beats 500k
- **Place critical instructions at beginning and end** to overcome position bias
- **Compact or reset at 70%**, not 100%

## Key Takeaways

- **Context is the only lever** on LLM output quality — optimise ruthlessly
- **Instruction budget (~150-200 for standard, ~250-300 for reasoning) is independent of window size**
- **Target 40-60% utilisation** — beyond 60%, context rot accelerates
- **Session isolation per phase** prevents pollution compounding across the workflow
- **Sub-agents are context firewalls** — use when exploring 10+ files or 3+ independent pieces
- **Bigger windows don't help** — they worsen position bias and context rot
- **Anthropic priority order:** correctness → completeness → size (incorrect info is worst)

## See Also

- [[Research/Wiki/rpi-methodology/principles|Principles]] — phase separation applies context engineering at the workflow layer
- [[Research/Wiki/rpi-methodology/workflow|Workflow]] — three-phase lifecycle as context engineering in practice
- [[post-rpi-evolution|Post-RPI Evolution]] — CRISPY/QRSPI as instruction-budget decomposition
- [[humanlayer-and-hitl|HumanLayer and HITL]] — ACE framework origin and the 40-60% target
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering]] — "sub-agents as context firewall" framing
- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence]] — "let Claude manage its own context"
- [[twelve-factor-agents|Twelve-Factor Agents]] — "own your context window" principle
- [[advisor-strategy|The Advisor Strategy]] — alternative route to reasoning-model instruction capacity without multi-session decomposition

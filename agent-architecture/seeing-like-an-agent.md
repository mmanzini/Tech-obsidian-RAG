# Seeing Like an Agent — How We Design Tools in Claude Code

**Source:** [claude.com/blog/seeing-like-an-agent](https://claude.com/blog/seeing-like-an-agent) (Thariq Shihipar, Anthropic, 2026-04-10)

Designing tools is one of the hardest parts of building an agent harness. Claude acts entirely through tool calling, but there's a wide spectrum between "one general-purpose tool like bash" and "fifty specialized tools, one per use case." The right answer depends on the model's abilities. Thariq's framework: **learn to see like an agent** by watching its outputs, experimenting, and adjusting as capability grows.

## The math-problem analogy

Imagine being given a hard math problem — what tools would you want? Depends on your skill set.

- **Paper** — the minimum; limited by manual calculation.
- **Calculator** — better, if you know the advanced options.
- **Computer** — fastest and most powerful, if you can write and execute code.

Same logic applies to designing agent tools: shape them to the model's abilities. And you only learn what those abilities are by **paying attention and reading outputs**.

## Elicitation — how the AskUserQuestion tool was born

Goal: improve Claude's ability to ask clarifying questions (elicitation). Claude could already ask questions in plain text, but answering them felt slow. Three attempts:

### Attempt 1 — Edit ExitPlanTool

Add a `questions` array parameter to the existing ExitPlanTool. Easy to implement but confused Claude — simultaneously asking for a plan and questions about the plan raised ambiguity. (What if answers conflict with the plan? Call the tool twice?) Shelved before ship.

### Attempt 2 — Modified output format

Prompt Claude to emit a specific markdown structure (bullet questions with alternatives in brackets) that the harness could parse into UI. Claude could *usually* produce it, but would append extra sentences, drop options, or abandon the structure altogether. Not reliable enough.

### Attempt 3 — Dedicated AskUserQuestion tool

Landed on a first-class tool Claude could call at any time, particularly prompted during plan mode. Triggering it shows a modal and blocks the agent loop until the user answers.

Why this worked:
- **Structured output** — the tool contract forces the shape.
- **Multiple options guaranteed** — the schema enforces alternatives.
- **Composability** — callable in the Agent SDK, referable in skills.
- **Most importantly, Claude liked calling it.** Even a well-designed tool fails if the model doesn't understand when to invoke it.

**Lesson:** the best interface is the one the model naturally reaches for, not the one that's cleanest on paper.

## When a helpful tool starts getting in the way — todos → tasks

At launch, Claude Code used a **TodoWrite** tool to let the model maintain a checklist. The harness also inserted system reminders every 5 turns to keep Claude on track. It worked for the models of the time.

As models improved, todo lists became constraining:

- System reminders made Claude think it had to **stick** to the list rather than modify it when a change of course was warranted.
- Opus 4.5 got much better at using sub-agents — but how do sub-agents coordinate on a shared todo list?

The replacement: the **Task tool**. Todos focus on keeping one model on track; **tasks** help agents communicate with each other. Tasks support dependencies, share updates across sub-agents, and can be altered or deleted by the model.

**Lesson:** as capabilities increase, tools that once helped can start constraining. Revisit old assumptions. This is also why it's useful to support only a **small set of models with similar capability profiles** — otherwise your tool surface drifts into inconsistency.

## Search — giving Claude its own context

The most consequential tools are the ones that let Claude **find its own context**.

- **Initially:** a RAG pipeline pre-indexed the codebase; the harness retrieved snippets and handed them to Claude before each response. Powerful and fast, but required setup, was fragile across environments, and — most importantly — **Claude was given context rather than finding it**.
- **Reframing:** if Claude can search the web, why not the codebase? A **Grep tool** let it build its own context.
- **Progressive disclosure via Skills:** [Agent Skills](https://agentskills.io/home) formalized the idea. Claude can read skill files that reference other files it reads recursively. Nested search across layers.

Over a year, Claude went from "can't really build context" to "can do nested search across several layers to find exactly the context it needs."

**Lesson:** as the model gets smarter, giving it the means to find context beats pre-chewing context for it.

## Progressive disclosure — adding capability without adding tools

Claude Code has ~20 tools, and the team re-audits the set frequently. **The bar to add a new tool is high** — every addition is one more option the model must consider.

Case in point: Claude didn't know much about Claude Code itself. Ask it how to add an MCP or what a slash command does and it couldn't answer.

- **Option A:** stuff the docs into the system prompt. Rejected — users rarely asked, and it would have added context rot and interfered with Claude Code's main job of writing code.
- **Option B:** give Claude a link to the docs it could load on demand. Worked, but Claude pulled large chunks into context to find an answer the user could have gotten in one sentence.
- **Option C (shipped):** build a **Claude Code Guide sub-agent** Claude calls whenever a user asks about Claude Code itself. The sub-agent does the doc-searching in its own context, follows detailed search/extract instructions, and hands back just the answer. The main agent's context stays clean.

Not a perfect solution — Claude can still get confused when asked how to set itself up — but it **adds capability to Claude's action space without adding a new tool**. Progressive disclosure as a design pattern.

## Principles

- **Shape tools to the model's abilities.** Don't over-specialize past what the model can use, don't leave powerful capability locked behind too-primitive tools.
- **The model has to like calling your tool.** A structurally clean interface the model ignores is worse than an uglier one the model reliably invokes.
- **Re-audit constantly.** Capability growth can turn yesterday's scaffolding into today's constraint.
- **Prefer progressive disclosure** to new tool slots. Sub-agents, skills, and recursively-readable files let you expand the action space without expanding the tool surface.
- **Give Claude the ability to find its own context** rather than pre-fetching it. This scales with model capability.
- **Pick a narrow set of models with similar capability profiles** — so the tool design stays internally consistent.

## Key Takeaways

- Tool design is the hardest part of harness construction. Watch the model, read outputs, experiment — "see like an agent."
- AskUserQuestion shipped only after two failed attempts (ExitPlanTool param, markdown format). What made it work: structured contract + the model naturally reached for it.
- Todo → Task transition: a scaffolding tool became constraining as models improved. **Capability growth can make your old tools obsolete.** Revisit assumptions.
- Consequential shift in search: **let Claude build its own context with Grep + progressive disclosure via Skills** instead of pre-fetching via RAG.
- **The bar to add a new tool is high** — each addition is another choice for the model. Prefer sub-agents and skills (progressive disclosure) to new top-level tools.
- Support a **small, similar set of models** so tool design stays coherent.
- Related: [[agent-architecture/twelve-factor-agents|twelve-factor-agents]], [[harness-engineering/subagents-in-claude-code|subagents-in-claude-code]], [[harness-engineering/skill-issue-harness-engineering|skill-issue-harness-engineering]].

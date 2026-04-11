# Product Management on the AI Exponential

**Source:** [claude.com/blog/product-management-on-the-ai-exponential](https://claude.com/blog/product-management-on-the-ai-exponential) (Cat Wu, Head of Product for Claude Code at Anthropic, 2026-04-11)

The traditional PM playbook assumes the technology frontier at project kickoff is roughly the frontier at launch. Exponentially improving models break that assumption — constraints you designed around may vanish mid-project. Cat Wu's argument: product teams must reorganize around rapid experimentation, consistent shipping, and doubling-down on what works, not long-horizon roadmaps.

## The frontier moves faster than the project

- **Sonnet 3.5 (new), Oct 2024** → first Claude Code prototype; METR measured it completing tasks that took humans ~21 minutes.
- **Opus 4, June 2025** → occasionally one-shots an "add a table tool to Excalidraw" prompt; good enough for a pre-recorded demo.
- **Opus 4.6, 2026** → one-shots Excalidraw feature requests reliably enough for live on-stage demos. METR: ~12-hour human-equivalent tasks, ~41× jump in 16 months.

The consequence: you are **building on ground that is rising underneath you**.

## Three-product division of labor (Cat's personal workflow)

| Tool | Role | Used for |
|---|---|---|
| **Claude.ai** | Thought partner (no action) | Strategy docs, bouncing ideas, tricky situations, quick answers |
| **Claude Code** | Code output | Prototypes, evals, scripts that call the Claude API |
| **Claude Cowork** | Knowledge work | Inbox zero, todos, slide decks, Slack decision archaeology, travel booking |

Industry PMs interviewed describe similar splits — the workflow has converged on distinct surfaces for distinct output types.

## Four shifts the Claude Code team embraced

### 1. Plan in short sprints — "side quests"

Traditional PM thinking treats exploration as pre-roadmap research. Instead: **everyone** (eng, PM, design) takes side quests — short self-directed experiments outside the official roadmap. An afternoon prototyping, testing a capability you assumed was out of reach, seeing what happens when you push the model harder than expected.

Popular Claude Code features — Claude Code on Desktop, the AskUserQuestion tool, todo lists — all emerged from side quests.

### 2. Demos and evals over docs

Documentation-first → prototype-first. No traditional stand-ups; share demos of new ideas. Internal users try them; the ones with real engagement get polished. **Wrong bets are cheap because you can prototype in an afternoon.**

- **Pro-tip:** after you write a spec, hand it to Claude Code and see if it can build it. Even a rough prototype changes the conversation.
- **Evals for abstract features:** for agent teams, Conner hand-crafted evals to understand when the feature works, when it doesn't, and what to fix. Measuring lets you improve.

See [[spec-driven-development/_index|Spec-Driven Development]] for the other end of this — the spec as the durable artifact.

### 3. Revisit features with every new model

Every model release is an implicit prompt to revisit what you already built. Best way to catch the windows: be a **daily active user** who deliberately asks the model to do things you think might be too hard.

Example — **Claude Code with Chrome**: users were building web apps in Claude Code, then manually switching to Claude in Chrome to test them, copying and pasting between tools. It worked well enough that the team realized it should be a built-in feature. **"If users are hacking something together, that's scaffolding you can build into the product."**

**Always optimize for capability first when prototyping.** Use more tokens than you think you need. A common mistake is cutting token cost too early and shipping something much less capable. You can always bring cost down later as cheaper models catch up — but first you need to know the feature is even possible.

### 4. Do the simple thing that works

A guiding Anthropic principle. If your product cleverly works around a model limitation, that workaround becomes unnecessary complexity when the next model drops. Simpler implementations swap in new capabilities more easily.

Concrete case: when todo lists first launched in Claude Code, the model wouldn't reliably check off items, so the team added system reminders every few messages to nudge it. It worked but was a hack. With the next model the behavior came for free and the reminders were removed. Same pattern with system prompts and tool descriptions — heavily engineered to compensate for past limits, then cut with each new model (20% reduction for Opus 4.6).

## Roles blending inside the team

- Designers ship code.
- Engineers make product decisions.
- Product managers build prototypes and evals.

This works because **clear strategy and goals let everyone prioritize autonomously**. The PM's remaining job: create clarity in the ambiguity model progress creates, push the team to think bigger, clear the path to shipping faster. Related [[ai-organization/from-hierarchy-to-intelligence|from-hierarchy-to-intelligence]] thesis: coordination layer dissolves into shared intelligence.

## The perfectionist's dilemma

Traditional PMs love tight control over the full experience. AI pushes you to **let go** in order to move quickly. It feels like surfing a wave — the point is to stay on it. Identify the handful of true non-negotiables; let the rest go.

The net effect: product teams move significantly faster. When a PM can go from idea to working prototype in an afternoon, the gap between "what if we tried…" and "here, try this" nearly disappears. At Anthropic this isn't limited to product — data science, finance, marketing, legal, and design teams picked up the tools on their own, and the whole org moves at the same speed instead of waiting on handoffs.

## Key Takeaways

- The PM playbook's assumption that the frontier is stable over a project lifetime is broken. Plan in short sprints, not quarterly roadmaps.
- **Side quests** replace pre-roadmap research — everyone on the team runs short self-directed experiments.
- **Demos and evals over docs.** Prototype-first because wrong bets are cheap in an afternoon. Send your spec to Claude Code and see if it can build it.
- **Every model release is an implicit prompt to revisit existing features.** Be a daily active user who asks the model to do things you think are too hard.
- **Optimize for capability first when prototyping** — use more tokens than you think you need. Cost comes down later; capability has to be proven first.
- **"Do the simple thing that works"** — workarounds for model limits become obsolete complexity. Todo-list reminders, heavy system prompts, and 20% of Opus-4.6's prompt all got removed as models improved.
- The PM role compresses to: identify non-negotiables, create clarity in ambiguity, push the team bigger, clear the path to ship.
- Cross-links: [[ai-organization/from-hierarchy-to-intelligence|from-hierarchy-to-intelligence]], [[ai-organization/claude-code-productivity-takeaways|claude-code-productivity-takeaways]], [[ai-organization/product-job-market-2026|product-job-market-2026]].

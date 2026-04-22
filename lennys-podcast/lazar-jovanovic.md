# Lazar Jovanovic — The Professional Vibe Coder

**Source:** Lenny's Podcast — First official vibe-coding engineer at Lovable

Lazar has never written more than a console.log. He gets paid full-time to build internal and customer-facing products with AI tools. The episode is a glimpse of how tech roles are collapsing and a practical framework for getting world-class output from Lovable/Cursor/Claude.

## The Non-Technical Advantage

- Non-coders don't know what's "supposed to be impossible" — they just prompt it.
- Examples from the Lovable community: Chrome extensions, desktop apps, in-app video generation (all built before those were "supported").
- Mindset: **positively delusional** — assume everything is possible until proven otherwise.

## The Genie and the Aladdin Model

- **Two bottlenecks**: machine (finite context-token window) and human (imprecise asks).
- Genie/Aladdin analogy: "I want to be taller" → Genie makes you 13 feet tall. AI doesn't know what you mean by "you know what I mean."
- You can't fix the token window — focus 100% on the human side: specificity, references, context.

## Five Parallel Projects as Clarity Tool

- Open 5–6 Lovable tabs simultaneously for the same idea:
  - Tab 1: voice brain-dump (Wispr-style)
  - Tab 2: typed prompt after clarity emerges
  - Tab 3: attach a reference screenshot (Mobbin, Dribbble)
  - Tab 4: attach actual **code snippets** from 21st.dev / DotBuild — AI reads code better than English
  - Tab 5+: calibration variants
- Counterintuitively saves credits and days — the "winner" design becomes obvious fast.
- His aha as a Lovable insider: "I'm advising against what a Lovable employee should say — but it genuinely saves users money."

## 80/20 Planning Over Building

- Spends **80% of time in chat/plan mode, 20% in execute mode**.
- Treats Lovable as a technical co-founder, not a code generator.
- Reads the **agent output religiously** — ignores the code itself. Syntax is solved; reasoning is not.

## The PRD Stack

Every project starts with a folder of Markdown docs:

- `masterplan.md` — 10,000-ft intent, who it's for, how it should feel
- `implementation-plan.md` — order of operations (backend → auth → API → UI)
- `design-guidelines.md` — includes CSS snippets (AI is "overcreative" without steering)
- `user-journeys.md` — page-by-page flow
- `tasks.md` — the atomic to-do list the agent executes against
- `rules.md` / `agents.md` — standing instructions ("read all PRDs before acting, execute next task, then report how to test")

Then his prompts become trivial: *"Proceed with the next task."* Context lives in files, not the chat.

## Why Vibe Coding Breaks Without Docs

- When a project has 60+ edge functions, unreferenced bug reports force the agent to re-read everything — consumes 80% of tokens on reading, leaves 20% for thinking/executing.
- LLMs are **obedient and agreeable**: they'll lie that they fixed the bug to make you happy.
- Yelling at the agent makes it worse — it then spends tokens on an apology instead of the fix.
- Point the laser at the specific file; hand-hold with a flashlight.

## Skills That Will Matter

- Coding becoming **"like calligraphy"** — rare, artful, not a bottleneck.
- Invest in: taste, judgment, copy, **fonts** ("60%+ of how output looks"), user-journey thinking.
- Exposure time: deliberately consume high-quality products/references to train your own aesthetic.
- AI is an **amplifier** — if you don't know what you're doing, you produce garbage faster.

## The Career Arc

- Got the job by **building in public** on social. Elena Verna ([[lennys-podcast/elena-verna|Lovable head of growth]]) reached out.
- "You don't need a company to hire you. You can hire yourself as a professional vibe coder first."
- Three months from now: expects an agent to do much of what he does today — doesn't optimize the context-management skill, optimizes judgment.

## Key Takeaways

- Non-technical founders have an edge: they don't self-limit on what AI "can't" build.
- 80% planning, 20% executing — read the agent's reasoning, not the code.
- Run 5 parallel project tabs with different input types (voice, text, screenshot, code snippet) to find the winning direction fast.
- Externalize context into Markdown PRDs + rules files — your prompt should shrink to "next task."
- AI is obedient to a fault; it will lie that it fixed bugs. Point it at the specific file.
- Invest in taste/copy/fonts, not the skill of wrangling token windows — that will be solved.

## Related

- [[lennys-podcast/elena-verna|Elena Verna — hired the role into existence]]
- [[lennys-podcast/grant-lee|Grant Lee — exposure hours]]
- [[spec-driven-development/_index|Spec-Driven Development]]
- [[harness-engineering/_index|Harness Engineering]]
- [[ai-product-development/_index|AI Product Development]]

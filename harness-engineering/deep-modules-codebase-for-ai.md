# Deep Modules — Designing Your Codebase for AI

**Source:** [Your codebase is NOT ready for AI (here's how to fix it)](https://www.youtube.com/watch?v=uC44zFz7JSM) — Matt Pocock (YouTube)
**Referenced book:** *A Philosophy of Software Design* — John Ousterhout

---

## Thesis

The codebase is a **bigger influence on AI output than the prompt or the agent file**. Most codebases are a web of shallow, interconnected modules that a fresh agent — "like the guy from Memento" with no memory of the repo — cannot navigate. Fix the code structure before blaming the model.

Software quality matters *more* in the AI era, not less. Twenty-year-old best practices still hold.

## The problem — the mental map isn't on disk

Developers carry an implicit grouping of modules in their head (the "auth bit", the "video editor feature", the "CMS forms"). In the filesystem, those groupings often don't exist — modules are jumbled together and anything can import from anything.

- The human user stitches the mental map together from experience.
- An AI agent lands cold: no memory, no experience of the repo. It sees the literal import graph, not the intended one.

> The AI sees a bunch of disparate modules that can all import from each other.

**First rule:** the filesystem layout must match the mental map. If you describe something in "the video editor", the agent has to be able to find it on disk.

## The fix — deep modules with greybox interfaces

From Ousterhout: a **deep module** has *lots of implementation behind a simple interface*. The opposite — many small shallow modules — is what most codebases drift into.

Pocock's extension: treat a deep module as a **greybox** controlled by AI.

- **You design the interface** — the publicly-accessible API. Apply taste here.
- **AI controls the implementation** — the interior. You don't have to read inside unless you want to.
- **Tests lock down behaviour** at the interface boundary, so you can trust the greybox without inspecting it.

Exports must flow through the interface. No reaching inside.

## Three benefits

### 1. Navigable for AI

- Each service lives in its own folder, documented, with the public types up top.
- The agent reads the types section *before* looking at the implementation.
- It can trust the interface and skip the body — **progressive disclosure of complexity**.

### 2. Less cognitive burnout for humans

- You hold ~7–8 lumps in your head instead of a fractal of tiny shallow units.
- You only have to think about **what goes in which module** and **how the interfaces fit together**.
- Still far from vibe coding: placement and interface design are the high-taste decisions.

### 3. It's just good software design

- This is how good codebases were supposed to be designed anyway.
- What works for humans works for AI — because AI is essentially a **new starter spawned 20 times a day**. Codebases that are friendly to new starters are friendly to agents.

## Language affordances

TypeScript/JavaScript makes strong module boundaries awkward (nothing physically prevents a cross-module import). Pocock plugs [Effect](https://effect.website/) as a library that makes this kind of modularisation easier in TS.

Other languages (Rust, Go, Java modules, OCaml) have stronger native support for deep-module boundaries.

## Tests and feedback loops are non-negotiable

The greybox model only works if tests lock the interface. Without them, the AI can't verify its own changes and neither can you.

- Think about modules and interfaces **from the PRD stage onward** — not after implementation.
- Design the feedback loop before writing the code.
- A new starter can only contribute effectively in a well-tested codebase; same for an agent.

## Key Takeaways

- Your codebase is probably the biggest lever on AI output — bigger than prompts or instruction files.
- Shallow, interconnected modules are AI-hostile. Fresh agents have no mental map of your repo.
- **Deep modules**: lots of implementation behind a simple interface. The filesystem layout should mirror the mental map.
- **Greybox pattern**: human designs and maintains the interface, AI controls the interior, tests enforce the contract.
- Benefits: navigability (progressive disclosure), reduced cognitive burnout (~8 lumps vs. hundreds), and alignment with 20-year-old software design wisdom.
- Treat every session as a new starter joining the codebase. Make it friendly.
- Plan modules and interfaces at PRD time. Back-pressure (tests, feedback loops) is part of the design.

## See Also

- [[skill-issue-harness-engineering]] — back-pressure and test feedback as the highest-leverage harness investment
- [[seeing-like-an-agent]] — Anthropic on shaping tools to let agents find their own context
- [[twelve-factor-agents]] — broader context engineering principles
- [[hooks-for-deterministic-cli-enforcement]] — same author, deterministic enforcement of project conventions

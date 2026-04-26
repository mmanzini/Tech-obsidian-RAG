# Zevi Arnovitz — The non-technical PM who ships code

Source: [[lennys-podcast]] interview with Zevi Arnovitz (PM, ex-Wix, now at Meta).

Arnovitz is a non-technical PM who ships real revenue-generating products himself using Cursor + Claude Code, multi-model peer review, and a disciplined /command library. A working playbook for the "PM who builds" role.

## Tool progression
- ChatGPT Projects (thought partner, structured context)
- Bolt / Lovable (demo-grade UI prototyping)
- Cursor + Claude Code (real shippable product)
- Each step added rigor rather than replaced the prior one
- See [[ai-dev-tools]]

## The CTO persona and Claude.md as system prompt
- Configures Claude Code to behave as a senior dev lead, not a compliant junior
- Claude.md holds the system prompt: house rules, architecture, tone, red lines
- Treats the AI as an opinionated colleague — expects pushback when he's wrong

## The /command library
- `/create-issue` — translate fuzzy intent into a crisp ticket
- `/exploration-phase` — map the codebase before editing
- `/create-plan` → `/execute-plan` — separate planning from doing
- `/review` — self-review the change
- `/peer-review` — feed competing models' reviews back in as if from other team leads
- `/update-docs` — document every change so future agents have context
- `/learning-opportunity` — teach him something he doesn't understand (used continuously)
- See [[agent-workflows]] and [[spec-driven-development]]

## Multi-model peer review
- Runs the same change through Claude, Codex (GPT), and Composer/Gemini
- Models have distinct personalities: Claude = communicative CTO; Codex = hoodie-in-dark-room specialist; Gemini = scary-brilliant designer
- Paste each model's findings back to the lead agent as "dev lead 1/2" — force them to defend or fix
- Non-technical reviewer gets expert-grade coverage by arbitraging model differences

## Postmortem every mistake into the prompt
- When the agent gets something wrong, ask it to introspect on what in its prompt/tooling caused the mistake
- Update `/commands`, docs, or tooling so the error cannot recur
- Compounding prompt quality is Arnovitz's core productivity hack
- See [[way-of-working]]

## Interview prep as an AI-native default
- For the Meta interview: built a coach project in Claude, a segmentation quiz in Base44, ran Comet/Perplexity over a public question bank to prioritise mocks
- Still insists human mocks are irreplaceable for Meta-tier rigor
- "You won't be replaced by AI — you'll be replaced by someone better at using AI than you"

## Key Takeaways
- Non-technical PMs can ship real products today with Cursor + Claude Code + a disciplined /command library
- Treat the AI as a CTO persona via Claude.md; expect opinionated pushback
- Multi-model peer review (Claude + Codex + Composer) gives non-technical reviewers expert-grade coverage
- Every mistake becomes a prompt/doc update — compounding quality
- For interviews or new skills, AI-first is now the default move, not a shortcut

## Related
- [[ai-dev-tools/_index|AI Dev Tools]]
- [[agent-workflows/_index|Agent Workflows]]
- [[spec-driven-development/_index|Spec-Driven Development]]
- [[way-of-working/_index|Way of Working]]
- [[product-management/_index|Product Management]]
- [[lennys-podcast/_index|Lenny's Podcast]]

# Dhanji Prasanna — rm -rf and Rebuild Every Release

**Source:** Lenny's Podcast, Dhanji R. Prasanna (CTO, Block; previously Google; creator of Goose open-source agent)

Dhanji wrote an AI manifesto to Jack Dorsey arguing that Block's GM-based structure was wrong for an AI-native company, and got the mandate to rebuild. The result: a functional re-org modeled on Apple, 8–10 hours/week saved per engineer, a 20–25% cut in manual hours, and Goose — an open-source MCP-based agent named after the Top Gun wingman.

## The AI manifesto to Jack
- Dhanji's argument to Dorsey: a GM org (one GM per product line) is an industrial-era fit; AI-native companies need **functional depth** like Apple's single-P&L model.
- Conway's Law in reverse: if you want AI-native products, your org chart has to stop mirroring 2010-era product silos.
- Block's re-org followed — functional leaders for design, engineering, AI — not GM-per-product.
- See [[ai-organization/_index|AI Organization]].

## Goose — the wingman agent
- Goose is Block's open-source MCP-based agent; name is deliberately Top Gun (Maverick's wingman, not a replacement pilot).
- Design principle: **Goose watches your screen and acts with you**, not instead of you.
- Example workflow: engineer reading Slack, Goose drafts a PR from the conversation in the background, hands it back for review.
- Gosling = the mobile-first variant.

## Conway's Law as a product diagnostic
- If your AI product feels disjointed, check the org — almost always the seams in the product map to the seams in the reporting lines.
- Block's feed product was stuck; Dhanji's fix included carving out a 2M-member segment with its own functional team to get around the GM seams.
- Re-orging around AI isn't cosmetic; it's the prerequisite to shipping AI-native experiences.

## Measurable hours back
- Internal instrumentation after rollout: 8–10 hours/week saved per engineer on average.
- 20–25% of *manual* hours (not total) cut in roles with heavy routine work — Dhanji is careful to separate that from productivity theatre.
- The metric that matters isn't LOC produced; it's hours of human cognition freed for non-routine judgment.

## "rm -rf and rebuild every release"
- Dhanji's provocation: in an AI-native world you should be able to regenerate most of a product from scratch each release from specs + agents.
- The durable artifact is the spec and the test suite, not the codebase.
- Connects to the [[spec-driven-development/_index|Spec-Driven Development]] thesis and Simon Willison's "code is cheap now."
- Anti-pattern: treating the codebase as the crown jewels when agents can regenerate it faithfully from spec.

## MCP-native from day one
- Goose is built on the Model Context Protocol rather than bespoke plugins so every new integration extends every agent.
- Block's internal MCP catalogue includes everything from Square admin tools to Slack/Jira — all accessible from the same Goose.
- See [[harness-engineering/_index|Harness Engineering]] on MCP as the right abstraction layer.

## Open source as talent + signal strategy
- Goose is intentionally open-source to (a) attract AI-native engineers, (b) force Block's internal tools to meet public-quality bars, and (c) get external contributors to harden the agent faster than a closed team could.

## Key Takeaways
- GM orgs are wrong for AI-native companies; go functional, Apple-style.
- Conway's Law is a live diagnostic — org seams become product seams.
- Goose is a wingman, not a replacement — agent watches screen, drafts work, human approves.
- 8–10 hours/week per engineer and 20–25% manual-hour cut are the numbers Block stands behind.
- "rm -rf and rebuild each release" — specs and tests are durable, code is disposable.
- MCP is the right abstraction layer; open-source is a talent magnet and quality-forcing function.

## Related
- [[ai-organization/_index|AI Organization]] — functional vs. GM structure, Conway's Law
- [[harness-engineering/_index|Harness Engineering]] — MCP, Goose
- [[agent-architecture/_index|Agent Architecture]] — screen-watching agents
- [[spec-driven-development/_index|Spec-Driven Development]] — specs as the durable artifact
- [[way-of-working/_index|Way of Working]] — hours-back metric
- [[lennys-podcast/_index|Lenny's Podcast]]

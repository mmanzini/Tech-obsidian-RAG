---
type: synthesis
title: Dianne Penn — Why the People Building AI Can't Tell You What's Next
description: Anthropic's Head of Product for AI Research and Labs on emerging-capability unpredictability ("evals are the new PRDs"), the coding bet behind Opus 3, frontier-products-for-frontier-models, hands-on leadership ("sweat the tokens"), and why Claude's willingness to push back makes it a better thinking partner.
bundle: ai-engineering
topic: product-leader-interviews
tags: [practitioner-interview, evals, product-management, constitutional-ai]
source: Resources/transcriptions/transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md
resource: https://www.youtube.com/watch?v=tivaWTTVRhY
timestamp: 2026-08-05T09:00:00Z
status: active
related:
  - ai-engineering/product-leader-interviews/cat-wu.md
  - ai-engineering/product-leader-interviews/elizabeth-stone.md
  - ai-engineering/ai-organization/anthropic-growth-takeaways.md
  - ai-engineering/model-fundamentals/compression-is-intelligence.md
  - ai-engineering/model-evaluation/index.md
---

# Dianne Penn — Why the People Building AI Can't Tell You What's Next

**Source:** Lenny's Podcast, Dianne Penn (Head of Product, AI Research and Labs, Anthropic) — [episode](https://www.youtube.com/watch?v=tivaWTTVRhY)

Penn joined Anthropic in 2023 as its first technical PM when the product team was five engineers (one engineer for the entire API business); she has shipped every model from Claude 2 through Fable and helped incubate Claude Code, MCP, Skills, computer use, tool use, and reasoning. Before Anthropic: Alexa AI at Amazon; before that, high-yield bond trading at JP Morgan (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Summary

Penn's throughline: capability jumps are emergent and discontinuous even as scaling smoothly lowers loss, so nobody — including the labs — knows the exact moment things become possible; the response is adaptability, first-principles reasoning, and evals as the instrument that detects the jumps (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md). Her team's saying — "evals are the new PRDs" — reframes the research-PM job as converting vague user feedback into on-distribution eval sets researchers can act on. Frontier products and frontier models need each other: Opus 4.5 wouldn't have had its moment without Claude Code, and vice versa (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Early Anthropic and the inflections

- Golden Gate Claude (early 2024, from interpretability research on model "features") was spun onto claude.ai within 24 hours and reached maybe 2,000 people — but proved Anthropic could ship authentic research-driven experiences at startup pace; a hidden identity-forming inflection (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Opus 3 (March 2024, company under 200 people): the commitment to build a frontier model, forging the cross-team trust that still underpins production-model work (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- **The coding bet**: in 2023 nobody said Anthropic, Claude, and coding in the same sentence. Penn noticed people using models for long-form code (not just autocomplete) and pushed to train Opus 3 for it — a relatively small training change that differentiated Anthropic and attracted the early developer base (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Opus 4.5 × Claude Code: "you need frontier products in order for people to feel the magic of frontier models" — model and vehicle each required the other for the adoption moment (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Inside the exponential

- Scaling-law papers show smooth loss curves *and* discontinuous emerging-capability graphs — models jump from can't-calculate to reliably-calculates. Without the eval you don't know the jump happened; this is also what makes safety hard (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- There is "product overhang and user overhang" on today's models — much unexplored even on current Opus and Fable (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- On Garry Tan's token-maxxing ($100K/year in tokens = living in 2028): Penn reframes token spend as the input; the output to orient goals around is **experimentation** — though the best internal prototypers do spend heavily on every research model, and "it's very hard to come up with a perfect strategy without touching the technology" (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Experimentation is not an individual sport: Anthropic's early all-company Slack channel of public Claude testing produced communal use-case discovery within ~10 requests of an idea being shared (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Forward-compatibility question she asks the team: "let's say Claude 8 comes around — what changes in what users do, and what does that mean for how you're building today?" Be stubborn about the area, loose about the exact approach (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Labs and the incubation model

- Labs' thesis: pull the thread on discontinuous bets outside the core roadmap (Claude Code, Skills, Claude Design, MCP) and find the 10x/100x/1000x of the "there there" (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- What makes it work: small pods (ideas often start with one engineer), founder-type people selected for zero-to-one appetite, strongly-held opinions on the theme with weakly-held prototypes, and willingness to shelve bets for one to two model generations (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Evals are the new PRDs

- The research-PM loop: vague feedback ("Claude hallucinated") is not actionable; the team reads consented transcripts deeply to classify the failure — tool-use miss, search synthesis, alignment — and shortens the distance to actionability for researchers (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md). "You have to sweat the tokens as much as you sweat the pixels."
- Canonical example: early "Claude doesn't follow instructions" feedback was ~80% JSON-schema failures; 30–40 reproduced examples became an eval set run on every Claude version — now permanently ~100%, pain point retired (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- PRDs are not dead: still used per model as a source of truth aligning product surfaces, engineering, legal, and safety, and for ambiguous zero-to-one opportunities (e.g. computer use) where vision matters before user pain points exist (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Hiring loop unchanged in three years; the trait is first-principles thinking — "figure out what I should do to achieve my goals, rather than continue the activities I've always done" (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Leadership, joy, and judgment

- Managers must be hands-on shippers: tenured hires get the same onboarding as early-career; Penn keeps one to two model work streams herself to "keep my theory of mind" of how fast models move (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Finding joy: pair with someone excited rather than hunting the perfect use case alone; go deep on one or two prototypes rather than shallow on many (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- EQ augmentation: she keeps a Crucial-Conversations-based skill for prepping difficult conversations and coaches her managers to use Claude as a coaching aid (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Avoiding brain rot: form your own POV first, then use Claude as sparring partner; for low-thinking-value artifacts (monthly business reviews) delegate fully and become the reviewer/verifier — "who's verifying the output" matters more than who wrote it (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Why alignment work makes Claude *better*: a thinking partner that just agrees adds nothing — knowing when to push back is integral to capability and proactivity; the hero goal is coming away with better ideas, not 10%-better ones (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- AI writing is a jagged edge under active training investment; safeguards evolve with capability (fallback UXs since the Fable models; the "model safeguards package"), with the goal of keeping general-purpose access inclusive (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Durable human ground: hard-earned judgment (which of the many buildable things to build), persistence, proactivity, and subject-matter expertise in domains still at the foot of the exponential (biology, life sciences) (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).
- Sustainability: 2024 shipped four model series; Q2 2026 alone exceeded that. What prevents burnout is the "hive mind" team culture — radical ownership, low-ego hiring, mind-melding so PTO actually replenishes (source: transcript-why-the-people-building-ai-cant-tell-you-whats-next-dianne-penn-anthropic.md).

## Key Takeaways

- Capability emergence is discontinuous and eval-detected — adaptability and first-principles reasoning beat fixed plans inside the exponential.
- "Evals are the new PRDs": convert consented-transcript failure analysis into on-distribution eval sets; 30–40 good examples can define and eventually retire a pain point.
- Frontier products and frontier models are mutually necessary (Opus 4.5 ⇄ Claude Code).
- Token spend is the input; experimentation — communal, not solo — is the outcome to manage.
- Leaders must ship hands-on to keep a theory of mind of the models; protect your own thinking by bringing a POV before consulting Claude.
- Claude's trained willingness to push back is a capability feature, not a constraint.
- The PM core — user-centric detail work, bubbled up in actionable form — becomes more needed as building gets technology-driven, not less.

## Related

- [[cat-wu]] · [Cat Wu — Shipping Faster than Anyone Else at Anthropic](../product-leader-interviews/cat-wu.md) — the product-side twin: same "evals as the new PRD" doctrine from the Claude Code org
- [[elizabeth-stone]] · [Elizabeth Stone — Netflix CPTO](../product-leader-interviews/elizabeth-stone.md) — the application-company mirror: fluency overlays and systems thinking versus Penn's research-side adaptability
- [[anthropic-growth-takeaways]] · [Anthropic Growth Takeaways](../ai-organization/anthropic-growth-takeaways.md) — the growth-org view of the same company culture
- [[compression-is-intelligence]] · [Compression is Intelligence](../model-fundamentals/compression-is-intelligence.md) — the loss-curve mathematics behind the smooth-scaling / jumpy-capability distinction Penn describes
- [[../model-evaluation/index|Model Evaluation]] — the topic-level home of eval design methods Penn's team pioneered internally

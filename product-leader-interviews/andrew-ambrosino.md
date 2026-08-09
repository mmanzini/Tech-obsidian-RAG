---
type: synthesis
title: Andrew Ambrosino — The New Shape of Product Work
description: Andrew Ambrosino (Codex desktop lead, OpenAI) on the inverted product process — implementation is cheap, curation is expensive — taste as the emerging top skill, launch timing bound to model capability, zone-defense PM, role collapse without role elimination, and Codex as a home base for all work.
bundle: ai-engineering
topic: product-leader-interviews
tags:
- practitioner-interview
- product-management
- ai-org-design
- design-in-ai
- long-running-agents
resource: https://www.youtube.com/watch?v=P3KDebPTUrw
sources:
- id: transcript-openai-codex-lead-on-the-new-shape-of-product-work
  resource: Resources/transcriptions/transcript-openai-codex-lead-on-the-new-shape-of-product-work.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-07-05T07:20:00Z'
status: stable
related:
- ai-engineering/agent-architecture/alexander-embiricos.md
- ai-engineering/product-leader-interviews/dan-shipper-ai-paradox.md
- ai-engineering/product-leader-interviews/max-schoening-agency-ai-era.md
---

# Andrew Ambrosino — The New Shape of Product Work

**Source:** [Lenny's Podcast — "OpenAI Codex lead on the new shape of product work"](https://www.youtube.com/watch?v=P3KDebPTUrw)
**Guest:** Andrew Ambrosino (product & engineering lead, Codex desktop app, OpenAI; designer → engineer → PM → founder)

---

## Summary

Andrew Ambrosino leads the Codex desktop app at OpenAI, where nearly 100% of employees — not just engineers — use Codex weekly and usage has grown 6x since January to 5M+ weekly actives (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). His core thesis: AI has *inverted* the product process. Implementation used to be the expensive part that documents, research, and prototypes de-risked up front; now anyone can build anything, so the expensive part is taste — curating the 90 uncoordinated explorations of the same feature into what should actually ship (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

## The flipped product process

The old process assumed implementation was scarce, so everything upstream (PRDs, research, prototypes) existed to de-risk it. With implementation abundant, the mediums have lost their built-in signal: a production-polished prototype used to mean "late in the process, de-risked"; now it may be one of 90 early explorations, and over-anchoring on it is the new failure mode — the "primal mark" problem (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Ambrosino rejects "PRDs are dead, prototypes are in": the skill now is picking the right medium per point — a document for product clarity around a vague area, a prototype for stress-testing an interaction pattern (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

The design process is likewise "both dead and alive": the tool-bound day-to-day specifics are dead, but the process *overlay* — knowing which stage you're at — matters more than ever. Teams keep a "baby Codex", a dramatically simplified codebase approximating the production app's interactions, to vibe-code design explorations quickly (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

## Taste as a professional skill

Taste is not just aesthetics (Paul Graham has great taste and wears cargo shorts): it's systems thinking (how does this fit, what theme is it part of), medium selection, framing, and only lastly the interaction-level polish. The real taste question: "if we can build anything, what's the goal here and how do we get there?" (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). AI remains bad at design partly for practical reasons (design is harder to grade than code — human taste is part of the feedback loop; labs invest in capabilities that accelerate AI research, and design isn't in that flywheel) and partly for harder ones: culture-bound novelty (a model that outputs Linear's website every time isn't designing) and the abstraction layer binding visual design to code semantics (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

## Launch timing: the Codex app would have failed in November

The Codex app released in February would "absolutely have failed" if launched in November — identical product shape, the only difference was the models in between (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Consequences for planning: list everything worth doing over 1–2 years, prototype all of it, ship what's ready, let the rest bake, and re-try each thing at every model leap — features fail because they're not smart enough yet, not because the shape is wrong. The original Codex web was "too AGI-pilled" for its moment; Claude Code's local, question-asking, less-autonomous shape matched where models actually were and won (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Corollary: build features that don't work yet as artifacts to test against future models — the operator → Atlas agent → in-app browser thread is fundamentally one feature re-released with different intelligence (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Nine-month plans stay deliberately hazy; precision at that horizon is false precision.

## Zone-defense PM and role collapse without elimination

Product people at OpenAI play zone defense: if two PMs are working too closely, that's a bad signal — spread out for company coverage, find the gaps, and fill them, steering chaos from inception to coherent product (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Roles have collapsed more on Codex than elsewhere (designers write code, PMs are technical), but Ambrosino warns against eliminating roles entirely: "we're getting rid of the product role and everybody's a builder" abandons a discipline with real best practices. A person's role is now *the average of where their work lands*, not the fence around it (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). IC vs manager also converges: ICs manage agents, managers manage at a different granularity; everyone needs command of a discipline plus the taste to separate signal from noise given unlimited tokens (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

## Loops, autonomous development, and computer use

"100% of code is AI-written" is yesterday's goalpost; the question now is supervised vs unsupervised. Fully autonomous development ("improve the app" loops listening to Twitter/Slack/email) isn't there yet — models increase complexity rather than delete code, and can't yet judge which feature requests to build, ignore, or reframe (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Ambrosino automates his own job through Codex: a morning daily brief distilled from ~3,000 Slack channels, coached conversationally ("next time this runs, de-emphasise this workstream") rather than by editing instructions; release management by gathering PR/Slack updates into a status tracker (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). Computer use closes the connector gap — Codex took over the Google Cloud console UI to set up Pub/Sub triggers when no connector existed. The team constantly triages which emergent personal workflows should become product primitives (memory) versus stay personal process (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

## Codex as home base

Internal dogfooding found clear PMF with engineers, but marketing, comms, finance, and legal kept using the "actively hostile" Codex app rather than leave for surfaces built for them — collapsing the developer-tool vs general-knowledge-work distinction (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md). The vision is a home base, not a super app: where you start, end, and automate work, while the app opens and drives the specialist tools you already use (talking to the Excel add-in rather than replacing Excel; a videographer's Codex built itself a Premiere Pro extension to control the editor). Two inverse models run in parallel: Codex reaching out into existing tools via connectors/computer use/extensions, and web apps being pulled inside Codex for the agent to operate on (source: transcript-openai-codex-lead-on-the-new-shape-of-product-work.md).

## Key Takeaways

- The product process is inverted: implementation is cheap, curation is expensive — taste (system fit, framing, medium choice, goal-setting) is the emerging top professional skill.
- PRDs aren't dead; medium selection is the skill. Beware the polished prototype as a false "late-stage" signal.
- Launch timing is bound to model capability: same product shape, different model, opposite outcome. Prototype everything, let non-working features bake, re-test at every model leap.
- Zone defense for PMs: spread for coverage, fill gaps, steer chaos — two PMs working closely together is an anti-pattern.
- Role collapse is real but role *elimination* is a trap: disciplines carry best practices; your role is the average of your work, not its boundary.
- Fully autonomous dev loops are blocked on models that add complexity instead of deleting code and can't prioritise feature requests.
- Don't get married to your process — get married to the outcomes you're uniquely able to deliver.

## Related

- [[alexander-embiricos|Alexander Embiricos — Codex Is a Teammate, Not a Tool]] · [Alexander Embiricos](../agent-architecture/alexander-embiricos.md) — the Codex product lead's companion episode: async delegation, three-layer stack; Ambrosino's zone-defense partner
- [[dan-shipper-ai-paradox|Dan Shipper — The AI Paradox]] · [Dan Shipper](../product-leader-interviews/dan-shipper-ai-paradox.md) — predicts the coding-OS surface Codex desktop is becoming; Shipper's SaaS-inside-Codex thesis is discussed in this episode
- [[max-schoening-agency-ai-era|Max Schoening — Agency, Malleable Software, and the Tiny Core]] · [Max Schoening](../product-leader-interviews/max-schoening-agency-ai-era.md) — parallel take on taste as a trainable skill and agency as the AI-era differentiator

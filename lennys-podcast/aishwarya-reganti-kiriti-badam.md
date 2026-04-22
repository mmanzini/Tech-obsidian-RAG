# Aishwarya Naresh Reganti & Kiriti Badam — Building AI Products When the System Is Non-Deterministic

**Source:** Lenny's Podcast — AI product leaders (ex-AWS, ex-Meta)

Aishwarya and Kiriti argue that AI product development fails when teams apply deterministic-era playbooks to non-deterministic systems. Their operating model: a V1→V2→V3 progression, the agency-control tradeoff, and a CCCD framework that assumes the product is never "done."

## Non-Determinism Makes Three Things Opaque

- **Input** is opaque: users phrase requests in infinite ways, and you can't enumerate them.
- **Output** is opaque: probabilistic generation changes across calls and model versions.
- **Process** is opaque: the model's reasoning path is not directly inspectable.
- Consequence: traditional PRDs, acceptance criteria, and QA all break. You cannot spec your way out.

## The Agency-Control Tradeoff

- Every AI feature sits on a spectrum from **low agency, high control** to **high agency, low control**.
- Low agency: suggestions the user accepts/rejects (Copilot).
- High agency: the system takes irreversible action on the user's behalf (autonomous resolution).
- More agency = more value when it works, more catastrophic when it fails.
- Choose consciously per feature; don't let "agentic" become a one-way ratchet.

## V1 → V2 → V3 Progression (Support Example)

- **V1:** system **suggests** a response; human sends.
- **V2:** system **drafts** the full response; human edits.
- **V3:** system **resolves** end-to-end; human audits a sample.
- Ship V1 before V2 before V3 — each step needs different evals, guardrails, and UX.
- Jumping to V3 destroys trust; users stop engaging with the product at all.

## CCCD — Continuous Calibration, Continuous Development

- Standard CI/CD assumes deterministic code. AI needs **CCCD**: the model's behaviour drifts under you.
- Continuous calibration: rerun your eval suite on every model/prompt change, not just every code change.
- Continuous development: treat prompts, eval sets, and guardrails as first-class source code.
- "The product is never done" — the model updates, the users' mental models update, the domain shifts.

## Evals AND Production Monitoring — Both Required

- Offline evals: curated datasets that regression-test known failure modes before release.
- Production monitoring: trace-level logging, anomaly detection, sampled human review.
- Teams that skip one always pay: eval-only = miss live edge cases; prod-only = break on every push.
- Budget: **eval and monitoring cost 20-30% of AI engineering headcount** at scale.

## Problem-First, Not Agent-First

- Start with the user problem and the current failure mode; choose AI shape last.
- "Multi-agent" is the most misunderstood term — most are orchestrated single-agent pipelines.
- "One-click agent" demos are pure marketing; real systems have dozens of guardrails.
- Rackspace CEO anecdote: up at 4-6AM "catching up on AI" — every exec is anxious, use that to build, not to ship theater.

## "Pain Is the New Moat"

- In the AI era, copying a feature takes weeks. The moat is the **pain tolerance** to keep iterating past V2 into V3.
- Domain expertise + willingness to debug non-determinism for 18 months = defensible.
- Capital and model access no longer differentiate — operator endurance does.

## Key Takeaways

- Accept non-determinism: input, output, and process are all opaque — don't pretend otherwise.
- Place each feature deliberately on the agency-control spectrum.
- Ship V1→V2→V3, each with its own evals and UX; don't leap to full autonomy.
- CCCD replaces CI/CD — prompts, evals, and guardrails are source code.
- You need both offline evals and production monitoring; skipping either burns you.
- Multi-agent is marketing; problem-first pipelines win.
- Operator pain tolerance is the durable moat.

## Related

- [[ai-product-development/_index|AI Product Development]]
- [[harness-engineering/_index|Harness Engineering]]
- [[agent-architecture/_index|Agent Architecture]]
- [[lennys-podcast/_index|Lenny's Podcast]]

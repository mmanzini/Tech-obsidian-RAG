# A PM's Complete Guide to Evals

**Source:** Aman Khan (Director of Product at Arize AI), guest post on [Lenny's Newsletter](https://www.lennysnewsletter.com/) — *Beyond vibe checks* (2025-04-08)

Evals are quietly becoming the defining skill for AI PMs. Prompts make headlines; evals decide whether a product thrives or dies.

## What evals are

Evals measure the quality and effectiveness of an AI system. Unlike unit tests (deterministic, pass/fail), evals deal with a non-deterministic system in a variable environment — more like a driving test than a train on tracks.

Three dimensions of the driving-test analogy: **awareness**, **decision-making**, **safety**.

## Three approaches

| Approach | Pros | Cons |
|---|---|---|
| **Human evals** (thumbs up/down, SME labels) | Directly tied to user | Sparse, low signal, expensive |
| **Code-based evals** (deterministic checks, e.g. "does this string appear?") | Cheap, fast, scalable | Weak signal for subjective/open-ended tasks |
| **LLM-as-judge** | Highly scalable, natural language, can generate explanations | Probabilistic; needs initial calibration against human labels |

LLM-as-judge is the sweet spot for PMs: prompts grade prompts. Use confidence scores or panels of judges for reliability.

## Standard eval criteria

- **Hallucination** — is the agent making things up vs. using provided context? (key for RAG)
- **Toxicity / tone** — is the output harmful or off-brand?
- **Overall correctness** — end-to-end success at the primary goal
- **Code generation, summarization quality, retrieval relevance** — off-the-shelf evaluators exist (Phoenix, Ragas)

## The four-part eval formula

1. **Set the role.** *"You are a judge evaluating written text."*
2. **Provide the context.** `{text}` variable — the agent output to grade.
3. **Provide the goal.** What exactly are you measuring? Be precise.
4. **Define the terminology and label.** "Toxic", "friendly", "helpful" mean different things. Ground the judge in your definition.

## Workflow: collection → first-pass → iteration → monitoring

### Phase 1 — Collection
- Capture real user interactions (thumbs, analytics, manual inspection)
- Document edge cases; balance by topic
- Build a representative dataset (10–100 examples with human ground truth). Spreadsheets to start, Phoenix/open-source when it outgrows that.

### Phase 2 — First-pass
- Write eval prompt following the four-part formula
- Run against dataset; target **≥90% agreement** with human labels
- Identify failure patterns; adjust the prompt

### Phase 3 — Iteration
- Refine prompt (few-shot examples of "good"/"bad" grades help)
- Expand dataset with new edge cases
- Use evals as the final boss of A/B-testing agent prompts or model swaps (e.g. GPT-4o → Claude Sonnet): rerun old dataset through new agent, grade with your eval

### Phase 4 — Production
- Run evals automatically on live traffic for trend metrics ("are users getting more frustrated over time?")
- Compare eval results to real user feedback; catch divergence
- Turn eval scores into stakeholder-facing dashboards and leading indicators

## Common mistakes

- Going complex too fast — noisy signal → team loses trust. Start with specific, narrow outputs.
- Skipping edge cases and few-shot grounding.
- Not validating the eval itself against real user feedback — you're not just testing code, you're validating whether the AI solves user problems.

## Where to start (this week)

1. Pick one critical feature (e.g. hallucination detection for a RAG chatbot).
2. Write a simple eval prompt.
3. Run it on 5–10 representative interactions.
4. Iterate the prompt until accuracy improves.

## Key Takeaways

- Evals are **unit tests for non-deterministic systems** — qualitative, LLM-graded, calibrated against humans.
- **LLM-as-judge** is the scalable default; PMs can write judge prompts directly.
- The four-part formula — role, context, goal, terminology — turns vibe checks into measurable signal.
- Evals are how you compare models, prompts, and system changes rigorously; without them, every change is faith-based.
- Start narrow (hallucination on one feature), hit 90% agreement with humans, then scale.

## Related

- [[product-management/ai-prototyping-for-pms|AI prototyping for PMs]] — build the prototype, then measure it with evals
- [[ai-organization/an-ai-glossary|AI glossary]] — RAG, RLHF, hallucination definitions
- [[harness-engineering/skill-creator-evals|skill-creator evals]] — Anthropic's eval-driven loop for skills
- [[ai-product-development/ai-product-development-framework|AI Product Development Framework]]
- [[lennys-podcast/hamel-husain-shreya-shankar|Hamel Husain & Shreya Shankar]] — 100 stratified traces → open/axial coding; the taxonomy-first method for evals
- [[lennys-podcast/brendan-foody|Brendan Foody]] — "the eval IS the PRD"; expert-graded evals as the scarce input
- [[lennys-podcast/aishwarya-reganti-kiriti-badam|Aishwarya Reganti & Kiriti Badam]] — CCCD (continuous calibration + development) loop

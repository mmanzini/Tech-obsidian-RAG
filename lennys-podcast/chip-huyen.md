# Chip Huyen — Talk to Users, Not Benchmarks

**Source:** Lenny's Podcast, Chip Huyen (Nvidia, Stanford, ex-Netflix; author of *AI Engineering*)

Chip Huyen's viral insight: the things people *think* improve AI apps (new frameworks, newer models, vector-DB debates) are almost never what *actually* improves them. Real gains come from talking to users, preparing data, and writing better prompts. Post-training is the new frontier; pre-training is basically done.

## Perceived vs. actual AI app improvements
- What people *think* works: staying on AI news, newest agent framework, vector-DB choice, fine-tuning.
- What *actually* works: talking to users, reliable platforms, better data prep, end-to-end workflow optimization, better prompts.
- Decision rule for any new tool or technique: (1) how much does optimal vs. non-optimal affect performance? (2) how costly is switching out later? Most framework debates fail both tests.

## Pre-training vs. post-training vs. fine-tuning
- Pre-training = encoding statistical structure of language at scale. Frontier labs have largely maxed out text data; the *big* pre-training jumps (GPT-2 → 3 → 4) are probably behind us.
- Post-training is where the action is: supervised fine-tuning, distillation, reinforcement learning (human / AI / verifiable rewards).
- RLHF uses *comparison* (A vs. B) rather than absolute scoring because humans are bad at scoring but good at preferring.

## Evals: pragmatic, not religious
- Eval is ROI work. If the expected gain is 80 → 82%, and two engineers could instead launch a new feature, don't do the eval.
- Do evals rigorously when (a) you operate at scale, (b) failure is catastrophic, or (c) eval *quality* is your competitive edge.
- Goal of eval isn't to hit a number — it's to guide product development by surfacing where the product is failing.
- For complex flows (deep research, agents), evaluate *every step*, not just end-to-end. Search query diversity, retrieval breadth and depth, then synthesis.

## Data prep is the RAG moat
- RAG quality gains come from data preparation, not from vector-DB selection.
- Techniques: right chunk size; contextual metadata; hypothetical-question generation per chunk; rewrite source material into Q&A format; add annotation layers written *for AI consumption* (humans have common sense that AI lacks).

## The GenAI inside-company split
- Two tool categories inside enterprises:
  - **Internal productivity** — chatbots, coding tools, internal-knowledge RAG.
  - **Customer/partner facing** — support bots, booking bots, sales bots.
- External tools get adopted when outcomes are measurable (conversion rate lift). Internal tools stall because productivity is hard to measure.
- Manager test: ask a line manager if they'd take Cursor subscriptions *or* one more headcount. They pick headcount. Ask a VP the same question — they pick the subscription. Productivity framing changes with scope.

## Cursor randomized trial (unexpected result)
- Friend's 30–40-person eng team split into three buckets (high/avg/low performers), each randomly half-assigned Cursor.
- Biggest productivity lift: *senior* engineers (they know how to decompose problems).
- But other companies report the opposite — seniors are the most *resistant* because their quality bar rejects AI output.
- No clean reconciliation yet; depends on team culture and code domain.

## System thinking as the durable skill
- Mehran Sahami (Stanford): CS isn't coding; CS is system thinking. Coding is a means.
- AI is good at local, well-defined tasks. It is *bad* at debugging across components, because the bug's cause is often in a different component than the one it keeps editing (Chip's real example: kept fixing code, real issue was tier/permissions).
- Org implication: companies are restructuring — fewer senior engineers, in heavier review/process roles, with AI and juniors producing code.

## AI engineer vs. ML engineer
- ML engineer builds models. AI engineer uses existing models to build products.
- Barrier to entry has collapsed; the addressable application space has expanded. Both expand at once.

## Idea crisis
- Despite infinite build tooling, most people don't know *what* to build.
- Chip's cure: for one week, write down what frustrates you. Build something against that list. Micro-tools over moonshots.

## Key Takeaways
- Most AI-app effort goes to the wrong layer. Users, data, and prompts dominate.
- Pre-training gains are flattening; post-training is where competitive advantage now lives.
- RAG quality is mostly data preparation — chunking, contextual metadata, AI-readable annotation.
- Evals are an ROI decision, not a virtue. Scale and stakes determine how hard you go.
- Productivity measurement is the real bottleneck on enterprise AI adoption.
- System thinking beats coding ability; debugging requires cross-component reasoning that AI still lacks.
- Build micro-tools for your own frustrations — it's how you exit the idea crisis.

## Related
- [[ai-product-development/_index|AI Product Development]] — RAG and evals
- [[harness-engineering/_index|Harness Engineering]] — Cursor adoption, senior/junior split
- [[ai-organization/_index|AI & Organisation Design]] — eval ownership across functions
- [[lennys-podcast/_index|Lenny's Podcast]]

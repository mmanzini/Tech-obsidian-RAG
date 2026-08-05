---
type: synthesis
title: Compression is Intelligence — Entropy, Cross-Entropy, and LLM Pre-Training
description: 3Blue1Brown's two-part derivation of information theory from the limits of compressing language — information, entropy, cross-entropy, KL divergence — and why prediction and compression are mathematically equivalent, making LLM pre-training reframable as building the most efficient text compressor.
bundle: ai-engineering
topic: model-fundamentals
tags: [model-fundamentals, information-theory, evals]
source: Resources/transcriptions/transcript-reinventing-entropy-compression-is-intelligence-part-1.md
resource: https://www.youtube.com/watch?v=l6DKRf-fAAM
timestamp: 2026-08-05T09:00:00Z
status: active
related:
  - ai-engineering/model-fundamentals/large-database-models.md
  - ai-engineering/claude-code-practice/claude-code-effort-and-model-selection.md
  - ai-engineering/product-leader-interviews/dianne-penn.md
---

# Compression is Intelligence — Entropy, Cross-Entropy, and LLM Pre-Training

**Source:** 3Blue1Brown, "Compression is Intelligence" trilogy, parts 1–2:
[Reinventing Entropy](https://www.youtube.com/watch?v=l6DKRf-fAAM) and [But what is cross-entropy?](https://www.youtube.com/watch?v=GlYgs6v2YfU)
**Author:** Grant Sanderson (3Blue1Brown)

---

## Summary

One of the conclusions of information theory is that prediction and compression are mathematically equivalent — two sides of the same coin — which means LLM pre-training with cross-entropy loss can be entirely reframed as creating the most efficient possible text compressor rather than as next-token prediction per se (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md). Part 1 rediscovers the definitions of information and entropy by asking about the limits of compressing language; part 2 derives cross-entropy — how well a code optimised for one distribution performs against another — and shows the choice of a logarithmic loss for LLMs is mathematically forced, not conventional (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md). The claim "compression is intelligence" stays squishy, but the safer version holds: the mathematical theory of compression is bizarrely relevant to AI (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md).

## Part 1 — Rediscovering information and entropy

- **The warm-up**: a robot receives four instructions (up ½, down ¼, left ⅛, right ⅛). A prefix-free code (no code word is a prefix of another) assigning 1/2/3/3 bits achieves 1.75 bits per instruction versus the naive 2 — and the fraction of code-word space each symbol consumes exactly matches its probability, the founding insight of information theory (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md).
- **Perfect compression looks like random noise**: if compressed output were distinguishable from coin flips, some messages would waste space; on the binary-string diagram, saving one bit for one message costs two bits elsewhere (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md).
- **Information** falls out: a message using n bits under perfect compression must have probability 2⁻ⁿ, so the information of an event is −log₂(p) — "how many times do you chop the space of possibilities in half". Unlikely messages carry high information; expected ones carry little. Information of a full message sums over symbols because logs turn the probability chain rule's products into sums (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md).
- **Entropy** H = Σ p·(−log₂ p) is the average information per symbol — the minimum bits per symbol any encoding can achieve, per Shannon's noiseless coding theorem (no code beats the limit; you can get arbitrarily close) (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md).
- **Language needed intelligent models from the start**: n-gram statistics break down for long strings, so Shannon probed human brains as black-box language models — first having his wife Betty guess next letters, then (in "Prediction and Entropy of Printed English", 1950) recording how many guesses interviewees needed. With 100+ letters of context he estimated English at about **one bit per character**. Estimating compressibility inevitably meant engaging with intelligent models of language — in the 2020s we went from interrogating such black boxes to designing them (source: transcript-reinventing-entropy-compression-is-intelligence-part-1.md).

## Part 2 — Cross-entropy

- **Definition by scenario**: if encoding stays optimised for distribution Q but symbols now arrive per distribution P, the average bits per symbol is Σ pᵢ·(−log₂ qᵢ) — the cross-entropy of Q relative to P. In the robot example, flipping the distribution while keeping the old code costs 2.625 bits/symbol vs the optimal 1.75 (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).
- **The one property to remember**: with P fixed and Q variable, cross-entropy is minimised exactly when Q = P, and that minimum equals the entropy of P. Order matters — it is asymmetric (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).
- **Language Trees and Zipping (2002)**: gzip-compressing document A alone versus A + a snippet of B approximates "how well does a compressor optimised for A handle B" — an empirical cross-entropy estimate good enough to recover the tree of language lineage and identify document authors, with no linguistics and no language models (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).

## Pre-training as compression

- Pre-training loss is the **average information per token from the model's perspective**: for each true next token, take −log of the probability the model assigned it, averaged over the training set. A model that follows what's going on is unsurprised (low loss); a confused model is surprised all the time (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md). (Natural log vs log₂ differs by a constant absorbed into the learning rate.)
- **Why logarithms are forced**: over many instances of a pattern ("my name is ___"), the average loss weighted by data frequencies P against model outputs Q is exactly the cross-entropy formula — and requiring that the total loss be minimised *only* when the model's distribution matches the data's statistics forces F to be a logarithm (via a Lagrange-multiplier argument: dF/dq must look like a constant over q, a property only logs have) (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).
- **Distillation** uses cross-entropy explicitly: the small model's per-token loss is the cross-entropy of its distribution against the large model's full distribution rather than a one-hot true token — like learning chess from someone talking through all the good moves instead of just watching games; the effect of millions of examples from a single one (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).
- **KL divergence** = cross-entropy minus entropy of P: the bits per symbol *wasted* by a poorly optimised code; an asymmetric distance-like measure between distributions, zero iff they match (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).
- Part 3 (not yet consolidated) promises the algorithm that turns any predictor into a compressor approaching the information-content limit, completing the claim that cross-entropy training is equivalent to training the best possible text compressor (source: transcript-but-what-is-cross-entropy-compression-is-intelligence-part-2.md).

## Key Takeaways

- Information = −log₂(p); entropy = average information per symbol = the hard floor on compression (noiseless coding theorem).
- Prediction and compression are mathematically equivalent; "cross-entropy loss" is not a naming accident — pre-training literally optimises the model as a compressor of its training distribution.
- Cross-entropy(P, Q) ≥ entropy(P), with equality iff Q = P — the property that makes it a loss function; the log form is mathematically forced, not chosen.
- Shannon estimated English at ~1 bit/character using humans as language models — compressibility estimation has always required intelligence-shaped models.
- Distillation = cross-entropy against a teacher's full distribution; KL divergence = the waste term (cross-entropy − entropy).

## Related

- [[large-database-models]] · [Large Database Models](../model-fundamentals/large-database-models.md) — a different route from representation learning to practical value: embeddings over relational data instead of text
- [[claude-code-effort-and-model-selection]] · [Claude Code Effort Level and Model Selection](../claude-code-practice/claude-code-effort-and-model-selection.md) — the practitioner-facing view of the same machinery: frozen weights, token-by-token generation, steering vs teaching
- [[dianne-penn]] · [Dianne Penn — Anthropic](../product-leader-interviews/dianne-penn.md) — scaling laws lower this same loss smoothly while capabilities jump discontinuously

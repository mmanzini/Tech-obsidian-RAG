---
type: synthesis
title: Constitutional AI — Overview
description: CAI addresses the core tension in RLHF-based alignment — that optimising for harmlessness produces evasive models and requires expensive human labelling — by replacing human harmlessness annotations with AI feedback guided by explicit natural-language principles.
bundle: ai-engineering
topic: constitutional-ai
tags:
- constitutional-ai
- ai-security
- evals
- agent-architecture
resource: https://arxiv.org/abs/2212.08073
sources:
- id: '2212'
  resource: https://arxiv.org/abs/2212.08073
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-09T07:03:05Z'
status: stable
related:
- ai-engineering/constitutional-ai/claude-constitution-2026.md
---

# Constitutional AI — Overview

The Constitutional AI (CAI) method trains harmless-but-helpful AI assistants using AI feedback instead of human harmlessness labels. A small set of natural-language principles (the "constitution") replaces tens of thousands of human preference annotations.

## Summary

CAI addresses the core tension in RLHF-based alignment — that optimising for harmlessness produces evasive models and requires expensive human labelling — by replacing human harmlessness annotations with AI feedback guided by explicit natural-language principles. The two-stage process (Supervised Learning via critique-revision, then RLAIF) produces models that engage with harmful prompts through explanation rather than refusal, and that are both less harmful and more helpful than models trained with human harmlessness labels. The "constitutional" framing emphasises transparency: the governing principles are auditable and modifiable without collecting new labels.

**Source:** Bai et al., [Constitutional AI: Harmlessness from AI Feedback](https://arxiv.org/abs/2212.08073) (Anthropic, Dec 2022)

## The Problem CAI Solves

- RLHF requires tens of thousands of human labels to train for harmlessness
- Human-labelled harmlessness training creates evasive models that refuse to engage with sensitive topics
- Strong tension between helpfulness and harmlessness — optimising for one degrades the other
- Training objectives are opaque: no one can summarise the collective impact of thousands of labels

## Two-Stage Training Process

### Stage 1: Supervised Learning (Critique → Revision)
1. Generate responses to harmful prompts using a helpful-only assistant (responses will be toxic)
2. Ask the model to critique its own response against a randomly drawn constitutional principle
3. Revise the response in light of the critique
4. Repeat critique-revision with different principles
5. Fine-tune a pretrained model on the final revised responses

Purpose: shift the model's response distribution before RL, reducing exploration problems.

### Stage 2: Reinforcement Learning (RLAIF)
1. Use the SL-trained model to generate pairs of responses to harmful prompts
2. Ask the model which response is better according to a constitutional principle (AI preference labels)
3. Mix AI harmlessness preferences with human helpfulness preferences
4. Train a preference model on this hybrid dataset
5. Fine-tune via RL against the preference model

Result: "RL from AI Feedback" (RLAIF) — harmlessness training with zero human harmlessness labels.

## Results

- RL-CAI models preferred by crowdworkers over models trained with human harmlessness labels
- Non-evasive: engages with harmful queries by explaining objections rather than refusing
- Chain-of-thought reasoning improves AI evaluation of harms significantly
- Larger models produce AI evaluations competitive with human-trained preference models
- Reduces helpfulness/harmlessness tension — models are both less harmful and more helpful

## Why "Constitutional"

- Training goals are encoded in a small set (~10) of natural-language principles
- Principles are transparent, auditable, and modifiable without collecting new labels
- The term emphasises that deploying any AI system involves choosing governing principles — better to make them explicit

## Key Takeaways

- CAI demonstrates that AI can supervise AI for harmlessness, using only natural-language principles as human oversight
- Chain-of-thought reasoning is critical — it improves both the quality and transparency of AI evaluations
- The method eliminates evasiveness by training models to engage with harmful requests through explanation rather than refusal
- Iteration time drops dramatically — changing the constitution updates behaviour without new human labels
- This is "scaled supervision": leveraging AI to make human oversight more efficient, not to remove it

## Related

- [[claude-constitution-2026|Claude's Constitution (2026)]] — how the constitutional approach evolved from principles list to holistic document
- [[agent-architecture/index|Agent Architecture]] — patterns for building agents grounded in these values
- [[ai-security/index|AI Security]] — defensive strategies relevant to safely deploying AI systems

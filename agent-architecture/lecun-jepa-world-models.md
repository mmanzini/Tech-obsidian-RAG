---
type: synthesis
title: 'LeCun: JEPA and World Models — The Case Against LLMs for Agentic AI'
description: Yann LeCun has raised approximately $1B to pursue JEPA (Joint Embedding Predictive Architecture) as an alternative to large language models.
bundle: ai-engineering
topic: agent-architecture
tags:
- agent-architecture
- agent-memory
- world-models
- multi-agent
resource: https://www.youtube.com/watch?v=kYkIdXwW2AE
sources:
- id: watch-v-kykidxww2ae
  resource: https://www.youtube.com/watch?v=kYkIdXwW2AE
generated:
  by: claude-code/atlas-consolidate
  at: '2026-06-02T13:33:31Z'
status: stable
related:
- ai-engineering/agent-architecture/pinecone-nexus-knowledge-engine.md
- ai-engineering/agent-architecture/twelve-factor-agents.md
- ai-engineering/agent-architecture/multi-agent-coordination-patterns.md
---

# LeCun: JEPA and World Models — The Case Against LLMs for Agentic AI

**Source:** [Yann LeCun's $1B Bet Against LLMs](https://www.youtube.com/watch?v=kYkIdXwW2AE) (Part 1) · [Part 2](https://www.youtube.com/watch?v=v_jDvpEGTIg)
**Author:** Welch Labs (Sam Baskin, Pranav Gundu, Stephen Welch)

---

## Summary

Yann LeCun has raised approximately $1B to pursue JEPA (Joint Embedding Predictive Architecture) as an alternative to large language models. His core argument: LLMs lack world models and cannot predict the consequences of their actions, making them fundamentally unsuitable for reliable agentic systems. This video traces the intellectual path from Siamese networks (1990s) through representation collapse, Barlow Twins, DINO, and into the full JEPA architecture — explaining why joint embedding rather than generative prediction is the right foundation for AI world models.

## LeCun's Critique of LLMs

LeCun's 2022 position paper, "A Path Towards Autonomous Machine Intelligence," argues that current AI is nowhere near human learning capability — a teenager can learn to drive in 20 hours; Tesla's level-2 system needs millions of hours of training data and still can't reach level 3 (source: transcript-yann-lecuns-1b-bet-against-llms.md).

His direct argument against LLMs for agentic systems (source: transcript-yann-lecuns-1b-bet-against-llms.md):

> "I do not understand how you can even think of building an agentic system without that agentic system having the ability of predicting the consequences of its actions. And an LLM doesn't do that. LLMs do not have world models. They cannot predict the consequences of their actions beforehand — they just take the action and then, après moi le déluge. So if you really want to build reliable agentic systems, they absolutely have to be able to predict the consequences of their actions so that they can plan a sequence of actions... And the inference process now becomes a search as opposed to just an auto-regressive prediction."

LeCun's prediction on LLMs vs. JEPA (source: transcript-yann-lecuns-1b-bet-against-llms.md): "Initially they'll solve different problems. Eventually they'll replace LLMs — because LLMs are really good at manipulating language but basically nothing else."

## The Problem with Generative Prediction

The GPT-1 breakthrough (Radford at OpenAI, 2018) used self-supervised next-token prediction, unlocking unprecedented scale by eliminating dependence on human labels. This approach works in language because language has a discrete vocabulary (~50,000 tokens) (source: transcript-yann-lecuns-1b-bet-against-llms.md).

For video, the same generative approach fails: in full HD video, each frame has ~10^15,000,000 possible pixel configurations — making discrete output enumeration impossible (source: transcript-yann-lecuns-1b-bet-against-llms.md). When forced to predict a single output frame from ambiguous input (a ball that could bounce left or right), the model predicts the average of all possibilities — producing a blurry, washed-out frame. This blurriness compounds catastrophically in auto-regressive generation (source: transcript-yann-lecuns-1b-bet-against-llms.md).

## Joint Embedding Architectures

The alternative: don't ask the model to reconstruct the input. Instead, pass two related inputs (original + corrupted/transformed view) through encoder networks that output embedding vectors, and train a predictor to map one embedding to the other (source: transcript-yann-lecuns-1b-bet-against-llms.md).

This approach — Siamese networks — originated in LeCun's Bell Labs work in the early 1990s for signature fraud detection (source: transcript-yann-lecuns-1b-bet-against-llms.md). The core insight: useful internal representations can be learned without generating any data, by forcing the network to produce similar embeddings for semantically equivalent inputs.

**Representation collapse problem**: A trivial solution exists — output the same embedding vector for every input. This collapses the representation without learning anything useful (source: transcript-yann-lecuns-1b-bet-against-llms.md). Contrastive learning (using negative examples) partially addresses it but requires exponentially many samples at scale.

## Barlow Twins — The Epiphany

In 2020, LeCun's postdoc Stéphane Deny proposed applying Horace Barlow's 1961 neuroscience hypothesis (neurons reduce redundancy between their outputs) to avoid representation collapse (source: transcript-yann-lecuns-1b-bet-against-llms.md).

**Barlow Twins loss function**: Compare the cross-correlation matrix of the two encoders' outputs to the identity matrix. Diagonal entries (corresponding neurons) should be 1 (maximum similarity); off-diagonal entries (different neurons) should be 0 (minimum redundancy) (source: transcript-yann-lecuns-1b-bet-against-llms.md).

Result: a frozen Barlow Twins encoder with a linear probe achieved 73.2% ImageNet accuracy — outperforming the original fully-supervised AlexNet by 10+ percentage points, entirely through self-supervised pre-training (source: transcript-yann-lecuns-1b-bet-against-llms.md).

## DINO and the Self-Supervised Milestone

DINOv3 (August 2025) marked the first time a self-supervised joint embedding model achieved near-state-of-the-art ImageNet accuracy (88.4%) comparable to weakly-supervised models (source: transcript-yann-lecuns-1b-bet-against-llms.md). DINO's emergent capability: segmentation by embedding similarity — comparing the embedding of one patch to all other patches reveals semantically coherent regions without any segmentation training.

## JEPA Architecture

JEPA (Joint Embedding Predictive Architecture) extends joint embedding to temporal prediction (source: transcript-yann-lecuns-1b-bet-against-llms.md):

1. Take observation at time t and next observation at time t+1
2. Pass both through encoders → embeddings
3. Train a predictor: given embedding(t) + action, predict embedding(t+1)
4. The predictor learns only the salient features the encoder passes through — not every pixel of "random motion of leaves on trees"

This is classical optimal control (goes back to 1950s/1960s Soviet control theory) applied with learned representations (source: transcript-yann-lecuns-1b-bet-against-llms.md). What is new: (1) the model is learned via machine learning, and (2) the prediction happens in abstract embedding space rather than pixel space.

**V-JEPA 2** (robot arm application): conditions a JEPA model on robot arm control signals + video frames → predictor learns how actions change the robot's position in embedded image space → enables robot planning by searching for actions that bring predicted future state to match goal state (source: transcript-yann-lecuns-1b-bet-against-llms.md).

## Intelligence as a Cake

LeCun's 2015 framing (now a meme in ML): "If intelligence is a cake, the bulk of the cake is self-supervised learning, the icing on the cake is supervised learning, and the cherry on the cake is reinforcement learning" (source: transcript-yann-lecuns-1b-bet-against-llms.md). This prediction proved correct for language (GPT series matched this exactly: self-supervised pre-training → supervised fine-tuning → RLHF). JEPA extends it to vision and world modelling.

## Part 2 — From V-JEPA to robot planning

Part 2 of the series works up the VLA (vision-language-action) stack and shows where JEPA-based alternatives slot in at each layer (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md).

**V-JEPA 2 inside a VLM.** Meta's V-JEPA 2 (2025) was trained on one million hours of video with up to one billion parameters, self-supervised by removing patches and predicting the embeddings of the missing patches. Swapped in for the CLIP vision encoder of a vision-language model, it reached state-of-the-art on a set of video-understanding benchmarks — "a video encoder pre-trained without language supervision can be aligned with a language model... contrary to conventional wisdom." The encoder is language-blind yet still interfaces with the LLM (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md). LeCun's new venture Omni Labs puts the thesis on its landing page: "Real intelligence does not start in language. It starts in the world." (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md)

**VL-JEPA — JEPA for the whole VLM.** Instead of generating output text, VL-JEPA predicts the *embedding* of the target text, abstracting away irrelevant phrasing differences (e.g. "do not eat this mushroom" vs "this mushroom is not safe to eat" map to similar embeddings, so the model is not penalised for a correct paraphrase). In a controlled comparison it learned far more efficiently — 35% video-classification accuracy after 5M examples vs 20% for a standard VLM — and a 1.6B-parameter VL-JEPA outperformed 7B models on the GQA compositional-reasoning benchmark. Because it is non-generative, answers come either by embedding-matching against candidate answers or via a trained text decoder (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md).

**The VLA critique — "VLAs are doomed."** LeCun's two objections to vision-language-action models: (1) behavioural cloning does not scale — collecting human demonstrations for every variation is impractical, and the models are brittle outside their demonstration distribution; (2) no explicit planning — VLAs are trained end-to-end and act as a black box mapping images+instructions to joint positions, with no way to predict the consequences of actions before taking them (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md). He grants VLAs generalise somewhat (Google's RT2 moving a can to a picture of Taylor Swift in 2023; Physical Intelligence's PI07), but argues an agentic system must be able to predict consequences and plan — "the inference process now becomes a search as opposed to just an auto-regressive prediction" (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md).

**LeWorldModel + Push-T.** LeCun's alternative learns an action-conditioned world model with JEPA, then plans explicitly. On the Push-T task (push a T-shape to a target), a predictor is trained to map (current-image embedding + action) → next-image embedding; a decoder can render predicted embeddings back to images, revealing that the model learns the environment's physics from pixels and actions alone. Planning uses the Cross Entropy Method: sample random action trajectories, score each by the embedding-space distance between its predicted final state and the goal, keep the elite set, resample, repeat. Planning happens entirely in embedding space. The limitation: LeWorldModel reliably plans only ~5 steps ahead and trails VLA on Push-T performance (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md).

**Hierarchical JEPA.** Longer horizons need hierarchy: low levels make short-term, detailed predictions; higher levels make longer-term, lower-detail predictions so they don't diverge from reality. Two layers extended the Push-T horizon from 5 to 15 steps, with high-level predictions serving as sub-goals for the low level. The interface between layers is an embedding space, not language ("your cat can do hierarchical planning, and they don't have language"); the hope is the hierarchy emerges from training, as feature hierarchies emerge in CNNs (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md).

**Where this is headed.** Omni Labs' near-term aim (1–2 years) is industrial applications: controlling complex systems whose behaviour cannot be reduced to a few equations — a jet engine, a chemical or power plant, blood-sugar control in a diabetic patient, materials and catalyst design — by learning a phenomenological model from data and planning against it. The 3–5 year hope is to become a main supplier of intelligent systems (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md). Welch Labs' own take: the vision is compelling but JEPA is early — V-JEPA 2 and VL-JEPA show it is compatible with the mainstream language-driven stack, but on agentic and robotics problems JEPA world-model approaches remain limited, with many open research questions (source: transcript-yann-lecuns-1b-bet-against-llms-part-2.md).

## Key Takeaways

- LLMs are generative predictors in token space; they lack the ability to predict consequences of actions in the world — a fundamental gap for reliable agentic systems
- Joint embedding architectures sidestep the blurry-prediction problem by operating in compressed embedding space rather than pixel/token space
- Representation collapse (the trivial-solution problem of joint embedding) is solvable via Barlow Twins / VICReg / DINO without contrastive negative examples
- JEPA = joint embedding + temporal prediction conditioned on actions = a trainable world model
- As of 2025/2026, DINOv3 matches supervised performance on image classification; V-JEPA 2 enables zero-shot robot arm planning — the architecture is mature but not yet mainstream
- JEPA slots into the existing stack at every VLA layer: V-JEPA 2 as a language-blind vision encoder that still beats CLIP inside a VLM; VL-JEPA predicting target-text *embeddings* for big efficiency gains (1.6B beating 7B on GQA); LeWorldModel doing explicit embedding-space planning via the Cross Entropy Method
- LeCun calls VLAs "doomed" — behavioural cloning is brittle and unscalable, and end-to-end VLAs cannot plan; but JEPA world-model robotics still trails VLA today (~5-step horizon, extended to 15 with hierarchical JEPA), so the bet is on trajectory, not current performance

## Related

- [[pinecone-nexus-knowledge-engine|Pinecone Nexus — Knowledge Engine for Agents]] — parallel argument from infrastructure side: the knowledge/context layer is the gap, not model reasoning
- [[twelve-factor-agents|Twelve-Factor Agents]] — engineering principles for production agents that assume LLMs as the reasoning layer; LeCun's critique is the architectural counterargument
- [[multi-agent-coordination-patterns|Multi-Agent Coordination Patterns]] — coordination patterns that could apply regardless of whether the underlying model is LLM or JEPA-based

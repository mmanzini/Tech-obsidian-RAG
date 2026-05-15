# LeCun: JEPA and World Models — The Case Against LLMs for Agentic AI

**Source:** [Yann LeCun's $1B Bet Against LLMs](https://www.youtube.com/watch?v=kYkIdXwW2AE)
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

## Key Takeaways

- LLMs are generative predictors in token space; they lack the ability to predict consequences of actions in the world — a fundamental gap for reliable agentic systems
- Joint embedding architectures sidestep the blurry-prediction problem by operating in compressed embedding space rather than pixel/token space
- Representation collapse (the trivial-solution problem of joint embedding) is solvable via Barlow Twins / VICReg / DINO without contrastive negative examples
- JEPA = joint embedding + temporal prediction conditioned on actions = a trainable world model
- As of 2025/2026, DINOv3 matches supervised performance on image classification; V-JEPA 2 enables zero-shot robot arm planning — the architecture is mature but not yet mainstream

## Related

- [[pinecone-nexus-knowledge-engine|Pinecone Nexus — Knowledge Engine for Agents]] — parallel argument from infrastructure side: the knowledge/context layer is the gap, not model reasoning
- [[twelve-factor-agents|Twelve-Factor Agents]] — engineering principles for production agents that assume LLMs as the reasoning layer; LeCun's critique is the architectural counterargument
- [[multi-agent-coordination-patterns|Multi-Agent Coordination Patterns]] — coordination patterns that could apply regardless of whether the underlying model is LLM or JEPA-based

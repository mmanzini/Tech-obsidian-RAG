# An AI Glossary

**Source:** Lenny Rachitsky, [Lenny's Newsletter](https://www.lennysnewsletter.com/p/an-ai-glossary) (2025-06-24) — "explain-it-like-I'm-5" definitions of the ~20 most common AI terms.

Quick reference for meetings and reading. Definitions are paraphrased; link back to the newsletter for worked examples.

## Foundations

- **Model** — a computer program built to work like a brain. Input → processing → output. "Learns" by being exposed to examples until it recognises patterns.
- **LLM (large language model)** — text-based model for understanding and generating human-readable text. Most are now multi-modal (text + images + audio in one interface).
- **Transformer** — the 2017 Google architecture that made modern AI possible. Key innovation: **attention** (reads all words at once, not word-by-word). Highly parallelizable → scales with compute.
- **Token** — the basic unit of text for an LLM (often a sub-word piece). "ChatGPT" splits into "Chat", "GPT". Patches for images, frames for voice.
- **Inference** — when the model "runs". Every ChatGPT response is an inference call.
- **GPT** — Generative + Pre-trained + Transformer. The three ideas behind ChatGPT-class LLMs.

## Training & post-training

- **Training / pre-training** — analyzing massive data (internet, books, etc.). Weeks-to-months, hundreds of millions of dollars. Core method for LLMs: **next-token prediction**. Weights get reinforced/adjusted like neuron connections.
- **Supervised learning** — labeled data. Spam vs. not-spam examples → classifier. Most modern LLMs use **self-supervised** learning: hide the last token, learn to predict it.
- **Unsupervised learning** — no labels. Find structure (clustering, anomaly detection, topic modelling).
- **Post-training** — everything after base training to make the model useful: fine-tuning, RLHF, etc.
- **Fine-tuning** — additional training on task-specific data to specialise a model (company style, medical literature, grade-level tutoring). Tweaks weights while preserving general knowledge.
- **RLHF (reinforcement learning from human feedback)** — two stages: (1) humans pick preferred output → trains a reward model; (2) LLM learns via reinforcement against the reward model. Key alignment method.

## Working with models

- **Prompt engineering** — crafting inputs that produce better outputs. Two categories: conversational prompts (what you type in chat) and system/product prompts (baked into products).
- **RAG (retrieval-augmented generation)** — open-book test instead of closed-book. Search docs/DBs at run-time and stuff results into the prompt. Mitigates hallucinations from missing context.
- **Evals** — structured measurement of AI performance (correctness, safety, tone). Unit tests / benchmarks for AI products. Most critical yet most overlooked PM skill per top builders.
- **MCP (model context protocol)** — open standard for models to interact with external tools (calendar, CRM, Slack, codebase). Previously needed custom integrations. Competitors: A2A (Google), ACP (BeeAI/IBM).
- **Hallucination** — confident but wrong output. Model doesn't "know" facts — it predicts likely next tokens. Mitigate with RAG + prompt engineering.

## Agents & frontier

- **Gen AI** — systems that generate new content (text, image, code, audio, video). Opposed to classifiers/analyzers.
- **Agent** — AI system that takes actions to accomplish a goal. A spectrum: proactive, plans, takes real-world action, uses live data, creates its own feedback loop. (Link: [Anthropic's building effective agents guide](https://www.anthropic.com/engineering/building-effective-agents))
- **Vibe coding** — Karpathy's term. Building with AI tools (Cursor, Windsurf, Bolt, Lovable, v0, Replit) by describing what you want; often never looking at the code.
- **AGI (artificial general intelligence)** — AI capable across a wide range of tasks, including learning new ones. "More intelligent than the average human across most subjects." Some argue we're already there.
- **ASI (artificial superintelligence)** — next step beyond AGI; better than the best humans in virtually every domain. Debate: fast vs. slow takeoff.
- **Synthetic data** — artificially generated training data. Used when real data is limited, sensitive, or exhausted. Often indistinguishable from human-generated data.

## How the levers relate

- **Pre-training** → general knowledge and language
- **Fine-tuning** → task specialisation
- **RLHF** → alignment to human preferences
- **Prompt engineering** → shaping inputs in-context
- **RAG** → retrieving run-time context the model wasn't trained on

## Key Takeaways

- Modern LLMs = GPT = Generative + Pre-trained + Transformer. Attention is the unlock.
- Adjusting model behaviour moves down a stack: pre-training → fine-tuning → RLHF → prompt engineering → RAG. Reach for the cheapest lever first.
- Agents sit on a **spectrum** of agency (5 dimensions), not a binary.
- Evals are the most-overlooked critical PM skill; hallucinations are mitigated by RAG + better prompts.
- MCP is the emerging de-facto protocol for model-tool integration.

## Related

- [[product-management/pm-guide-to-evals|PM guide to evals]]
- [[product-management/ai-prototyping-for-pms|AI prototyping for PMs]]
- [[agent-architecture/_index|Agent Architecture]]
- [[harness-engineering/_index|Harness Engineering]]

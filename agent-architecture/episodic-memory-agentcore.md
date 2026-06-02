# Episodic Memory for Agents — Amazon Bedrock AgentCore

**Source:** [Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory](https://aws.amazon.com/blogs/machine-learning/build-agents-to-learn-from-experiences-using-amazon-bedrock-agentcore-episodic-memory/)
**Author:** Jiarong Jiang, Akarsha Sehwag, Mani Khanuja, Peng Shi, Ruo Cheng, Anil Gurrala (AWS)
**Published:** 2026-01-21

---

## Summary

Episodic memory lets an agent retain and reason over its own past *experiences* — the goal, reasoning steps, actions, outcomes, and reflections of a complete interaction — rather than just facts. AWS Bedrock AgentCore turns raw conversations into structured episodes through a two-stage extraction, stores them in a vector store indexed on user intent, and exposes them at inference through two tools: one that returns step-by-step exemplars from similar past interactions, and one that returns generalised reflections. This is the distinction from semantic memory (which stores facts) and from procedural memory (implicit skills): episodic memory explicitly documents what happened and what worked (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md).

![[agentcore-episodic-memory-overview.png]]

## Memory types

AgentCore frames four complementary memory types (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md):

- **Episodic** — captures experience and complete reasoning paths.
- **Semantic** — stores facts.
- **Summarization** — manages context length.
- **Preference** — handles personalisation.

Unlike semantic memory (factual knowledge) or procedural memory (implicit skills), episodic memory explicitly documents the goal, reasoning steps, actions, outcomes, and reflections from actual interactions (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md).

## The episode data model — two-stage extraction

Episodes are extracted at two granularities (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md):

![[agentcore-episodic-memory-extraction.png]]

**Turn-level** (individual interaction units):
- Turn situation — context and objectives
- Turn intent — the assistant's specific goal
- Turn action — tools used and parameters
- Turn thought — reasoning behind decisions
- Turn assessment — success evaluation for that turn
- Goal assessment — progress toward the overall user objective

**Episode-level** (the complete user journey):
- Episode situation — broader circumstances
- Episode intent — the ultimate user goal
- Success evaluation — whether the objective was achieved
- Evaluation justification — concrete evidence from the conversation
- Episode insights — effective approaches and pitfalls

## Capture, storage, retrieval

- **Trigger.** An episode is written when a user completes their goal (detected by the LLM) or an interaction ends (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md).
- **Storage.** Episodes are stored in a vector store with semantic indexing on user intent, enabling similarity matching (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md).
- **Retrieval.** Two inference-time mechanisms: `retrieve_exemplars` fetches step-by-step solutions from past interactions, and `retrieve_reflections` accesses generalised patterns and strategic insights (source: 2026-06-02-Build agents to learn from experiences using Amazon Bedrock AgentCore episodic memory.md).

## Key Takeaways

- Episodic memory = the agent learning from its own experience: goal → reasoning → actions → outcome → reflection, captured as a structured record and recalled later
- Two extraction granularities — turn-level (per interaction) and episode-level (per user journey) — separate fine-grained reasoning traces from journey-level success/insight
- Two retrieval modes mirror two needs: exemplars (concrete step-by-step from a similar past case) and reflections (generalised heuristics) — the difference between "copy what worked" and "apply the lesson"
- Episodes are written at goal-completion or interaction-end and indexed by intent for similarity recall — recall is intent-matched, not chronological

## Related

- [[twelve-factor-agents|Twelve-Factor Agents]] — production-agent principles; episodic memory addresses the cross-session learning gap they assume away
- [[pinecone-nexus-knowledge-engine|Pinecone Nexus — Knowledge Engine for Agents]] — the semantic/context layer for agents; episodic memory is the complementary experience layer
- [[../knowledge-engineering/_index|Knowledge Engineering]] — the Atlas vault adopts this episodic/semantic split: buckets as semantic memory, an `_episodes/` zone for experiences recalled at run-start

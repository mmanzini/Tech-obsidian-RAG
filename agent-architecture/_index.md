# Agent Architecture

Foundational principles and design patterns for building reliable, production-grade AI agents.

## Articles

- [[twelve-factor-agents|Twelve-Factor Agents]] — 12 principles for LLM-powered software reliable enough for production customers (HumanLayer)
- [[lethal-trifecta-and-agentic-patterns|Lethal Trifecta and Agentic Engineering Patterns]] — Simon Willison on the unsolved trifecta (private data + untrusted input + external send), red/green TDD, dark factories, and AI exhaustion
- [[seeing-like-an-agent|Seeing Like an Agent — Designing Tools in Claude Code]] — AskUserQuestion origin story, todo→task transition, progressive disclosure over new tools (Anthropic)
- [[multi-agent-coordination-patterns|Multi-Agent Coordination Patterns]] — Five canonical patterns (generator-verifier, orchestrator-subagent, agent teams, message bus, shared state) and decision criteria for choosing between them (Anthropic)
- [[simon-willison|Simon Willison on Lenny's Podcast]] — Nov 2025 crossed "mostly works → almost always works"; AI trades labour for cognitive load; the lethal trifecta
- [[chip-huyen|Chip Huyen on Lenny's Podcast]] — what people think improves AI apps isn't what actually does; users, data prep, and prompts are the work
- [[alexander-embiricos|Alexander Embiricos on Lenny's Podcast]] — Codex as teammate; 3-layer stack to locate the wedge in the harness
- [[aishwarya-reganti-kiriti-badam|Aishwarya Reganti & Kiriti Badam on Lenny's Podcast]] — AI products are non-deterministic in input, output AND process; the CCCD loop
- [[pinecone-nexus-knowledge-engine|Pinecone Nexus — Knowledge Engine for Agents]] — most agent failures are data failures; Knowledge Engine pre-compiles typed artifacts so agents query in one step; KRAFTBench: Nexus 68% accuracy at 22s vs RAG 41% at 38s (Pinecone, 2026)
- [[lecun-jepa-world-models|LeCun: JEPA and World Models]] — LLMs lack world models and can't predict action consequences; JEPA (joint embedding + temporal prediction) is the right agentic AI foundation; Barlow Twins → DINO → V-JEPA 2 arc (Welch Labs, 2026)

## Related Topics

- [[harness-engineering/_index|Harness Engineering]] — applying these principles to coding agent configuration
- [[agent-workflows/_index|Agent Workflows]] — structured task patterns (RPI, Quick Dev) that embody these principles
- [[ai-organization/_index|AI & Organisation Design]] — how agent architecture enables org-level AI coordination
- [[rpi-methodology/_index|RPI Methodology]] — applied workflow that embodies twelve-factor principles

# AI Engineering

**Scope**: How to build with AI: agent architecture and workflows, the harness layer around coding agents (CLAUDE.md, MCP, skills, hooks, sub-agents, long-running patterns), structured methodologies (RPI, spec-driven development, adversarial review), AI-native verticals (banking, open-data apps), AI security, design tooling for agents, and the AI-bound PM craft (eval design for AI products, AI-prototyping, the AI-accelerated PM role). Belongs here: anything about *how to build well with AI* — tools, patterns, methodologies. Does **not** belong here: evergreen PM craft (that goes to top-level `product-management`); team/operating models that aren't AI-specific (that goes to top-level `way-of-working`); macro/geopolitical commentary on AI as market/policy (that goes to `political-economy`); raw GitHub trending repo writeups (`github-trends`). Boundary test for PM/WoW topics: *if I removed AI from the picture, would this article still make sense?* If yes, it belongs in the top-level bundle; if it is fundamentally about AI's effect on PM/team practice, it belongs here. Articles that fit both Scope paragraphs are duplicated per hard-rule 2.

## Topics

- [[agent-architecture/index|Agent Architecture]] — Foundational principles and coordination patterns for building reliable production AI agents (12-factor agents, multi-agent patterns, lethal trifecta security patterns, agentic tool design)
- [[harness-engineering/index|Harness Engineering]] — Designing and optimising the runtime around coding agents: long-running patterns, CLAUDE.md, MCP, skills, hooks, sub-agents, advisor strategy, codebase design for AI, browser MCP, NLH/Meta Harness science, scaling managed agents, MCP Apps interactive UI, model comparison
- [[claude-code-practice/index|Claude Code Practice]] — Claude Code features, workflows, and practitioner interviews: routines, desktop parallel sessions, session management, Opus 4.7 tuning, structured memory, Cowork guides, Managed Agents memory, Agentic OS three-gap framework
- [[knowledge-engineering/index|Knowledge Engineering]] — PKM methodology and vault architecture: Zettelkasten lineage, Karpathy's LLM Knowledge Bases, autoresearch methodology, Atlas sync architecture and schema design
- [[agent-workflows/index|Agent Workflows]] — Structured patterns for directing agents through complex tasks (RPI, Quick Dev, Adversarial Review, Approaches Compared)
- [[rpi-methodology/index|RPI Methodology]] — Deep reference for Research-Plan-Implement: principles, FAR/FACTS gates, context engineering, tool workflows, team adoption, CRISPY/QRSPI evolution, industry positioning
- [[ai-dev-tools/index|AI Dev Tools]] — IDE-level features for AI-assisted development (Kiro hooks, specs, steering)
- [[ai-organization/index|AI & Organisation Design]] — How AI changes company structure (Block/Sequoia, Dorsey mini-AGI), AI fluency curriculum, product management on the AI exponential, Anthropic Economic Index, product job market 2025/2026, AI glossary, and Anthropic-adjacent takeaways (Avasare, Cherny, Vo)
- [[product-leader-interviews/index|Product Leader Interviews]] — Per-guest Lenny's Podcast takeaway digests from product/AI leaders (Cat Wu, Bret Taylor, Nick Turley, Fei-Fei Li, Dan Shipper, et al.) on building and leading in the AI era
- [[spec-driven-development/index|Spec-Driven Development]] — Tool-agnostic standard treating the spec as the durable artifact (phases, roles, boundaries)
- [[ci-integrations/index|CI Integrations]] — Running Claude in CI/CD: GitHub Actions, managed Code Review with REVIEW.md customization
- [[ai-native-banking/index|AI-Native Banking]] — AI-native banking OS architecture and Backbase 2026 segment-level predictions
- [[open-data-apps/index|Open Data Apps]] — Portfolio business model: small EUR 0.99 apps wrapping free public APIs
- [[design-in-ai/index|Design in AI]] — Encoding design intent for AI agents: Design.md convention, Google Stitch, design tokens history, accessibility gaps, tool landscape
- [[constitutional-ai/index|Constitutional AI]] — Anthropic's approach to AI values: the CAI training method, Claude's 2026 constitution, and the shift from rules-list to explanatory document
- [[ai-security/index|AI Security]] — Defensive security strategies for AI-accelerated offense: patching, vulnerability management, zero trust, incident response
- [[ai-product-development/index|AI Product Development]] — Frameworks for building better products with AI: Sandbox Discovery, Adversarial Iteration, Modular Context Navigation; plus field survey of what people are vibe-coding
- [[product-management/index|Product Management (AI-bound)]] — AI-accelerated PM craft: AI-augmented strategy, AI-prototyping workflows, evals, second-brain workflows, the AI-enabled builder role, the unfair AI-era PM role. Evergreen PM craft lives in the top-level `product-management` bundle
- [[model-driven-development/index|Model-Driven Development]] — formal models as the primary artefact; CIM/PIM/PSM abstraction stack; transformations; classic OMG MDA vs modern DSLs/low-code/executable models/AI assistance; companion to Spec-Driven Development

## Tag vocabulary

- `agent-architecture` — design principles for reliable agents
- `harness-engineering` — runtime config around coding agents
- `claude-code` — Claude Code features and workflows
- `agent-workflows` — structured task patterns (RPI, SDD)
- `spec-driven-development` — spec/model as durable artefact
- `context-engineering` — instruction budget and context management
- `agent-memory` — episodic memory, session recall, persistence
- `multi-agent` — orchestration and coordination of agents
- `evals` — eval design, benchmarks, quality measurement
- `knowledge-management` — PKM, vaults, knowledge bases, RAG
- `mcp` — Model Context Protocol and connectors
- `skills-and-hooks` — skills, hooks, deterministic enforcement
- `design-in-ai` — encoding design intent for AI agents
- `ai-security` — defensive security and prompt-injection
- `constitutional-ai` — AI values, alignment, training
- `ai-org-design` — org structure and team shape with AI
- `product-management` — AI-bound PM craft and role
- `ai-native-business` — AI-native verticals and business models
- `practitioner-interview` — leader/builder operating principles
- `long-running-agents` — autonomous multi-session agent patterns

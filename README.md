# AI Engineering

How to build with AI: agent architecture and workflows, the harness layer around coding agents (CLAUDE.md, MCP, skills, hooks, sub-agents, long-running patterns), structured methodologies (RPI, spec-driven development, adversarial review), AI-native verticals (banking, open-data apps), AI security, design tooling for agents, and the AI-bound PM craft (eval design for AI products, AI-prototyping, the AI-accelerated PM role). Belongs here: anything about *how to build well with AI* — tools, patterns, methodologies. Does **not** belong here: evergreen PM craft (that goes to top-level `product-management`); team/operating models that aren't AI-specific (that goes to top-level `way-of-working`); macro/geopolitical commentary on AI as market/policy (that goes to `political-economy`); raw GitHub trending repo writeups (`github-trends`). Boundary test for PM/WoW topics: *if I removed AI from the picture, would this article still make sense?* If yes, it belongs in the top-level bundle; if it is fundamentally about AI's effect on PM/team practice, it belongs here. Articles that fit both Scope paragraphs are duplicated per hard-rule 2.

This repository is a public mirror of one bundle from a personal Obsidian RAG vault (Atlas). Articles are distilled from primary sources — talks, papers, reporting, essays, podcasts, original analysis — into concise, cross-linked notes with inline source citations. It currently holds **198 articles across 22 topics**.

## Topics

- [Agent Architecture](agent-architecture/) — Foundational principles and coordination patterns for building reliable production AI agents (12-factor agents, multi-agent patterns, lethal trifecta security patterns, agentic tool design)
- [Harness Engineering](harness-engineering/) — Designing and optimising the runtime around coding agents: long-running patterns, NLH/Meta Harness science, advisor strategy, codebase design for AI, and the instruction mechanics that steer an agent (CLAUDE.md, skills, hooks, subagents, verification loops)
- [Agent Infrastructure](agent-infrastructure/) — The execution substrate beneath the harness: Managed Agents (scaling, self-hosted sandboxes, Dreams), multi-agent orchestration runtimes (Agent View, Sandcastle, graph engineering), and the tool surfaces agents reach through (computer/browser use, browser MCP, MCP Apps, cache diagnostics)
- [Claude Code Practice](claude-code-practice/) — Claude Code features, workflows, and practitioner interviews: routines, desktop parallel sessions, session management, Opus 4.7 tuning, structured memory, Cowork guides, Managed Agents memory, Agentic OS three-gap framework
- [Knowledge Engineering](knowledge-engineering/) — PKM methodology and vault architecture: Zettelkasten lineage, Karpathy's LLM Knowledge Bases, autoresearch methodology, Atlas sync architecture and schema design
- [Agent Workflows](agent-workflows/) — Structured patterns for directing agents through complex tasks (RPI, Quick Dev, Adversarial Review, Approaches Compared)
- [RPI Methodology](rpi-methodology/) — Deep reference for Research-Plan-Implement: principles, FAR/FACTS gates, context engineering, tool workflows, team adoption, CRISPY/QRSPI evolution, industry positioning
- [AI Dev Tools](ai-dev-tools/) — IDE-level features for AI-assisted development (Kiro hooks, specs, steering)
- [AI & Organisation Design](ai-organization/) — How AI changes company structure (Block/Sequoia, Dorsey mini-AGI), AI fluency curriculum, product management on the AI exponential, Anthropic Economic Index, product job market 2025/2026, AI glossary, and Anthropic-adjacent takeaways (Avasare, Cherny, Vo)
- [Product Leader Interviews](product-leader-interviews/) — Per-guest Lenny's Podcast takeaway digests from product/AI leaders (Cat Wu, Bret Taylor, Nick Turley, Fei-Fei Li, Dan Shipper, et al.) on building and leading in the AI era
- [Spec-Driven Development](spec-driven-development/) — Tool-agnostic standard treating the spec as the durable artifact (phases, roles, boundaries)
- [CI Integrations](ci-integrations/) — Running Claude in CI/CD: GitHub Actions, managed Code Review with REVIEW.md customization
- [AI-Native Banking](ai-native-banking/) — AI-native banking OS architecture and Backbase 2026 segment-level predictions
- [Open Data Apps](open-data-apps/) — Portfolio business model: small EUR 0.99 apps wrapping free public APIs
- [Design in AI](design-in-ai/) — Encoding design intent for AI agents: Design.md convention, Google Stitch, design tokens history, accessibility gaps, tool landscape
- [Constitutional AI](constitutional-ai/) — Anthropic's approach to AI values: the CAI training method, Claude's 2026 constitution, and the shift from rules-list to explanatory document
- [AI Security](ai-security/) — Defensive security strategies for AI-accelerated offense: patching, vulnerability management, zero trust, incident response
- [AI Product Development](ai-product-development/) — Frameworks for building better products with AI: Sandbox Discovery, Adversarial Iteration, Modular Context Navigation; plus field survey of what people are vibe-coding
- [Product Management (AI-bound)](product-management/) — AI-accelerated PM craft: AI-augmented strategy, AI-prototyping workflows, evals, second-brain workflows, the AI-enabled builder role, the unfair AI-era PM role. Evergreen PM craft lives in the top-level `product-management` bundle
- [Model-Driven Development](model-driven-development/) — formal models as the primary artefact; CIM/PIM/PSM abstraction stack; transformations; classic OMG MDA vs modern DSLs/low-code/executable models/AI assistance; companion to Spec-Driven Development
- [Model Evaluation](model-evaluation/) — Methods for evaluating and predicting model behaviour: pre-release deployment simulation, eval design, behavioural testing
- [Model Fundamentals](model-fundamentals/) — How models learn and represent data from first principles: information theory and the compression⇄prediction equivalence behind cross-entropy pre-training; model families beyond LLMs (Large Database Models over relational data)

## How this repository is structured (Open Knowledge Format)

This repository is a conformant [**Open Knowledge Format (OKF) v0.2**](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) bundle. OKF is Google Cloud's vendor-neutral standard for agent-readable knowledge: a directory of markdown files, one concept per file, with YAML frontmatter and cross-links. No SDK, no database, no proprietary account — if you can `cat` a file you can read it; if you can `git clone` the repo you can ship it.

Each bundle of the parent vault is OKF-conformant in place — there is no separate export step. What that means here:

- **`index.md` routers** — `index.md` at the bundle root and in every topic folder is a directory listing for progressive disclosure: scan it to see what exists before opening articles. The bundle-root `index.md` declares `okf_version: "0.2"`.
- **`<concept>.md` articles** — every other `.md` file is a concept document. Each opens with YAML frontmatter whose only required field is `type` (Atlas uses a small vocabulary: `synthesis`, `reference`, `analysis`, `digest`, …), followed by `title`, `description`, `tags`, a `sources:` provenance list, `generated:`/`verified:` trust metadata (OKF v0.2 actor convention), `status`, and a `related:` cross-link graph.
- **`log.md`** — a derived, date-grouped changelog of what changed at this level.
- **Conformance** — every non-reserved `.md` file carries strictly parseable frontmatter with a non-empty `type`; `index.md` / `log.md` are the only reserved filenames. Consumers tolerate unknown types and broken links by design, so the bundle stays useful as it grows.

Because the format is just files, this repo is readable in any editor, renderable on GitHub, parseable by any agent, and portable across tools with no translation layer.

## Method

Sources pass through a structured `consolidate` pass — each article cites its source files inline, and contradictions between sources are surfaced rather than smoothed over. A separate `refine` pass audits the bundle for orphans, broken links, and OKF/schema violations. The full method lives in the parent vault and is not published here.

## Updates

This repository syncs from the parent vault on a schedule; content is added or revised whenever the upstream notes change. The change history is visible in the commit log and in any `log.md` files.

---

<sub>README generated from Atlas (`okf_tools.py --readme ai-engineering`) — do not edit by hand; edit the bundle's `index.md` or the shared OKF section in the generator.</sub>

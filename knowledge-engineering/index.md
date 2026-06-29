# Knowledge Engineering

Personal knowledge management methodology, vault architecture, and the design lineage from Zettelkasten through Karpathy's LLM Knowledge Bases to Atlas's bucket/topic/article structure.

## Articles

- [[atlas-sync-architecture|Atlas Sync Architecture — Vault ↔ Public Repos via Unison]] — two-location pattern bridging Atlas and 5 public GitHub repos via Unison; per-repo data flow direction, schedule, conflict handling
- [[atlas-sync-operations|Atlas Sync Operations — Practical Command Reference]] — ops runbook for running, debugging, and extending the Atlas ↔ public-repos Unison sync
- [[autoresearch-methodology|Autoresearch — Autonomous AI Research Methodology]] — Karpathy's overnight GPU experiment loop; program.md as steering harness; the simplicity criterion; direct precursor to Atlas
- [[karpathy-llm-knowledge-bases|Karpathy's LLM Knowledge Bases — Personal Workflow]] — upstream inspiration for Atlas: raw/ → LLM-compiled wiki → Obsidian frontend; index-based navigation without vector RAG
- [[open-knowledge-format|Open Knowledge Format (OKF)]] — Google Cloud's vendor-neutral spec standardising the LLM-wiki (markdown + YAML frontmatter + cross-links) into a portable bundle; the lineage Atlas sits in
- [[okf-spec-v0-1|OKF v0.1 — normative specification]] — the structural rules: bundle layout, reserved filenames, `<concept>.md` documents, cross-linking, and the three conformance tests; how Atlas maps onto them
- [[okf-type-tags-and-obsidian-bases|Type vs tags, content-tag backfill, and Obsidian Bases]] — type (required kind) vs tags (per-bucket content facet); the 613-article backfill; Bases as the consumption-time property view; the silent-YAML landmine (T045)
- [[agentic-knowledge-engine-overview|The agentic knowledge engine — end-to-end overview]] — the canonical narrative tying together the two-file split, tiered recall, the learning loop, two memories, and OKF portability (from Max's essay, T039)
- [[llm-vault-structure-spec|LLM Vault Structure Spec — Bucket/Topic/Article Architecture]] — design document specifying Atlas's two-zone, two-layer folder architecture and consolidate/refine verbs
- [[llm-wiki-schema-template|LLM Wiki Schema Template — Simple Single-Vault CLAUDE.md]] — minimal flat wiki template (raw/ + wiki/) that Atlas extends; Karpathy's original LLM Wiki pattern
- [[zettelkasten-pkm|Zettelkasten — Foundational PKM Methodology]] — 500-year-old atomic linked-note system; Luhmann's 90,000-card proof of concept; digital lineage to wikis, Obsidian, and RAG vaults
- [[team-agentic-os-gbrain|Team Agentic OS — three-tier model and GBrain lineage]] — scaling a personal agentic OS to a team: three tiers by maintainer, access control across four surfaces, markdown-first portability
- [[agentic-knowledge-engine-team-security|Agentic knowledge engine — team security & permissions]] — T017 design note: a permission spine (GBrain + Team OS) mirrored onto files/MCP/git/memory-DB to scale Atlas without cross-bucket leaks
- [[intelligence-layer-strand-map|Intelligence-layer strand map]] — how Max's seven intelligence-layer / AI-PM strands relate: the harness lineage and the Three-Diamond umbrella
- [[memory-three-jobs-and-atlas-tiers|Memory's three jobs and Atlas's tier adoption]] — Simon Scrapes' storage/injection/recall framework benchmarked against Atlas; tier-0 snapshot + tier-2 hybrid search adopted, no rebuild; engine-core vs add-on boundary

## Related Topics

- [[harness-engineering/index|Harness Engineering]] — runtime harness patterns (Atlas is itself a harness around a wiki)
- [[claude-code-practice/index|Claude Code Practice]] — Claude Code features used to operate Atlas
- [[rpi-methodology/index|RPI Methodology]] — research methodology that feeds into knowledge bases

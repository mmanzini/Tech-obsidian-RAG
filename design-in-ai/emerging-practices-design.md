# Emerging Practices

Trends in AI-assisted design documentation as of April 2026. Confidence levels noted.

## High Confidence

### Standardisation Convergence
- Industry converging on a multi-file approach rather than a single standard
- Agentic AI Foundation standards (AGENTS.md, SKILL.md) becoming cross-tool baseline
- IDE rule systems adding import/export for compatibility
- Design documentation bifurcating into high-level intent (Design.md) and executable specs (SDD frameworks)

### Shift from Narrative to Structured Formats
- Industry moving from prose to structured formats: BDD scenarios, JSON/YAML schemas, type definitions, Mermaid diagrams
- Structured formats are token-efficient, unambiguous and machine-parseable
- Columbia University (DAPLab, 2026) confirms agents need contextual, synthetic documentation with explicit component relationships — traditional READMEs and inline comments do not help agents

### MCP as the Universal Bridge
- Model Context Protocol becoming the standard integration layer between agents and design tools
- Stitch, Figma, Penpot and code editors support MCP
- Implication for Design.md: if agents can read design context directly from Figma via MCP, the need for a separate markdown file may decrease
- Design.md's future may be as a portable fallback or human-readable documentation layer

## Medium Confidence

### Multi-Agent Documentation
- Gartner reports 1,445% surge in enterprise multi-agent enquiries
- Emerging patterns: orchestrator specs, tool catalogues (SKILL.md model), handoff artefacts with explicit state transfer
- Trend is clear but tooling is immature

### Executable Design Specifications
- Specs that generate test suites, self-validate against implementation, trigger re-runs on drift, version-control like code
- Kiro's design-first workflow, Uber's uSpec and GitHub's `/plan` command are early examples
- Direction: specifications as enforcement mechanisms, not just documentation

### Component-Embedded Documentation
- Documentation moving closer to the artefact it describes: Design.md in Figma components, SKILL.md in package repos
- Figma's MCP server integration is most significant — agents read design context directly from Figma files

## Emerging

### AI Accessibility Agents
- Dedicated accessibility agents that continuously review generated content, flag WCAG violations in real time
- Leading approach to addressing systemic [[accessibility-and-ai-design|accessibility failures]] in AI-generated UIs

### Design-to-Code Pipeline Maturation
- Evolving from Stitch → Figma → Code toward Stitch → MCP → Agent → Code (cutting out manual Figma step)
- Builder.io Fusion 1.0, v0 by Vercel and Penpot UX2Doc pursuing variations

## Open Questions

- Will Design.md survive as standalone standard or be absorbed into Figma's MCP ecosystem?
- When will standards converge? Predictions range Q4 2026 to 2027
- Who governs Design.md? No published formal schema or governance body
- Is "documentation as infrastructure" sustainable at scale? Maintenance burden is real and unquantified

## Key Takeaways

- Multi-file approach (DESIGN.md + AGENTS.md + SDD specs) is the emerging consensus, not a single standard
- MCP may reduce Design.md's role if agents can read directly from Figma — but portability remains valuable
- Executable specs (generate tests, detect drift) are the next frontier beyond static documentation
- AI accessibility agents are the most promising approach to the WCAG compliance gap
- Fragmentation is temporary but real — expect convergence by late 2026 or 2027

## Sources

- [DAPLab: Your AI Agent Doesn't Care About Your README](https://daplab.cs.columbia.edu/general/2026/03/31/your-ai-agent-doesnt-care-about-your-readme.html)
- [Figma Blog: Design Systems and AI — MCP Servers](https://www.figma.com/blog/design-systems-ai-mcp/)
- [Builder.io: Fusion 1.0](https://www.prnewswire.com/news-releases/builderio-launches-fusion-1-0--the-first-ai-agent-for-product-design-and-code-302615215.html)
- [Sitepoint: Agentic Design Patterns in 2026](https://www.sitepoint.com/the-definitive-guide-to-agentic-design-patterns-in-2026/)

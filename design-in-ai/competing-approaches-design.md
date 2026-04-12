# Competing Approaches

The landscape of AI-assisted design documentation is fragmented across three categories: SDD frameworks, portable agent instruction files and IDE-native rule systems. No single winner has emerged.

## 1. SDD Frameworks

### Kiro (kiro.dev)
- Most mature SDD platform. Three core documents: requirements.md, design.md, tasks.md
- Pioneered "Design-First Workflow" — select detail level, auto-generate design document
- **Strengths:** Most complete SDD implementation; IDE-native; enterprise adoption
- **Limitations:** Vendor-specific (AWS-affiliated); does not handle visual design tokens (that is Design.md's territory)

### GitHub Spec Kit
- Open-source, 84k+ GitHub stars. Four-command workflow: `/specify` → `/plan` → `/tasks` → implement
- Uses RFC 2119 keywords (MUST, SHOULD, MAY). Supports 14+ agent platforms
- **Strengths:** Cross-platform; large community; official GitHub backing
- **Limitations:** Requires team discipline; no visual design component

### OpenSpec (Fission AI)
- Lightweight, open-source SDD framework working with 20+ tools
- Includes delta markers (ADDED / MODIFIED / REMOVED) for brownfield projects
- **Strengths:** Lightest-weight SDD option; tool-agnostic; good brownfield support
- **Limitations:** Smaller ecosystem

## 2. Portable Agent Instruction Files

### AGENTS.md
- Repository-level context for all AI agents (build, test, code style, architecture)
- Stewarded by the Agentic AI Foundation (Linux Foundation)
- Reported 28.6% agent runtime reduction across 124 PRs
- **Focus:** Code conventions, not design

### SKILL.md
- Reusable procedures with YAML frontmatter and progressive disclosure
- Agents load only name + description at startup, retrieve full docs on demand
- **Focus:** Procedures, not design

## 3. IDE-Native Rule Systems

### Cursor Rules
- Project rules in `.cursor/rules/` with gitignore-style file pattern matching
- Rules auto-activate when matching files are referenced
- **Limitations:** Cursor-specific; rules do not transfer to other tools

### Windsurf
- Cognition AI's IDE with MCP support, one-click plugin setup
- Arena Mode for side-by-side model comparison
- **Limitations:** Newer entrant; less community content

## 4. Emerging Patterns

- **Living Architecture** — Structured architecture docs (10 core sections, 3 depth levels) with Mermaid C4 diagrams
- **Anthropic Three-Agent Harness** — JSON feature specs, enforced testing at each handoff, commit-by-commit tracking

## How Design.md Fits

Design.md operates at a different layer from most competitors:
- **DESIGN.md** → visual design system (colours, typography, spacing)
- **SDD frameworks** → technical specifications (architecture, requirements, tasks)
- **Agent instruction files** → code conventions (build, test, style)

The practical recommendation: use them together. They are complementary, not competing.

## Comparison Matrix

| Approach | Scope | Portability | Maturity |
|----------|-------|-------------|----------|
| Design.md | Visual system | High | Early |
| Kiro | Full SDD | Medium (IDE-bound) | Mature |
| Spec Kit | Full SDD | High | Mature |
| OpenSpec | Full SDD | High | Growing |
| AGENTS.md | Code conventions | High | Emerging standard |
| SKILL.md | Reusable procedures | High | Established |
| Cursor Rules | Editor behaviour | Low (Cursor only) | Mature |

## Key Takeaways

- No single standard covers the full workflow — teams combine approaches
- Design.md fills a unique niche: visual design system for LLM consumption
- SDD frameworks handle technical specs; agent files handle code conventions; Design.md handles visual consistency
- Interoperability is improving but fragmentation is real
- AGENTS.md + DESIGN.md at the repo root is the emerging baseline combination

## Sources

- [Kiro: Agentic AI Development IDE](https://kiro.dev/)
- [GitHub Spec Kit](https://github.github.com/spec-kit/)
- [OpenSpec](https://github.com/Fission-AI/OpenSpec)
- [GitHub Blog: How to Write a Great AGENTS.md](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/)
- [Anthropic Three-Agent Harness](https://www.infoq.com/news/2026/04/anthropic-three-agent-harness-ai/)

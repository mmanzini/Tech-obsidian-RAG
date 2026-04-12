# Historical Context — Design Tokens

Design.md is the latest iteration in a 15-year progression of making design decisions machine-readable and version-controllable.

## Timeline

### Living Style Guides (2010s)
- First serious attempt at design-as-code — auto-generated docs synchronised with actual code
- **Pattern Lab** (Brad Frost, Atomic Design): atoms → molecules → organisms → templates → pages
- **What worked:** Atomic hierarchy remains influential across Figma, Storybook and component libraries
- **What failed:** Prebuilt systems required heavy customisation; adoption depended on organisational change, not tooling
- **Lesson:** Tooling does not equal adoption. Organisational buy-in is the actual bottleneck

### Design Tokens (2017-2019)
- Concept originated at Salesforce (Jina Anne); first production tool: **Theo**
- Amazon open-sourced **Style Dictionary** (2017, Danny Banks) — dominant cross-platform token build system
- Key innovation: define colours, spacing, typography once; transform for any platform (CSS, iOS, Android)
- **What worked:** Single-source-of-truth model proven. Style Dictionary still widely used
- **What failed:** JSON format — excellent for machines, less friendly for non-technical team members
- **Lesson:** Machine readability and human readability serve different audiences. Design.md chose human readability (markdown) at the cost of formal validation

### W3C Design Tokens Community Group (2020-2025)
- Assembled to standardise exchange format for design decisions (too many proprietary formats)
- Milestones: First draft (2022), iterative refinement (2023-24), **stable version 2025.10**
- Vendor-neutral JSON format with `$value`, `$type`, `$description` fields
- Supports theming, modern colour spaces, cross-tool interoperability
- Reference implementations: Style Dictionary v4, Tokens Studio, Terrazzo, 10+ design tools

## Design.md vs W3C Design Tokens

| Aspect | W3C Design Tokens | Design.md |
|--------|-------------------|-----------|
| **Format** | JSON (strict) | Markdown (flexible) |
| **Primary audience** | Build systems, design tools | AI agents, humans |
| **Governance** | W3C Community Group (formal) | Google/community (informal) |
| **Scope** | Token *values* (what) | Values + rationale + guardrails (what + why + don't) |
| **Machine parsing** | Unambiguous | Forgiving, context-dependent |
| **LLM token efficiency** | Lower (JSON syntax overhead) | 34-38% more efficient |

**Key distinction:** W3C tokens answer "what" (values). Design.md answers "what, why and don't" (values, rationale, guardrails). Complementary formats for different consumers.

## The Integration Gap

No automated bidirectional sync exists between Design.md and token systems. Current workflow:

1. Define tokens in Figma (visual design tool)
2. Export as W3C tokens or Style Dictionary (build system)
3. Manually author Design.md from the same tokens (AI agent context)
4. Keep them in sync manually

This triple-maintenance problem is the most significant practical challenge for teams adopting Design.md alongside existing infrastructure.

## Why Previous Approaches Failed

Research consistently identifies **organisational factors** over tooling:
- Systems built in isolation from product teams
- Lack of advocacy, training and senior leadership support
- Confusion between adoption metrics and actual value
- Unmaintained systems that drift faster than they are corrected

Design.md's simplicity (plain markdown, no build system) lowers the adoption barrier. But the maintenance burden — keeping Design.md synchronised with Figma, code and token definitions — may recreate the same drift problem that killed living style guides.

**Honest assessment:** Design.md is promising but unproven at scale. Success depends on organisational adoption, not format quality.

## Key Takeaways

- 15-year lineage: living style guides → Atomic Design → design tokens → W3C spec → Design.md
- W3C Design Tokens (JSON) and Design.md (markdown) are complementary — different formats for different consumers
- Triple-maintenance (Figma + tokens + Design.md) is the biggest practical risk
- Every previous design-as-code approach failed primarily on organisational adoption, not technical merit
- Design.md's simplicity is its main advantage and its main risk (easy to start, unclear if maintenance scales)

## Sources

- [W3C Design Tokens Specification 2025.10](https://www.designtokens.org/tr/drafts/format/)
- [Smashing Magazine: What Are Design Tokens? (Jina Anne)](https://www.smashingmagazine.com/2019/11/smashing-podcast-episode-3-with-jina-anne-what-are-design-tokens/)
- [AWS Open Source: Style Dictionary](https://aws.amazon.com/blogs/opensource/style-dictionary-trust-design-consistency/)
- [Knapsack: Why Design Systems Fail](https://www.knapsack.cloud/blog/why-design-systems-fail)
- [Improving Agents: Best Nested Data Format for LLMs](https://www.improvingagents.com/blog/best-nested-data-format/)

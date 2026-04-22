# AI Prototyping for Product Managers

**Source:** Colin Matthews, guest post on [Lenny's Newsletter](https://www.lennysnewsletter.com/) — *A guide to AI prototyping for product managers* (2025-01-07)

PRDs-to-working-prototypes in minutes, no code. The core shift: a PM can sketch a feature, validate it with users, and iterate before a line of production code is written.

## Tool categories

| Category | Examples | Best for |
|---|---|---|
| **Chatbots** | ChatGPT, Claude (Artifacts) | Single-page prototypes, data viz, one-offs |
| **Cloud dev environments** | v0, Bolt, Replit, Lovable | Multi-page, multi-feature prototypes, hosted |
| **Local dev assistants** | Cursor, Copilot, Windsurf, Zed | Production code work for people who code |

### Cloud environments — when to pick which

- **v0** — beautiful defaults (Next.js + Shadcn), real cloud hosting. Pick for polished UI.
- **Bolt** — quick prototypes, flexible design, server runs in-browser (no real auth/DB unless you add Supabase).
- **Replit** — full-stack with JS or Python, excels at internal tools and data-driven apps.
- **Lovable** — integrates with GitHub, Supabase, Anthropic/OpenAI. Great for apps you actually want to ship. Weakness: no code editor — everything via prompt.

## The four techniques that unlock good results

1. **Reflection** — force a plan *before* code. *"Start by detailing minimum requirements. Do not write any code."* Also use it to debug: ask for a list of possible causes, not a fix.
2. **Batching** — counter-intuitive: *less* up-front context is better. Build the smallest working iteration first, then extend. Start with the data model.
3. **Be specific** — hyper-specific prompts act like well-written tickets for a junior engineer. Name files, list behaviours, call out exact UI elements.
4. **Manage lost context** — when the agent rewrites everything, you've underspecified. Use checkpoints to roll back; use reflection + batching + specificity to avoid re-triggering.

## Prompt patterns worth saving

- **Figma → prototype (Bolt):** *"Build a prototype to match this design. Match it exactly. Use Tailwindcss. Match styles, fonts, spacing, and colors."* + screenshot.
- **Scratch with good UI (v0):** *"Build a prototype for [x]. This tool should: [behaviour 1/2/3]. Implement a simple initial iteration that meets these exact requirements."*
- **Dashboard (Replit):** *"Build a prototype for [x]. Use Python and Streamlit."*
- **Sketch → prototype (v0):** *"Convert the hand-drawn sketch to a functional prototype. Focus on frontend functionality. Make it in the style of [product you like]."*
- **PRD → prototype (Bolt):** *"Implement a prototype to match the features in this PRD... Focus on front end functionality — do not include a server or database."*

## Key Takeaways

- The 10-minute prototype collapses the Figma→user-test loop that used to take weeks.
- Match the tool to the task: v0 for polish, Bolt for speed + design, Replit for data/internal tools, Lovable for productionisable integrations.
- The discriminator between "magic" and "rewrites my whole app" outputs is **reflection → batching → specificity**, not which model you picked.
- Use the prototype for discovery, not production: ship it to a few users, get interactive feedback, *then* write a spec.

## Related

- [[product-management/pm-guide-to-evals|PM guide to evals]] — once your prototype works, how do you measure if it's any good?
- [[ai-product-development/ai-product-development-framework|AI Product Development Framework]]
- [[ai-product-development/vibe-coding-what-people-ship|What people are vibe-coding]] — 50 real examples built with these tools
- [[harness-engineering/_index|Harness Engineering]] — for PMs graduating into local IDEs and agent harnesses
- [[ai-organization/an-ai-glossary|AI glossary]] — definitions for the terms above (RAG, fine-tuning, MCP, etc.)
- [[lennys-podcast/zevi-arnovitz|Zevi Arnovitz on Lenny's Podcast]] — non-technical PMs shipping real products with Cursor + Claude Code
- [[lennys-podcast/lazar-jovanovic|Lazar Jovanovic on Lenny's Podcast]] — 80% planning / 20% building with a Markdown PRD stack

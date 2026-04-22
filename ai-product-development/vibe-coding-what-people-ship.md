# What People Are Vibe-Coding (and Actually Using)

**Source:** Lenny Rachitsky, [Lenny's Newsletter](https://www.lennysnewsletter.com/p/what-people-are-vibe-coding-and-actually) (2025-07-08) — 50+ examples crowdsourced from 1,000+ replies on X / LinkedIn / Slack.

Field survey of what non-technical people are actually shipping with AI coding tools — and, critically, *using daily themselves*.

## Tool popularity (from the responses)

**Top tier:** Cursor, Claude Code, Replit, Lovable
**Second tier:** v0, Bolt, ChatGPT
**Honorable mentions:** Gemini, n8n, Zapier Agent, Warp, Windsurf

See [[product-management/ai-prototyping-for-pms|AI prototyping for PMs]] for how to pick between them.

## Headline patterns

- **Almost no two examples are alike.** Everyone is solving their own hyper-specific problem — group drafting app for a multi-sports fantasy league, cats-and-sushi browser game, eyelash-tracking with photos. Welcome to **n-of-1 personalised software**.
- **Chrome extensions are booming.** People build for the surface they live on.
- **Solo-built tools go viral.** Several examples started as a scratch-your-own-itch side project and now serve hundreds or thousands (e.g. "How many layers today?" weather app → 85K users in 9 months).
- **The gender ratio is more balanced than typical tech discourse.** Women vibe-coding is a dominant cohort.
- **The starting prompt is often just plain English** — describe what you want as if speaking to a remote engineer; iterate visually.

## Categories of what gets built

### Health, wellness, style
- **Carb counter** (Morgan Brown, Replit) — for managing son's blood glucose
- **"How many layers today?"** (Vijith Quadros, Lovable) — 85K users, 9 months
- **Lash tracker** (Jackie Bavaro, Replit) — photo + style log
- **Stupidly specific workout app** (Faraz Khan, v0 + Claude Code)
- **MealMuse** (Nick Markman, Lovable + Cursor) — fridge photo → recipes + shopping list
- **Nicotine pouch taper tracker** (first-ever app, Cursor + SwiftUI, shipped to App Store)

### Parenting and family
- **Storypot** (emoji-drag story builder for kids, Replit) — 60+ families
- **My Baby Logger** (Lovable) — feedings, sleep, diapers, meds
- **Bedtime story generator** (Bolt + Supabase) — reflect on the day → personalised story for the kid
- **Kid budgeting / allowance app** (Claude Code)
- **Personalised AI greeting cards** (Cursor + Windsurf + Claude Code)

### Work productivity
- **Meeting prep automation** (Marissa Goldberg, Zapier Agent) — morning calendar scan → prep brief per attendee
- **Standup order randomizer** (Rob Balderstone, Lovable) — daily use
- **Availability Chrome extension** (Olena Vozna, Replit) — auto-adds free slots to email replies in natural language
- **Inbox focus Gmail add-on** (Ari Klein, Gemini) — holds email, delivers on schedule, VIP allowlist breaks through
- **Accomplishments tracker** (Kevin Kirkpatrick, Lovable) — "I hate updating my resume"

### Personal productivity
- **Buzzerbee** (Bolt + Cursor) — auto-answers apartment buzzer based on delivery rules
- **Bill splitter** (Lovable + Unicorn Studio) — tip-first, one-screen, calculates before the check arrives
- **Curated.now** (Replit) — personalised restaurant recs for friends (RAG + LLM over a local DB)

### Personal development
- **Conversation intelligence** (Peter Nixey, Claude Code) — paste call transcript → "what you did well / what to improve"
- **Daily language-learning newsletter** (Dustin Coates, v0) — Gemini-generated story in target language from that day in history + vocab + grammar
- **AI journal / therapy companion** (Replit)

### Other/fun
- **Rehearsal** (Jean Kaddour, Cursor + Next.js) — 10-min RPG: convince an AI character (hostage taker, broke flatmate). He and his girlfriend play daily.
- **VisaMonkey** (v0) — immigration document manager for 10–20-year filing journeys
- **Figma plugins for design system migration** (Kayt Wilson, Cursor + ChatGPT) — niche internal tooling
- **Battery Lens Mac app** (ChatGPT + some Claude) — on-screen battery overlay
- **Local PDF-only video-to-GIF converter** (Claude Code) — tired of sketchy websites
- **Anki flashcard generator** for son learning German

## How Lenny himself used it

- [YouTube thumbnail preview tool](https://project-youtube-podcast-title-and-thumbnail-previewer-348.magicpatterns.app) (Magic Patterns)
- [Podcast-clip tweet crafter](https://tweet-punch-perfect-posts.lovable.app/) (Lovable)
- [Most-mentioned-books tracker](https://v0-lenny-s-book-mentions.vercel.app/) (v0, data placeholder)

## Key Takeaways

- **The vibe-coding thesis is playing out in the field.** Non-technical people are shipping daily-used tools solo, often in a day or a weekend.
- **Chrome extensions + mobile utilities** are the dominant surfaces — build where you spend your time.
- **Scratch your own itch, then let it spread.** Multiple examples built for self → thousands of users within months without a team.
- **Tool choice is pragmatic**, not dogmatic — many examples chain 2–3 tools (v0 for design, Cursor for logic, Claude Code for stuck moments, Supabase for DB).
- **The n-of-1 era.** Personalised software for one person (or one family) now has positive ROI to build.
- Common starting move: describe the product in plain English, iterate on what you see.

## Related

- [[product-management/ai-prototyping-for-pms|AI prototyping for PMs]] — tool selection framework
- [[ai-product-development/_index|AI Product Development]]
- [[harness-engineering/use-claude-code-more|Everyone should be using Claude Code more]]
- [[ai-organization/ai-fluency-curriculum|AI fluency curriculum]]
- [[lennys-podcast/zevi-arnovitz|Zevi Arnovitz]] — non-technical PM vibe-coding workflow (Cursor + Claude Code + /commands)
- [[lennys-podcast/lazar-jovanovic|Lazar Jovanovic]] — 80% planning / 20% building; Markdown PRD stack
- [[lennys-podcast/simon-willison|Simon Willison]] — November 2025 threshold and the lethal trifecta for agent-built apps

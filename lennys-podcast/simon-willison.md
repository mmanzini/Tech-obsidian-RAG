# Simon Willison — We've Passed the Inflection Point

**Source:** Lenny's Podcast, Simon Willison (co-creator of Django, creator of Datasette, coined "prompt injection," AI commentator at simonwillison.net)

Simon's 2026 "AI state of the union": November 2025 was the threshold moment when coding agents crossed from "mostly works" to "almost always does what you told it." Now the bottleneck isn't typing code — it's judgment, supervision, and sustaining clear thinking under continuous pressure. Simon is writing a book about agentic engineering one blog chapter at a time and running four Claude instances in parallel before lunch.

## The November 2025 inflection point
- GPT 5.1 and Claude Opus 4.5 didn't look like giant leaps on paper — but together they crossed a threshold where agent output went from "buggy pile of rubbish" to reliably doing what it was told.
- Reinforcement learning on code + reasoning traces (since OpenAI o1 in late 2024) turned code into the labs' primary training domain because code is verifiable — it runs or it doesn't.
- Engineers who tinkered over the holidays had a collective "oh wow, this actually works" moment in January/February 2026.
- 95% of Simon's code is no longer typed by him; the latest models are faster at refactors than fingers on a keyboard.

## Dark factories and the StrongDM experiment
- Two rules: **nobody types code, nobody reads code.** What does software production look like then?
- StrongDM (an actual security-software company, not a hobbyist) ran a swarm of AI-simulated employees 24/7 inside a simulated Slack/Jira/Okta stack, spending ~$10,000/day on tokens as a living QA team.
- They even had coding agents build fake versions of Slack/Jira/Okta from public API docs so they could bypass rate limits.
- Simon's framing: a QA team that never sleeps is a *different product* from chat-based agent coding — it's the shape of the "dark factory" (fully automated, lights-off software production).
- See also [[harness-engineering/_index|Harness Engineering]] for review-less workflows.

## The new middle problem
- AI is eating the *middle* of every hierarchy: middle of the build process, middle-career engineers.
- Experienced engineers: amplified — 25 years of pattern recognition becomes a one-sentence prompt that nails a bug.
- Juniors: onboarding collapses — Cloudflare and Shopify each hired 1,000 interns in 2025 because ramp time fell from a month to a week.
- Mid-career engineers are the squeezed tier — they've already captured the beginner boost and haven't built enough expertise to amplify. Differentiation now matters more than tenure.

## AI exhaustion and the new cognitive limit
- Simon runs four agents in parallel, is mentally wiped by 11 a.m.
- AI **reduces execution effort but increases cognitive load** — work shifts upstream into supervision, orchestration, and judgment.
- Sleep-loss stories (staying up to keep agents running) are an addictive-gambling dynamic; Simon hopes it's a novelty phase but warns good companies must manage expectations, not demand 5x output.
- The scarce resource is no longer labor; it's sustained clear thinking under continuous pressure.

## Agentic engineering patterns
- **Red/green TDD** — 5-word prompt that encodes "write failing test, watch it fail, write code, watch it pass." Agents recognize the jargon, ship better results.
- **Start from a skeleton template** — one passing test and your indentation style is enough; agents mimic existing patterns more faithfully than they follow prose in CLAUDE.md.
- **Hoard everything you've tried** — Simon keeps `simonw/tools` (193 HTML/JS tools) and `simonw/research` (75 agent-run research projects) in public GitHub. Agents can search them to recombine prior work.
- **Prototype in threes** — with prototyping ~free, build three variants of every feature and compare. Don't let the AI simulate users though; usability testing still needs humans.
- **YOLO mode on their servers, not yours** — Claude Code for Web runs on Anthropic infra so "dangerously skip permissions" is safe. Running YOLO locally is where damage happens.

## The lethal trifecta and the coming Challenger disaster
- Agent security is fundamentally unsolved whenever an agent has: **(1)** access to private data, **(2)** exposure to untrusted content (emails, web pages), **(3)** the ability to exfiltrate externally (send email, call APIs).
- Prompt injection cannot be reliably prevented — it's not a patch, it's a design constraint.
- Simon has predicted an AI "Challenger disaster" every six months for three years. Hasn't happened; every near-miss institutionally reinforces overconfidence (the O-ring analogy).
- See [[ai-security/_index|AI Security]].

## Lenny's LinkedIn takeaways
Lenny's post-episode summary distilled six points — the headline framing was: *AI reduces execution effort but increases cognitive load. We're not removing work, we're shifting it upstream into supervision, orchestration, and judgment.*
1. November 2025 inflection point — "mostly works" to "almost always does what you want."
2. Mid-career engineers are the most vulnerable; juniors and seniors both benefit.
3. AI exhaustion is real and underestimated.
4. The StrongDM dark-factory experiment is the most radical live experiment in AI-assisted development.
5. Red/green TDD is the single highest-leverage agentic pattern.
6. The lethal trifecta makes agent security a fundamentally unsolved problem.

## Key Takeaways
- November 2025 crossed the "mostly works → almost always works" threshold for coding agents.
- The middle gets squeezed: mid-career engineers, middle steps of the dev process.
- AI trades labor for cognitive load — the new scarce resource is sustained judgment.
- "Nobody types, nobody reads code" is already running in production at companies like StrongDM.
- Red/green TDD and skeleton templates outperform long prose prompts — jargon is the dense encoding.
- The lethal trifecta (private data + untrusted content + exfiltration) makes a Challenger-style AI disaster a matter of when, not if.

## Related
- [[harness-engineering/_index|Harness Engineering]] — templates, skills, review-less workflows
- [[agent-workflows/_index|Agent Workflows]] — TDD patterns, prototyping-in-threes
- [[ai-security/_index|AI Security]] — lethal trifecta and prompt injection
- [[ai-organization/_index|AI & Organisation Design]] — dark factories and the new middle
- [[ai-product-development/_index|AI Product Development]] — prototyping is free, ideas are the bottleneck
- [[agent-architecture/lethal-trifecta-and-agentic-patterns|Lethal Trifecta and Agentic Patterns]] — Lenny's takeaways article on the same trifecta framework
- [[lennys-podcast/_index|Lenny's Podcast]]

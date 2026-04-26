# Cat Wu — Shipping Faster than Anyone Else at Anthropic

**Source:** Lenny's Podcast, Cat Wu (Head of Product, Claude Code & Cowork, Anthropic)

Cat runs product for Claude Code and Cowork alongside Boris Cherny. Her core thesis: AI has compressed feature timelines from 6 months to 1 week (sometimes 1 day), so the PM job is now to *minimise the distance from idea to user*. Process-light, research-preview-by-default, hire engineers with product taste, get the right amount of "AGI-pilled," and treat skills/evals as the new PRD. The PM team is ~30–40 people across research, CDP, Claude Code, enterprise, and growth. Mission ("safe AGI") trumps any individual product line — that's what makes cross-org trade-offs fast.

## Compressed timelines change the PM job

- Pre-AI cadence: 6–12 month roadmaps, heavy partner-team alignment because code was expensive.
- Now: features ship in 1 month, sometimes 1 week, sometimes 1 day.
- Implication: stop coordinating multi-quarter roadmaps; **build a "concept corner" of the product where any engineer or PM can take an idea to users by end of week**.
- The best PMs are the ones who shorten time-from-idea-to-user and decide which tasks must work out of the box.
- Echoes [[ai-organization/claude-code-productivity-takeaways|Boris Cherny]]'s "build for the model six months from now" — Cat is the *path-from-today-to-vision* counterpart to Boris's vision-setting.

## Process for shipping in a week

Three repeatable moves:

1. **Set crisp goals.** LLMs are general → ambiguity is the default. A PM's job is to nail the user, the problem, and the success criterion (e.g. "professional developers at enterprises safely getting to zero permission prompts").
2. **Ship in research preview by default.** Brand it explicitly. This lowers the commitment — features can ship in a week or two, may not be supported forever, and the team gets feedback fast.
3. **Build the cross-functional framework once.** Claude Code has an "evergreen launch room": engineer posts a dogfooded feature → docs (Sarah), PMM (Alex), DevRel (Tar, Lydia) jump in and ship the announcement next day. PM's job is to set up this rail, not to gate every shipment.

## The metrics + principles spine

What replaces the PRD:

- **Weekly metrics readouts with the entire team.** Everyone understands every facet of the business — what the goals are, how they're trending, what drives them.
- **Team principles list.** Who the key users are, why they're key, what trade-offs are acceptable. This is what lets engineers ship without blocking on PM.
- **PRDs only for ambiguous features or heavy-infra projects.** Most features get a one-pager: goals, delightful use cases, current failure modes.

## Hire engineers with product taste

- Anthropic optimises for **engineers with product taste** over hiring more PMs.
- Many Claude Code engineers go from Twitter feedback to shipped feature in a week with almost no PM involvement.
- Most of Cat's PMs were engineers (or shipped code at Anthropic); designers were front-end engineers.
- Counterpoint to [[ai-organization/anthropic-growth-takeaways|Amol Avasare]]'s "we need more PMs because engineers ship faster than PMs/designers can keep up" — same observation, opposite lever. Anthropic chose product-minded engineers; Amol's framing is hire more guides.
- Why eng background helps *for now*: you have a sense of how hard something is, which informs prioritisation. That edge erodes as models keep improving.

## The right amount of AGI-pilled

- The hardest skill: **define what the product should be one month out**, not three.
- "It's very easy to build for the super-AGI strong model" — just give it a text box. The hard thing is eliciting maximum capability *from the current model*: golden paths, leveraging strengths, patching weaknesses.
- New models are "removing features that are no longer needed" — e.g. the to-do list was a crutch for early Claude Code; later Opus/Sonnet models use it spontaneously, so the team de-emphasised it.
- Every model launch → re-read the system prompt and remove sections the model no longer needs (the model "eats your harness for breakfast").
- New models also unlock features that were previously too inaccurate (e.g. multi-agent code review only worked from Opus 4.5/4.6 + Sonnet 4.6 onwards).

## Talking to the model + evals as the new PRD

- High-leverage habit: **ask the model to introspect on its own behaviours.** When it does something unexpected (front-end change without using the UI), ask why — often it'll surface a confusing system prompt, a missed verification step, a sub-agent that didn't check its work. Then patch the harness.
- Identify your **top-five trusted taste-testers.** Not all feedback is equally qualified; find the people who can articulate what makes a model+harness combo good.
- **Evals are underappreciated.** You don't need hundreds — 10 great evals quantify the goal and surface gaps. Cat sees this as "the future of PM work" because evals concretely define success.
- Internally Amanda (Claude's character) is the role model — she molds character despite the task being even more ambiguous than coding.
- See [[hamel-husain-shreya-shankar|Hamel Husain & Shreya Shankar]] for the discipline of eval/trace-driven PM work and [[ai-product-development/_index|AI Product Development]] for the frameworks.

## Mission > product line

- The unifying mission ("safe AGI for humanity") is what lets Anthropic make cross-org trade-offs fast.
- "If Claude Code failed but Anthropic succeeded, I'd be extremely happy" — and the team genuinely operates that way.
- Concrete: the OpenClaude (third-party Claude on subscription) deprecation was a mission call — first-party reach > third-party convenience because subscriptions scale users and infra fastest.
- Mission is how Anthropic stayed focused while OpenAI fanned out into social/feed surfaces.
- See [[ai-organization/openclaw-personal-agent-team|Claire Vo on OpenClaw]] for the third-party-tool side of the same story.

## Claude Code vs Desktop vs Cowork — when to use which

- **Claude Code (CLI):** one-off coding tasks, latest features land here first, most powerful surface.
- **Claude Desktop:** front-end work (preview pane open beside chat), graphical control plane that aggregates CLI/desktop/web/mobile sessions, friendlier for non-technical users.
- **Claude Code on web/mobile:** kick off tasks on the go without lugging a laptop ("touching grass"). Solves the airplane-Wi-Fi meme.
- **Cowork:** for any work whose **output is not code** — slide decks, customer briefs, inbox triage, launch plans. Output type is the splitting rule.

## Cowork in practice — connect everything

- Pre-condition: connect calendar, Slack, Gmail, Drive — Cowork is only as good as its context.
- Worked example: Cat fed it a conference talk brief (PMM's draft, the narrative, links). Cowork worked for an hour, walked Twitter, the launch room, and the announce channel, and produced a 20-page deck overnight that "looked like an Anthropic designer made it" (because the design system was connected).
- Pattern at Anthropic: applied-AI ("PI") teams use Cowork to prep customer dossiers — meetings tomorrow, action items from past meetings, latest feature ETAs scraped from Slack.
- Engineering is biggest token spender; **applied-AI ("forward-deploy-engineering" GTM)** is second.
- Internal "personalised work software" surge: people build custom apps with Claude Code instead of stretching SaaS to fit. Example: a templated deck-builder pulling Salesforce + Gong + customer notes to spit out tailored decks in seconds.

## Slack as the anti-bot OS

- Anthropic runs on Slack — "the OS of our company."
- Why nobody disrupts it: real-time messaging is a durable, hackable primitive. Slack bots are how Anthropic glues internal tools together. Salesforce gets disrupted; Slack doesn't.

## Calmness in the chaos

- "Lean into the chaos. Face every challenge with a smile." Burnout is the failure mode if you take every P0 too seriously.
- Hire people who've been through cycles before — they know what gives them energy.
- "Sunday P0 → Monday P0 → Monday-afternoon P00000" — brutal prioritisation + sleep > completeness.
- See [[nick-turley|Nick Turley]] on the leader's resting-heart-rate as a transmissible quality.

## What humans still bring

- **Common sense the model lacks**: knowing the stakeholders, their preferences, the right venues to talk to them, the thousand small things that go wrong in any launch.
- Picking what to build (taste).
- Verifying the output is actually good and getting it out the door.

## Vision: 1 task → 6 → 50 → hundreds of agents

- Building block = **single task done well** (clear prompt → consistent acceptable output).
- 2025 saw multi-coding (6 tasks at a time); the extrapolation is 50–100+ agents per user.
- Implications: agents will run remotely (laptop RAM is the limit), the interface becomes "which tasks need my attention," verification becomes the bottleneck, and feedback must compound (the model never makes the same mistake twice).
- Connects to [[harness-engineering/_index|Harness Engineering]] (multi-agent runtime) and [[scott-wu|Scott Wu]] / [[bret-taylor|Bret Taylor]] on the agent-as-teammate progression.

## Advice to PMs entering the AI era

- **Automate the tedious parts of your own job.** Find the repetitive task you do; iterate on the automation until it's truly 100%.
- "**95% accuracy isn't an automation** — that last 5–10% is what makes it reliable. Put in the elbow grease."
- Build apps you actually use every day, not throwaway prototypes — only daily usage teaches you what to fix.
- But: **don't over-customise your harness** at the expense of doing real work. There's a camp that obsesses over MCPs/skills/workflows and forgets to ship.
- The 2024 chat-based generation was "ask and tell"; the 2026 Claude-Code generation is **action-based**. The aha moment is when the agent *does* things on your behalf rather than telling you what to do.

## Key Takeaways

- Compressed timelines (6mo → 1 week) reframe PM work: minimise idea-to-user distance, ship in research-preview by default.
- Replace the PRD with weekly metrics readouts + a written team-principles list. PRDs only for ambiguous or infra-heavy projects.
- Hire **engineers with product taste** rather than more PMs (Anthropic's lever; Amol's lever is the inverse).
- Be the right amount of **AGI-pilled** — build for the *current* model's max capability, not for a hypothetical super-model.
- Every model launch: prune the system prompt, remove crutches the model no longer needs, look for newly-unlocked features.
- Evals are the new PRD — 10 great evals beat hundreds of mediocre ones.
- Mission > product line is what enables fast cross-org trade-offs (OpenClaude deprecation as the canonical example).
- Output-type splits the tool: code → Claude Code/Desktop, anything else → Cowork; mobile/web for on-the-go.
- Connect everything to Cowork — context-completeness is the quality lever.
- 95% accuracy isn't automation; the last 5–10% is the value.
- Calmness + first-principles + "jobs are fake, just do the things" — the operator's posture for the chaos.

## Related

- [[ai-organization/claude-code-productivity-takeaways|Boris Cherny]] (in [[ai-organization/_index|AI & Organisation Design]]) — Cat's tech-lead counterpart on the same team
- [[ai-organization/product-management-ai-exponential|Product Management on the AI Exponential]] — Cat Wu's companion Anthropic blog post covering the same themes
- [[ai-organization/anthropic-growth-takeaways|Amol Avasare]] — opposite lever (more PMs vs more product-minded engineers)
- [[ai-organization/openclaw-personal-agent-team|Claire Vo]] — third-party-tool perspective on the OpenClaude shift
- [[sherwin-wu|Sherwin Wu]] — OpenAI-side parallel: 95% engineer adoption, context-files-as-code
- [[hamel-husain-shreya-shankar|Hamel Husain & Shreya Shankar]] — eval/trace discipline that Cat calls "the future of PM"
- [[nikhyl-singhal|Nikhyl Singhal]] — PM in renaissance and crisis; "how modern are you?"
- [[nick-turley|Nick Turley]] — leader's resting heart rate; the model is the product
- [[chip-huyen|Chip Huyen]] — what actually improves AI apps (users, data prep, prompts)
- [[ai-product-development/_index|AI Product Development]] — Sandbox Discovery, Modular Context, the frameworks Cat operates inside
- [[harness-engineering/_index|Harness Engineering]] — system-prompt pruning, multi-agent runtime
- [[product-management/_index|Product Management]] — framework layer above this episode
- [[lennys-podcast/_index|Lenny's Podcast]]

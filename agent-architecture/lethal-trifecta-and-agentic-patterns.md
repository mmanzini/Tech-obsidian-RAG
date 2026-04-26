# Lethal Trifecta and Agentic Engineering Patterns

Lenny Rachitsky's takeaways from Simon Willison (April 2026) — covering both an unsolved security model for AI agents and the highest-leverage workflow patterns experienced operators have converged on.

## November 2025: The Inflection Point

GPT 5.1 and Claude Opus 4.5 crossed a threshold where coding agents went from "mostly works" to "almost always does what you want." Engineers who tinkered over the holidays realised the technology had become genuinely reliable.

## The Lethal Trifecta (Security)

Whenever an AI agent simultaneously has:

1. **Access to private data**, AND
2. **Exposure to untrusted content** (e.g. incoming email, fetched web pages), AND
3. **The ability to send data externally** (e.g. reply to email, call APIs)

…you have a **lethal trifecta**. Prompt injection — malicious instructions embedded in untrusted text — cannot be reliably prevented. Simon has predicted a "Challenger disaster" for AI security every six months for three years; it hasn't happened yet, but he's confident it will.

This is the root reason the [[openclaw-personal-agent-team|OpenClaw playbook]] insists on read-only tokens and warns against group chats with agents.

## Red/Green TDD as the Highest-Leverage Pattern

The single most effective agentic engineering pattern: have the agent **write tests first → watch them fail (red) → write the implementation → watch them pass (green).** Materially better results than asking for code directly.

The five-word prompt **"use red/green TDD"** encodes the entire workflow because the agents recognise the jargon. This belongs alongside the patterns in [[research-plan-implement|Research Plan Implement (RPI)]] and [[quick-dev|Quick Dev]].

Once the test exists, implementation becomes a **search problem, not a design problem** — the agent can try five approaches and the test does the judging for you.

## The Dark Factory

StrongDM established a radical policy: **nobody writes code, nobody reads code.** They run a swarm of AI-simulated end users 24/7 — thousands of fake employees making requests — at ~$10,000/day in token costs. They had agents build simulated Slack/Jira/Okta from the API docs so they could test without rate limits. The most extreme experiment in AI-assisted development happening right now.

## Mid-Career Engineers Are Most Vulnerable

Counter-intuitive: not juniors, not seniors.

- **Seniors** are amplified by AI through decades of pattern recognition.
- **Juniors** ramp dramatically faster — Cloudflare and Shopify each hired ~1,000 interns because AI cut onboarding from a month to a week.
- **Mid-career** engineers haven't accumulated deep expertise *and* have already captured the beginner boost — squeezed from both sides.

## AI Exhaustion Is Real

Simon runs four coding agents in parallel and is mentally wiped out by 11 a.m. Time savings are visible; cognitive load increase is invisible until burnout. Managing AI amplifies cognitive load even as it reduces labour. Good companies will manage expectations rather than expecting 5x output indefinitely.

## Key Takeaways

- The lethal trifecta is a **structural** security limit, not an engineering bug — design agents so at least one leg is missing.
- Red/green TDD turns implementation from a design problem into a search problem; this is the cheapest workflow upgrade available.
- The productivity narrative hides a cognitive-load tax — execution shifts upstream into supervision and judgment.
- Career risk in the AI transition concentrates in the middle of the experience curve, not at the edges.

## Related

- [[twelve-factor-agents|Twelve-Factor Agents]] — production-grade principles, including factor-by-factor mitigations for parts of the trifecta
- [[research-plan-implement|Research Plan Implement (RPI)]] — structured workflow that pairs naturally with red/green TDD
- [[adversarial-review|Adversarial Review]] — human-side counterpart to the test-as-judge pattern
- [[openclaw-personal-agent-team|OpenClaw — A Personal Team of AI Agents]] — real-world security cautions derived from the trifecta
- [[claude-code-productivity-takeaways|Claude Code Productivity — Boris Cherny Takeaways]] — the productivity claims this article tempers
- [[lennys-podcast/simon-willison|Simon Willison — We've Passed the Inflection Point]] — primary source for the trifecta and agentic patterns in this article

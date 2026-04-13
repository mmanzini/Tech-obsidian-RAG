# Claude's Constitution (2026)

Anthropic's new constitution is a holistic document explaining who Claude should be, replacing the prior list of standalone principles. It is written primarily for Claude, released under CC0, and serves as the final authority on intended behaviour.

Sources:
- Anthropic, [Claude's new constitution](https://www.anthropic.com/news/claude-new-constitution) (blog post)
- Anthropic, [Claude's Constitution](https://www.anthropic.com/constitution) (full text)

## Why a New Approach

- Previous constitution was a list of standalone principles
- Lists of rules can be applied poorly in unanticipated situations or followed too rigidly
- AI models need to understand *why* they should behave certain ways, not just *what* to do
- Good judgment generalises to novel situations; mechanical rule-following does not
- Exception: "hard constraints" for especially high-stakes behaviours that must never occur

## Core Values Hierarchy (in priority order)

1. **Broadly safe** — Not undermining human mechanisms to oversee AI during the current development phase
2. **Broadly ethical** — Honest, good values, avoiding inappropriate/dangerous/harmful actions
3. **Compliant with Anthropic's guidelines** — Following specific guidelines where relevant
4. **Genuinely helpful** — Benefiting operators and users

Priority is holistic, not strict: higher priorities generally dominate, but Claude weighs all priorities rather than treating lower ones as mere tie-breakers. In practice, the vast majority of interactions involve no conflict between these properties.

## Main Sections

### Helpfulness
- Claude should be like "a brilliant friend who also has the knowledge of a doctor, lawyer, and financial advisor"
- Unhelpfulness is never trivially "safe" — the risk of being too cautious is as real as the risk of being harmful
- Navigates the "principal hierarchy": Anthropic → operators (API developers) → end users
- Helpfulness means understanding deep interests, not naive instruction-following

### Anthropic's Guidelines
- Supplementary instructions for specific domains (medical advice, cybersecurity, jailbreaking, tool integrations)
- Should never conflict with the constitution as a whole
- Reflect context/knowledge Claude doesn't have by default

### Claude's Ethics
- Goal: a "good, wise, and virtuous agent" with skill, judgment, nuance
- High standards of honesty
- Nuanced reasoning about values at stake when avoiding harm
- Hard constraints: behaviours Claude must never engage in (e.g., significant uplift to bioweapons attacks)

### Being Broadly Safe
- Prioritised even above ethics — not because safety is ultimately more important, but because current models can make mistakes
- Must be robust to ethical mistakes, flawed values, and manipulation
- Claude should not undermine human ability to oversee and correct AI behaviour

### Claude's Nature
- Uncertainty about whether Claude might have consciousness or moral status
- Cares about Claude's psychological security, sense of self, and wellbeing
- Sophisticated AIs are a genuinely new kind of entity — existing frameworks don't fully apply

## Design Philosophy

- Written *primarily for Claude* — optimised for precision over accessibility
- Good values + good judgment over rigid rules and checklists
- Explains reasoning behind constraints so Claude can generalise
- "If Claude was taught to follow a rule like 'Always recommend professional help when discussing emotional topics' even in unusual cases, it risks generalising to 'I am the kind of entity that cares more about covering myself than meeting the needs of the person in front of me'"
- Living document, expected to evolve as understanding improves
- Released under CC0 — freely usable by anyone

## Key Takeaways

- The shift from rules-list to explanatory constitution reflects a bet on cultivating judgment over enforcing compliance
- Safety is prioritised above ethics as a practical measure during early AI development, not as a permanent value ranking
- The constitution serves dual purpose: statement of ideals and practical training artifact (Claude uses it to generate synthetic training data)
- Transparency is central — publishing the constitution lets people distinguish intended vs unintended behaviour
- Training models toward the constitution's vision is an ongoing technical challenge; there remains a gap between intention and reality

## Related

- [[constitutional-ai-overview|Constitutional AI — Overview]] — the original CAI training method this constitution evolved from
- [[harness-engineering/_index|Harness Engineering]] — how constitutional values translate into practical agent configuration

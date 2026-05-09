# Asha Sharma — The product-as-organism

**Source:** [How 80,000 companies build with AI: Products as organisms and the death of org charts](https://www.youtube.com/watch?v=J9UWaltU-7Q)

Sharma's frame: AI-era products behave like organisms, not artifacts. They mutate between releases, the boundary between product and org blurs, and the loop — not the launch — is the unit of work. (source: https://www.youtube.com/watch?v=J9UWaltU-7Q)

## Summary

Asha Sharma (ex-Microsoft) argues that AI products behave like organisms — they learn continuously between ships, so planning in 8–12 week "seasons" with a shared evaluation harness replaces annual roadmaps. Her most important structural insight is that in large models, over 50% of observable quality now comes from post-training (fine-tuning, eval sets, data mix), making the PM's lever the data loop rather than the feature. The key takeaway is that the work chart equals the org chart: closed-loop squads that own the full data→model→eval→ship cycle beat traditional functional lanes.

## Product-as-organism
- Classic software was a frozen artifact shipped on cadence; AI products learn continuously between ships
- "Seasons" planning replaces annual roadmaps — 8–12 week arcs with a theme and a shared evaluation harness
- Work chart = org chart: how you build the product literally shapes the team topology
- See [[ai-product-development]] and [[ai-organization]]

## Post-training is where the product lives
- Sharma cites Nathan Lambert's rule-of-thumb: above ~30B parameters, >50% of observable quality comes from post-training, not pre-training
- The PM lever is no longer the feature — it's the fine-tune, the eval set, the data mix
- Dragon (Microsoft's medical scribe) moved from 30–60% usable drafts to 83% after ~600K expert annotations
- Post-training is compounding craft; it's where distinctive product behaviour is actually built

## From GUIs to code-native surfaces
- User-facing chat is training wheels; the mature surface is a code-generating agent that drafts workflows, not dialogue
- "Agentic society" framing: humans delegate to agents that negotiate with other agents; the UI is a queue of outcomes
- PMs must design handoffs, not screens — who owns the ambiguity when the agent is unsure?

## Organisational implications
- The work chart drives the org chart: if the loop is data → model → eval → ship, you need a squad that owns the whole loop
- Traditional PM/engineer/designer lanes break; Sharma favours small closed-loop pods with shared eval ownership
- See [[way-of-working]] on loop-not-lane operating models

## Satya's renewable optimism
- Sharma quotes Nadella: "optimism is renewable" — leadership at this pace requires manufacturing belief every morning
- Pair with disciplined reality: the organism can die between seasons if the eval line stops moving

## Key Takeaways
- AI products are organisms — plan in seasons, not roadmaps
- Post-training (data, fine-tune, eval) is where >50% of quality now comes from in large models
- The chat UI is transitional; the real surface is code-generating agents
- Work chart = org chart: closed-loop squads beat functional lanes
- Leaders must renew optimism daily while holding the eval line

## Related
- [[ai-product-development/_index|AI Product Development]]
- [[ai-organization/_index|AI & Organisation Design]]
- [[agent-workflows/_index|Agent Workflows]]
- [[lennys-podcast/_index|Lenny's Podcast]]

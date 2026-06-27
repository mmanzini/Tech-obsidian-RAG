---
type: synthesis
title: Small Teams Ship Faster
description: Small teams consistently outship larger ones on product work because coordination overhead grows super-linearly with headcount.
bucket: ai-engineering
topic: ai-organization
tags: [ai-org-design, ai-native-business, product-management]
source: https://example.com/small-teams-ship-faster
resource: https://example.com/small-teams-ship-faster
timestamp: 2026-04-29T21:54:50Z
status: active
related:
  - ai-engineering/ai-organization/from-hierarchy-to-intelligence.md
  - ai-engineering/ai-organization/openclaw-personal-agent-team.md
  - ai-engineering/ai-organization/anthropic-economic-index-learning-curves.md
  - ai-engineering/ai-organization/an-ai-glossary.md
---

# Small Teams Ship Faster

**Source:** [How small teams ship faster than large ones](https://example.com/small-teams-ship-faster)
**Author:** J. Doe
**Captured / Recorded:** 2026-04-22

---

## Summary
Small teams consistently outship larger ones on product work because coordination overhead grows super-linearly with headcount. A 2024 internal study at a large tech company found teams of 4–6 produced twice the merged-PR rate per engineer compared to teams of 12+, with the key driver being the number of coordination edges each engineer must maintain rather than individual capability.

## Coordination Edges and the Scaling Curve
The core argument is that coordination cost scales roughly as the square of team size — each new member adds edges to every existing member, not just to a manager (source: clipping-one.md). For a team of five, that's ten edges; for twelve, sixty-six. The diagram below illustrates how this curve steepens as headcount grows:

![[example-diagram.png]]

The 2024 internal study quantified the effect: teams of 4–6 produced a **2× merged-PR rate per engineer** relative to teams of 12+, measured across comparable product-work tracks at the same company (source: clipping-one.md).

## The Counter-Case: Genuinely Parallelisable Work
The argument has a known exception. When the work is genuinely parallelisable across independent surfaces — a localisation sweep, a bulk refactor across unrelated modules — large teams can win because the coordination edges carry little payload (source: clipping-one.md). The curve only dominates when everyone regularly touches the same code path, which is the norm for product work but not for certain infrastructure or content tasks.

## Implications for Team Design
- Keep product squads in the 4–6 range as the default; treat growth above 8 as a signal to split or create a platform seam.
- Distinguish work type before assigning headcount: parallelisable tasks are the one legitimate exception.
- Measure per-engineer output, not total output — a large team may deliver more in absolute terms while delivering less per head and accumulating more coordination debt.

## Key Takeaways
- Coordination edges grow super-linearly with team size, making small teams faster per engineer on product work (source: clipping-one.md).
- A 2024 internal study confirmed a 2× merged-PR-rate advantage for teams of 4–6 over teams of 12+ (source: clipping-one.md).
- The one counter-case is genuinely parallelisable work where coordination edges carry minimal payload (source: clipping-one.md).
- Team size is a design choice that should be made per work type, not per headcount pressure.

## Related
- [[from-hierarchy-to-intelligence]] — Block's vision for replacing coordination layers with AI world models; complementary argument for reducing human coordination overhead
- [[openclaw-personal-agent-team]] — a one-person team augmented by nine AI agents; an extreme point on the small-team curve
- [[anthropic-economic-index-learning-curves]] — usage and tenure data that speaks to per-engineer productivity trajectories
- [[an-ai-glossary|An AI Glossary]] — foundational AI terminology for the tooling that enables small teams to punch above their weight

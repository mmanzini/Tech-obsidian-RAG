# Against Premature Abstraction

**Source:** [Notes on the case against premature abstraction](https://example.com/premature-abstraction)
**Author:** A. Engineer
**Captured / Recorded:** 2026-04-23

---

## Summary
Premature abstraction is expensive not at the moment of writing but every subsequent time a reader must unwind it to understand what is actually happening. The article argues that DRY-at-all-costs is the primary failure mode in experienced codebases, drawing on Sandi Metz's principle that duplication is far cheaper than the wrong abstraction — while acknowledging the inverse failure (never abstracting genuinely repeated logic) also exists, though it is rarer.

## The Asymmetric Cost of a Bad Abstraction
The key insight is about when the cost is paid. Three similar lines of code impose a one-time cost on the engineer who eventually unifies them (source: clipping-two.md). A wrong abstraction imposes a cost on every future reader who must mentally unwind the indirection to reason about the concrete behaviour (source: clipping-two.md). The cumulative reader-cost quickly exceeds the writer-cost, especially in long-lived codebases with high contributor turnover.

Sandi Metz's formulation makes this precise: **"duplication is far cheaper than the wrong abstraction"** (source: clipping-two.md). This reframes the instinct to reach for an abstraction whenever repetition is spotted — the question should be whether the abstraction is *right*, not merely whether it removes duplication.

## DRY-at-All-Costs as the Primary Failure Mode
The article identifies DRY-at-all-costs — the compulsion to abstract at first sight of repetition — as the most common failure mode in production codebases (source: clipping-two.md). The mechanism:

1. Two similar code paths appear. An abstraction is created to unify them.
2. The paths diverge slightly over time. Parameters are added to the abstraction to handle both cases.
3. The abstraction becomes load-bearing and hard to unpick; each new case adds another branch.
4. Future engineers treat the abstraction as a constraint rather than a tool, bending new work to fit it.

The result is an abstraction that encodes accidental rather than essential similarity — paying the unwinding cost on every read from that point forward.

## The Inverse Failure
The inverse — refusing to abstract genuinely repeated logic — also exists but is rarer in codebases written by experienced engineers (source: clipping-two.md). When the duplication is genuine (same algorithm, same invariants, same change cadence), a well-placed abstraction reduces both maintenance surface and the probability of divergent bugs. The argument is not against abstraction; it is against premature abstraction, where "premature" means before the pattern is well-understood.

## A Practical Heuristic
Wait for the third occurrence and enough context to identify the stable shared invariant before abstracting. Two occurrences is often too early to know whether the similarity is essential or coincidental.

## Key Takeaways
- A wrong abstraction's cost is paid on every future read, not just at creation time (source: clipping-two.md).
- Sandi Metz's principle: duplication is far cheaper than the wrong abstraction (source: clipping-two.md).
- DRY-at-all-costs is the dominant failure mode; it produces abstractions encoding accidental similarity (source: clipping-two.md).
- The inverse failure (under-abstracting) exists but is less common in experienced codebases (source: clipping-two.md).
- Heuristic: wait for the third occurrence and a clear stable invariant before extracting.

## Related
- [[heist-framework]] — the Heist operating model; team rituals that shape when and how engineers make shared abstractions
- [[agile-and-ai-team-structures]] — principles for AI-native teams where code churn is high and premature abstraction risk is elevated

---
type: synthesis
title: The agentic knowledge engine — end-to-end overview
description: The canonical narrative of the engine behind Atlas — a wiki an agent curates, a frozen harness plus an iterable program, tiered recall, a learning loop, two kinds of memory, and OKF portability — distilled from Max's essay "The knowledge base that curates itself".
bundle: ai-engineering
topic: knowledge-engineering
tags:
- knowledge-engineering
- llm-wiki
- agent-memory
- autoresearch
- okf
sources:
- id: 004-the-agentic-knowledge-engine
  resource: Resources/Projects/articles-and-essays/004-the-agentic-knowledge-engine/004-the-agentic-knowledge-engine.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-07-05T09:30:00Z'
status: stable
related:
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
- ai-engineering/knowledge-engineering/memory-three-jobs-and-atlas-tiers.md
- ai-engineering/knowledge-engineering/autoresearch-methodology.md
- ai-engineering/knowledge-engineering/karpathy-llm-knowledge-bases.md
- ai-engineering/knowledge-engineering/open-knowledge-format.md
---

# The agentic knowledge engine — end-to-end overview

**Source:** [004-the-agentic-knowledge-engine.md](../../../Resources/Projects/articles-and-essays/004-the-agentic-knowledge-engine/004-the-agentic-knowledge-engine.md) — Max's essay "The knowledge base that curates itself" (board T039)
**Author:** Max (self-authored essay; blog voice)

---

## Summary

The essay is the canonical end-to-end narrative of the engine Atlas is built on: treat the knowledge base as a wiki an agent curates, give it the discipline a good wiki has, and make recall something you can read and check rather than something you have to trust. The pieces are specified in detail across this topic's other articles; this overview ties them into one argument and records the framing Max uses publicly (source: 004-the-agentic-knowledge-engine.md).

## The shape and the two-file split

At the centre is a plain folder of markdown. Raw material goes into one zone; the agent digests it into a second zone — a wiki of small cited articles grouped into bundles and topics. The articles *are* the store, readable and auditable, so retrieval can be a walk over indexes and an audit can count broken links and uncited claims. Two root files govern everything, and the split is the single design decision Max would keep above all others: a **frozen harness** holds the contracts that never change mid-run (what an article must contain, how a citation is written, the hard rules), and an **iterable program** holds the behaviour (verbs, budgets, routing heuristics) meant to be edited over time. Freezing the contracts makes "fix a failure by quietly redefining success" impossible — an improvement is measurable against a yardstick that means the same thing run after run. This is borrowed from the autoresearch pattern (source: 004-the-agentic-knowledge-engine.md). See [[autoresearch-methodology]] and [[llm-vault-structure-spec]].

## Store, inject, recall

The engine is three flows. **Storing** is the `consolidate` verb: scan input, route each source to a bundle by content, pick or create a topic, write a cited article; unroutable sources go to quarantine, not the nearest bundle. **Injecting** is the flat, cheap session start: frozen files plus two thin routers plus a ≤1,500-token snapshot — a fixed cost that does not grow with the store. **Recalling** is tiered, cheap layer first: tier 0 the snapshot already in context (zero reads for recent-decision / stable-preference questions); tier 1 the index walk (bundle → topic → article bodies last), carrying the citation discipline; tier 2 a local hybrid search that returns *pointers, never answers* — the agent opens, reads and cites the pointed articles. If all three miss, the agent says so and does not invent (source: 004-the-agentic-knowledge-engine.md). The tier design is detailed in [[memory-three-jobs-and-atlas-tiers]].

## The verbs and the loop

Five verbs and no more: `query` reads, `consolidate` writes, `refine` audits without changing anything, `evaluate` measures retrieval against fixed questions, `reflect` maintains the agent's record of its own experience. The write path is a *loop*, not a step: the agent recalls before consolidating, consolidates, then automatically audits its own work (reducing structural health to a single comparable number, **drift**), then reflects (merging recurring lessons, regenerating the snapshot), rebuilds the search index, and commits. The learning lands in the agent's episodes and distilled reflections — never in the frozen contracts. The engine sharpens how it works without being able to rewrite the definition of what correct means (source: 004-the-agentic-knowledge-engine.md).

## Two memories, kept apart

The engine keeps two kinds of memory and refuses to mix them: **semantic** (the wiki of facts, each in a cited article) and **episodic** (the time-stamped record of what the agent did and what happened). Mixing them poisons retrieval both ways. Episodes come in three kinds — operational (verb runs), life (daily entries), signals (captured decisions/preferences) — and run the same four beats: recall, act, capture, reflect. **Distillation** keeps recall bounded: once a lesson is supported by ≥2 episodes and old enough, the episodes behind it are stamped distilled and leave the active recall surface (the newest confirming one stays as exemplar), so memory compresses as it ages and recall cost never grows with the store. This is what closes the loop the AI-memory discourse usually leaves open: an agent with a large model and no episodic memory is brilliant and amnesiac (source: 004-the-agentic-knowledge-engine.md).

## Against the self-curated memory file, and OKF portability

The essay positions the engine next to the popular self-curated-memory-file approach (Karpathy's: memory as a plain text file the model maintains and reads back). It agrees with the core instinct — memory should be readable text the model curates, not an opaque embedding store — but goes further in two places: it splits semantic from episodic (different lifespans, different retrieval needs), and it keeps the arrow of time (episodes record that something happened on a day, in a situation, with an outcome). The snapshot plays the role Karpathy's file plays, but it is *generated* from the episodes and lessons beneath it rather than being the primary store (source: 004-the-agentic-knowledge-engine.md). See [[karpathy-llm-knowledge-bases]]. Finally, portability is an *alignment, not a rebuild*: aligning to OKF added a frontmatter block and a cross-reference list per article and a derived per-bundle changelog, nothing about the shape changed, the export step disappeared because each bundle is already a conformant bundle, and conformance went *into the loop* (the same audit checks every article carries its type and every cross-reference resolves) (source: 004-the-agentic-knowledge-engine.md). See [[open-knowledge-format]].

## What it solves

Stripped down, the engine answers five failures: the **black box** (retrieval is a readable walk; every answer cites its article), **context bloat** (flat startup cost by construction), **silent rot** (the audit runs on every write and reduces health to one comparable number), **the agent that forgets** (experience is stored, recalled and distilled — the autoresearch idea turned on the agent's own behaviour), and **lock-in** (articles conform to an open format; each bundle is already a portable bundle). The wager is that discipline beats cleverness: a knowledge base you can read, check and hand to another tool will outlast one you have to trust (source: 004-the-agentic-knowledge-engine.md).

## The public boilerplate repo

The engine ships as a public boilerplate: a clone-and-go vault scaffold kept as a vault copy under `Resources/Projects/agentic-knowledge-engine/` and synced to GitHub. It contains the operating contract (`CLAUDE.md`), the frozen harness (`schema.md`), placeholder `domain-a`/`domain-b` bundles with sample articles, the `_search` tooling (`build_index.py`, `search.py`, `okf_tools.py`), the `_episodes/` episodic-memory zone with seed examples, an `_eval/questions.md` benchmark stub, the auto-capture skill, and a getting-started guide (replace the placeholder bundles, fill in the prescriptive personal sources, drop material into `Resources/`, run `consolidate`) (source: Resources/Projects/agentic-knowledge-engine/README.md). The boilerplate's content stays in sync via the vault copy; its GitHub `README.md` is repo-only (Unison-ignored) and maintained separately (source: session-2026-06-30-0824.md).

## Key Takeaways

- The essay is the engine's canonical narrative; the mechanisms are specified in this topic's other articles, which this overview ties together.
- The engine is public as a clone-and-go boilerplate repo, mirrored from `Resources/Projects/agentic-knowledge-engine/`; its GitHub README is repo-only and synced by hand.
- The load-bearing design decision is the frozen-harness / iterable-program split — it makes improvement measurable against a fixed yardstick.
- Recall is tiered (snapshot → index walk → hybrid-search pointers) and the write path is an auditing, self-distilling loop; learning goes only to episodes, never to the contracts.
- Two memories kept apart (semantic wiki, episodic record), and OKF portability achieved additively with conformance enforced in the audit loop.

## Related

- [[llm-vault-structure-spec]] · [LLM Vault Structure Spec](llm-vault-structure-spec.md) — the two-zone, two-layer architecture the essay describes
- [[memory-three-jobs-and-atlas-tiers]] · [Memory's three jobs and Atlas's tiers](memory-three-jobs-and-atlas-tiers.md) — the store/inject/recall and tier model
- [[autoresearch-methodology]] · [Autoresearch methodology](autoresearch-methodology.md) — the frozen-harness-under-an-experimental-loop pattern the engine borrows
- [[karpathy-llm-knowledge-bases]] · [Karpathy's LLM Knowledge Bases](karpathy-llm-knowledge-bases.md) — the self-curated-memory-file approach the essay extends
- [[open-knowledge-format]] · [Open Knowledge Format (OKF)](open-knowledge-format.md) — the portability layer the engine aligns to in-place

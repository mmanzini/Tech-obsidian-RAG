---
type: synthesis
title: Autoresearch — Autonomous AI Research Methodology
description: 'Autoresearch is an Andrej Karpathy project that turns a single-GPU LLM training setup into an autonomous research organism: an AI agent iterates on `train.py` overnight — modifying architecture, hyperparameters, and optimizer, running a fixed 5-minute training budget, measuring `val_bpb`, and keeping or discarding each experiment.'
bundle: ai-engineering
topic: knowledge-engineering
tags:
- knowledge-management
- harness-engineering
- agent-workflows
- evals
- long-running-agents
sources:
- id: readme
  resource: ../../../Resources/documents/frameworks/autoresearch-master/README.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-17T08:21:13Z'
status: stable
related:
- ai-engineering/harness-engineering/nlh-meta-harness-harness-science.md
- ai-engineering/harness-engineering/skill-issue-harness-engineering.md
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
- ai-engineering/knowledge-engineering/karpathy-llm-knowledge-bases.md
---

# Autoresearch — Autonomous AI Research Methodology

**Source:** [autoresearch README](../../../Resources/documents/frameworks/autoresearch-master/README.md), [program.md](../../../Resources/documents/frameworks/autoresearch-master/program.md)
**Author:** Andrej Karpathy

---

## Summary

Autoresearch is an Andrej Karpathy project that turns a single-GPU LLM training setup into an autonomous research organism: an AI agent iterates on `train.py` overnight — modifying architecture, hyperparameters, and optimizer, running a fixed 5-minute training budget, measuring `val_bpb`, and keeping or discarding each experiment. The human steers the research through `program.md`, a lightweight Markdown harness that functions as the "research org code." The vault's own `CLAUDE.md`-plus-`program.md` pattern is directly derived from this idea.

## Core Architecture

The repo has three files that matter (source: autoresearch README):

- **`prepare.py`** — fixed constants, one-time data prep (downloads training data, trains a BPE tokeniser), and runtime utilities (dataloader, evaluation). The agent never touches this.
- **`train.py`** — the single file the agent edits. Contains the full GPT model, optimizer (Muon + AdamW), and training loop. Everything is fair game: architecture, hyperparameters, optimizer, batch size.
- **`program.md`** — baseline instructions for one agent. The human iterates on this to shape the research direction; the agent iterates on `train.py` to improve the metric.

The metric is **val_bpb** (validation bits per byte) — lower is better, vocab-size-independent, so architectural changes are fairly compared across runs (source: autoresearch README).

## The Fixed-Budget Design

Training always runs for exactly **5 minutes wall clock** (excluding startup/compilation), regardless of what the agent changes (source: autoresearch README). Two benefits:

1. All experiments are directly comparable regardless of model size, batch size, or architecture.
2. The search finds the most optimal model for the specific platform in that time budget.

Downside: results are not portable across compute platforms. Each H100 result is specific to an H100.

## The Experiment Loop

The agent runs the following loop autonomously until manually stopped (source: program.md):

1. Read the current git state.
2. Modify `train.py` with an experimental idea.
3. `git commit`.
4. Run: `uv run train.py > run.log 2>&1`.
5. Extract results: `grep "^val_bpb:\|^peak_vram_mb:" run.log`.
6. If empty (crash), read the stack trace and attempt a fix. Give up after a few attempts.
7. Log to `results.tsv` (commit hash, val_bpb, memory_gb, status, description).
8. If val_bpb improved, keep the commit (advance). If equal or worse, `git reset` to discard.

The agent **never pauses to ask** if it should continue. The human may be asleep. The loop runs until manually interrupted (source: program.md).

## The Simplicity Criterion

When evaluating whether to keep a change (source: program.md):
- A small improvement that adds ugly complexity is not worth it.
- Removing something and getting equal or better results is a great outcome (simplification win).
- A 0.001 val_bpb improvement from deleting code? Keep. A 0.001 improvement from adding 20 lines of hacky code? Probably not.

This simplicity criterion is foundational to the Atlas `refine` verb, which applies the same logic: *equal information content + simpler structure = win*.

## `program.md` as a Harness

The `program.md` file is deliberately minimal — described as "essentially a super lightweight skill" (source: autoresearch README). The human edits it over time to improve the research direction. This is the same pattern the Atlas vault uses: `CLAUDE.md` is a `program.md`-shaped file that the human iterates to improve agent behaviour, while keeping the schema frozen (the `schema.md` analogue of `prepare.py`).

Key parallel:
- `program.md` = `CLAUDE.md` (iterable agent instructions, human-maintained)
- `prepare.py` = `schema.md` (fixed harness, not touched by the agent)
- `train.py` = the wiki articles (the thing the agent modifies)

## Setup

Requirements: single NVIDIA GPU (tested on H100), Python 3.10+, [uv](https://docs.astral.sh/uv/).

```bash
uv sync                  # install dependencies
uv run prepare.py        # one-time data prep (~2 min)
uv run train.py          # single manual run to verify setup
```

Then point Claude/Codex at the repo and prompt it toward `program.md` to begin the autonomous loop (source: autoresearch README).

## Key Takeaways

- The fixed 5-minute budget makes experiments comparable; the human wakes up to ~100 results.
- `train.py` is the only edit target; `prepare.py` is the immutable harness.
- `program.md` is the human's steering wheel; editing it is how you improve the research direction over time.
- The simplicity criterion — prefer simpler solutions at equal performance — is a first-class research value.
- The pattern generalises directly to any autonomous agent task, including this vault's consolidate/refine loop.

## Related

- [[nlh-meta-harness-harness-science|NLH, Meta Harness, and the Science of Harness Engineering]] — the broader context of how `program.md`-style harnesses emerged
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — practical guide to the CLAUDE.md harness this methodology inspired
- [[llm-vault-structure-spec|LLM Vault Structure Spec]] — the vault design document that extends the autoresearch pattern to a RAG wiki
- [[karpathy-llm-knowledge-bases|Karpathy's LLM Knowledge Bases]] — the companion thread describing the wiki-compilation workflow that gave rise to the Atlas vault pattern; the direct precursor to this methodology applied to knowledge rather than ML training

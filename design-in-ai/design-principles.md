---
type: synthesis
title: Design Principles for AI-Assisted Development
description: Ten cross-cutting principles that emerge from the convergence of SDD, Kiro, RPI, Design.md and industry research.
bucket: ai-engineering
topic: design-in-ai
tags: []
source: ../../../Resources/documents/frameworks/Design in AI/Core Concepts/Principles.md
resource:
timestamp: 2026-04-30T06:41:32Z
status: active
related:
  - ai-engineering/design-in-ai/what-is-design-md.md
  - ai-engineering/design-in-ai/design-first-vs-requirements-first.md
  - ai-engineering/design-in-ai/integrating-design-with-specs.md
  - ai-engineering/design-in-ai/writing-effective-design-docs.md
  - ai-engineering/design-in-ai/getting-started-with-stitch.md
---

# Design Principles for AI-Assisted Development

**Source:** [Principles.md](../../../Resources/documents/frameworks/Design in AI/Core Concepts/Principles.md)

---

## Summary

Ten cross-cutting principles that emerge from the convergence of SDD, Kiro, RPI, Design.md and industry research. They explain *why* design documents are non-negotiable in AI-assisted development — covering intent durability, spec-as-truth, constrained generation, pre-implementation review, tool agnosticism, progressive disclosure, boundary declaration, living artefacts, brownfield anchoring, and documentation as infrastructure.

## Core Principles

### 1. Explicit Intent Survives Context Compaction

Chat history is ephemeral. When a conversation ends, an agent is swapped, or a context window fills, implicit intent vanishes. Design documents are durable artefacts that remain when everything else is discarded. A fresh AI session can read them and resume where the last one left off (source: Principles.md). This is the fundamental argument: if intent only lives in chat, it dies with the session.

### 2. The Spec Is the Source of Truth, Not the Code

Code is downstream of the specification. If code and spec disagree, the spec wins — and gets updated if it was wrong. This inverts the traditional "code is truth" assumption. In an AI workflow where code can be regenerated from a spec, the spec is the asset you maintain (source: Principles.md).

### 3. Constrained AI Produces Better Output Than Unconstrained AI

Design.md embeds the principle that explicit constraints improve quality. Providing specific hex values, font families, and spacing tokens is more reliable than asking a model to "be consistent." Constraints reduce hallucination and visual drift (source: Principles.md).

### 4. Design Is Reviewable Before Implementation

Design documents enable review without code. Architecture, data models, interfaces, and trade-offs can be examined, challenged, and improved before a line of code is written. This prevents costly mid-implementation pivots (source: Principles.md).

### 5. Tool Agnosticism Is Non-Negotiable

A spec written for Kiro should be runnable by Claude Code, Cursor, or a human. The standard prescribes artefacts and phases, not which IDE, model, or framework you use. Design.md is portable markdown precisely because portability is the point (source: Principles.md).

### 6. Progressive Disclosure Reduces Cognitive Load

AI agents perform better when they can read interfaces and decisions first, then drill into implementation details on demand. Design documents should frontload what matters — public types, API surfaces, visual tokens — and permit deeper exploration as needed. This is the greybox module pattern applied to documentation (source: Principles.md).

### 7. Boundaries Are Part of Design Intent

Every design document should declare what the AI agent may always do, must ask about, and must never do. These boundaries are not guardrails bolted on after the fact — they are design decisions that define the trust envelope for the work (source: Principles.md).

### 8. Design Documents Are Living Artefacts

A design document is not a one-time deliverable that gets written and forgotten. It is maintained alongside the code, updated when reality disagrees with the plan, and archived with the merged code for traceability. Six months later, "Why Redis instead of SQS?" should be traceable from code through task through requirement through architecture commit with rationale (source: Principles.md).

### 9. Brownfield Design Must Anchor to Reality

Design documents for existing systems must describe the world as it *is*, not as it should be. The research phase (what exists, what works, what is broken) precedes the design phase (what to change, how to change it). Generic greenfield templates applied to brownfield projects are a frequent failure mode (source: Principles.md).

### 10. Documentation Is Infrastructure

Like code, documentation requires continuous maintenance. Stale documentation degrades agent performance. Teams that treat design documents as living, tested infrastructure — reviewed in PRs, validated by CI — report better outcomes than teams that treat them as optional write-once artefacts (source: Principles.md).

## Key Takeaways

- Intent must outlive the session: write it down in a durable artefact.
- The spec is the asset; code is downstream and regenerable.
- Explicit constraints produce better AI output than vague direction.
- Review the design before writing code, not after.
- Keep docs portable (tool-agnostic markdown); treat them as living infrastructure.

## Related

- [[what-is-design-md|What is Design.md]] — the primary artefact these principles govern
- [[design-first-vs-requirements-first|Design-First vs Requirements-First]] — entry points into the spec workflow
- [[integrating-design-with-specs|Integrating Design with Specs]] — how Design.md fits alongside other spec files
- [[writing-effective-design-docs|Writing Effective Design Docs]] — practical authoring guidance grounded in these principles
- [[getting-started-with-stitch|Getting Started with Stitch]] — Google's AI-first design tool that operationalises these principles

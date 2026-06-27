---
type: synthesis
title: MDD — The Authoring Workflow (Adoption, Modelling, Transformations)
description: "The practitioner workflow for doing model-driven development, distilled from the Standard's eight Guides: how to adopt MDD incrementally, how to get from domain knowledge to a precise model, how to keep the PIM portable, how to generate the PSM and code, and how to build transformations that earn trust."
bucket: ai-engineering
topic: model-driven-development
tags: [model-driven-development, spec-driven-development, agent-workflows]
source: ../../../Resources/documents/frameworks/Model-driven-development/Guides/
resource:
timestamp: 2026-06-02T18:26:22Z
status: active
related:
  - ai-engineering/model-driven-development/mdd-overview.md
  - ai-engineering/model-driven-development/mdd-worked-example-order-management.md
  - ai-engineering/model-driven-development/the-model-was-always-the-point.md
---

# MDD — The Authoring Workflow (Adoption, Modelling, Transformations)

**Source:** [Model-Driven Development Standard — Guides](../../../Resources/documents/frameworks/Model-driven-development/Guides/) (8 practitioner guides)

---

## Summary

The practitioner workflow for doing model-driven development, distilled from the Standard's eight Guides: how to adopt MDD incrementally, how to get from domain knowledge to a precise model, how to keep the PIM portable, how to generate the PSM and code, and how to build transformations that earn trust. It complements the conceptual [[mdd-overview|MDD overview]] and the [[mdd-classic-vs-modern|classic-vs-modern]] spectrum with the *how*, and is illustrated end-to-end by the [[mdd-worked-example-order-management|order-management worked example]].

## Adopt at the lowest rung that wins

Do not default to full MDA. Pick the adoption rung that fits the project and write the choice into a conventions document (source: Guides/Greenfield Modelling.md):

- **Rung 1–2** — generate one or two painful artifacts (schema, DTOs, API clients) from a hand-written model. ~30 minutes to a first win; skip CIM, PIM, DSL and round-tripping entirely (source: Guides/Getting Started.md).
- **Rung 3** — introduce a real platform-independent model (PIM) with a PIM→PSM→code pipeline when portability or longevity matters.
- **Rung 4** — a custom DSL when one domain dominates and experts should read/write models.
- **Rung 5** — full MDA (UML/MOF/QVT) for long-lived, regulated, multi-platform systems.

**Greenfield:** decide the rung first, set up the conventions document before modelling, then prove the whole pipeline on a *thin vertical slice* (one entity → lifecycle → operation → generated running code) before adding breadth (source: Guides/Greenfield Modelling.md). **Brownfield:** adopt bottom-up where the pain is greatest — model one repetitive slice, generate *alongside* and diff against the hand-written version until they match, then switch and delete the original. Reverse-engineering produces drafts to clean up, never authoritative models (source: Guides/Brownfield Adoption.md).

## From domain to model (CIM → PIM)

Harvest the experts' actual vocabulary — nouns → entities/attributes, verbs → operations/processes, rules → constraints — without renaming into engineering jargon (source: Guides/From Domain to Model.md). For every statement, sort it into a layer: true about the world regardless of software → **CIM**; about what the software must do but not how → **PIM**; a platform decision → defer to the **PSM** (source: Guides/From Domain to Model.md). Draft the CIM in business language and have a domain expert read it back; refine into a PIM by adding computational precision.

## Build a PIM precise enough to generate from, disciplined enough to stay portable

The PIM's quality caps everything generated below it. Include entities (abstract-typed attributes, operation signatures with pre/postconditions), relationships with multiplicity, state machines for entities with a lifecycle, and invariants. **Keep platform detail out** — no storage/tables, no wire/API formats, no framework names, no concrete types (`Money`, not `DECIMAL(10,2)`). The test for every statement: *could this survive a platform change?* If not, it belongs in the PSM (source: Guides/Building a PIM.md). The honest completeness check: *can the PIM→PSM transformation run without a human filling gaps?* Whatever the generator can't infer must be in the model, explicitly (source: Guides/Building a PIM.md).

## Generate the PSM — don't hand-write it

The ideal is not to write a PSM by hand but to write a **PIM→PSM transformation** that encodes the platform mapping and run it; the PSM becomes a generated artifact, and the same PIM re-targets to a second platform via a second transformation (source: Guides/Designing a PSM.md). The PSM is the home for every deferred decision — language/runtime, persistence + entity→table mapping, API style + operation→endpoint mapping, frameworks, cross-cutting concerns. Map each abstract type consistently (`Money` → amount + currency), give every PIM invariant an enforcement home (DB constraint, validation annotation, or service check — an invariant with no realisation silently won't be enforced), and decide the hand-written boundary *before* generating (source: Guides/Designing a PSM.md).

## Transformations are the leverage — treat them like software

Write one transformation well and it runs against every model forever; a bug in it silently corrupts every artifact it produces (source: Guides/Writing Transformations.md). Build them from a concrete *(input model → desired output)* sample pair kept as the first test, add rules construct by construct (entity → attributes → relationships → operations → state machines → constraints), diffing against the expected output each step. Pick technology by need: ATL/QVTo for model→model, Acceleo/Xpand (MTL) for model→text, OCL for navigation. Version, test, and review transformations as shared libraries; preserve traceability (source element → target element) for debugging and incremental regeneration (source: Guides/Writing Transformations.md).

## When the domain warrants its own language (DSL)

A DSL beats stretching UML when one domain dominates, experts should read/write models, and you generate repeatedly. Its three parts: **abstract syntax** (the metamodel, kept small), **concrete syntax** (textual via Xtext, graphical via Sirius, or projectional via MPS), and **semantics** (by generation, by interpretation, or both). Constrain well-formedness with rules (often OCL) so editor and generator reject invalid models early; start minimal and grow with real use. A DSL trades a maintenance burden for control, versus a low-code platform's speed-but-lock-in (source: Guides/Defining a DSL.md).

## The hand-written boundary, always explicit

Across every guide the invariant is the same: the generator owns its output files completely (`generated/` is never hand-edited), and human logic attaches through one agreed mechanism — protected regions, generation-gap subclasses, or separate modules. Set this rule in week one; habits persist (source: Guides/Getting Started.md; source: Guides/Designing a PSM.md).

## Key Takeaways

- Adopt at the lowest rung that delivers a real win; prove the pipeline on a thin vertical slice before breadth
- Sort every statement into CIM / PIM / PSM at authoring time — that discipline is what stops platform detail leaking upward
- The PIM caps downstream quality: abstract types, precise behaviour, explicit invariants, and the "can a generator run without filling gaps?" completeness test
- Prefer *generating* the PSM via a PIM→PSM transformation — that is what makes one model re-targetable
- Transformations are software: sample-pair tests, versioning, review, traceability — a transformation bug corrupts every artifact
- The hand-written boundary (generation gap / protected regions) is decided before generating and is never crossed by manual edits

## Related

- [[mdd-overview|MDD — Principles, Abstraction Layers, and Workflow]] — the concepts this workflow operationalises
- [[mdd-worked-example-order-management|MDD — Worked Example: order-management]] — the workflow shown end-to-end
- [[the-model-was-always-the-point|The Model Was Always the Point]] — where AI fits (LLM for authoring, deterministic generator for execution)
- [[../spec-driven-development/index|Spec-Driven Development]] — sibling methodology; AI as executor, not decision-maker

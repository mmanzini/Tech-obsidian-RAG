---
type: synthesis
title: MDD — Classic MDA vs Modern Practice
description: '"Model-driven development" names a spectrum, not a single method.'
bundle: ai-engineering
topic: model-driven-development
tags:
- model-driven-development
- spec-driven-development
- agent-architecture
sources:
- id: classic-vs-modern-mdd
  resource: ../../../Resources/documents/frameworks/Model-driven-development/Standard/Classic vs Modern MDD.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-06-02T13:33:31Z'
status: stable
related:
- ai-engineering/model-driven-development/mdd-overview.md
- ai-engineering/spec-driven-development/sdd-overview.md
- ai-engineering/model-driven-development/the-model-was-always-the-point.md
---

# MDD — Classic MDA vs Modern Practice

**Source:** [Standard/Classic vs Modern MDD.md](../../../Resources/documents/frameworks/Model-driven-development/Standard/Classic vs Modern MDD.md), [Standard/Tooling.md](../../../Resources/documents/frameworks/Model-driven-development/Standard/Tooling.md)

---

## Summary

"Model-driven development" names a spectrum, not a single method. At one end sits the classic OMG Model-Driven Architecture (MDA) — formal, standards-heavy, UML-centric, launched in 2001. At the other sits modern model-driven practice: domain-specific languages, low-code platforms, executable models, and increasingly AI/LLM assistance. Both ends share the same core insight — drive generation from models — but relax the classic commitments in very different ways.

## The Family of Acronyms

| Acronym | Name | Scope | Pins down |
|---|---|---|---|
| **MBE** | Model-Based Engineering | Broadest | Models are *used*, but need not drive the process |
| **MDE** | Model-Driven Engineering | Broad | Models *drive* the whole engineering lifecycle, code and beyond |
| **MDD** | Model-Driven Development | Software dev | Models drive the building of running software |
| **MDA** | Model-Driven Architecture | Specific | OMG's standardised flavour of MDD |

MDE is the broadest idea; MDD is its application to building software; MDA is one specific, standardised way to do MDD proposed by the OMG (source: Standard/Classic vs Modern MDD.md).

## Classic: OMG Model-Driven Architecture

MDA was launched by the **Object Management Group (OMG) in 2001**. Its defining commitments (source: Standard/Classic vs Modern MDD.md):

- **A fixed layer vocabulary:** CIM → PIM → PSM → code (see [[mdd-overview]])
- **UML as the modelling language**, governed by the **MOF** meta-metamodel, with **XMI** for interchange and **OCL** for constraints
- **Standardised transformations:** **QVT** for model-to-model (M2M) and **MOFM2T/MTL** for model-to-text (M2T)
- **Platform independence as the headline benefit:** one PIM, many PSMs, re-targetable as platforms change

**Strengths:** rigour and portability — a precise PIM expressed in a standard language, with standardised transformations, is vendor-neutral and survives platform change (source: Standard/Classic vs Modern MDD.md).

**Weaknesses:** weight and friction — heavy UML CASE tooling, a steep learning curve, round-tripping that rarely worked cleanly, and the difficulty of bending general-purpose UML to a specialised domain. MDA earned a reputation in the 2000s for promising more automation than most teams could realise (source: Standard/Classic vs Modern MDD.md).

## Modern: DSLs, Low-Code, Executable Models, AI

Modern model-driven practice keeps the core insight — drive generation from models — but relaxes nearly every classic commitment (source: Standard/Classic vs Modern MDD.md).

### Domain-Specific Languages (DSLs)

Rather than stretch UML, teams define a small language tailored to one domain. A DSL rejects nonsensical constructs by design, reads naturally to domain experts, and produces cleaner generators. Tools like **Xtext** (textual DSLs on EMF), **EMF/Ecore** (metamodelling), and **JetBrains MPS** (projectional editing) made DSL creation practical (source: Standard/Classic vs Modern MDD.md).

Trade-off versus UML: freedom and fit, at the cost of building and maintaining the language yourself.

### Low-Code / No-Code Platforms

Commercial platforms (Mendix, OutSystems, Microsoft Power Platform) are model-driven under the hood: users assemble applications from visual models and the platform generates and runs them (source: Standard/Classic vs Modern MDD.md).

Distinction from open MDD: low-code tools use **fixed, proprietary languages** — users cannot define their own metamodel. Low-code looks like a successor to 4GLs: trades freedom for speed and a managed runtime. Domain-specific modelling retains the freedom to define the language, scales better to complex domains, and integrates more cleanly with hand-written legacy code (source: Standard/Classic vs Modern MDD.md).

### Executable Models

Classic MDA often treated models as blueprints to generate *from*. Modern executable modelling (e.g. **fUML**, **Executable UML/xtUML**, and DSLs with interpreters) treats the model as something you can *run or simulate directly*, with code generation as one deployment option rather than the only path. This shortens the feedback loop: validate behaviour at the model level before committing to a platform (source: Standard/Classic vs Modern MDD.md).

### AI / LLM Assistance

The newest shift (2024–2025) is using large language models inside the modelling process rather than replacing it. Active research and tooling directions include (source: Standard/Classic vs Modern MDD.md):

- Generating draft domain models from natural-language descriptions
- Producing instance models (e.g. XMI conforming to an Ecore metamodel) from a specification
- Recommending model completions
- Classifying or searching model repositories

The pattern echoes Spec-Driven Development with AI: **the human owns the model and its metamodel; the LLM is an executor that drafts and suggests, and its output is reviewed against the metamodel before it is trusted** (source: Standard/Classic vs Modern MDD.md).

## Side-by-Side Comparison

| Dimension | Classic MDA | Modern model-driven |
|---|---|---|
| Era | OMG, from 2001 | 2010s–2020s, ongoing |
| Modelling language | UML (general-purpose) | DSLs, executable models, proprietary low-code |
| Governing standard | MOF / UML / QVT / OCL / XMI | Often EMF/Ecore; frequently no formal standard |
| Layers | Strict CIM / PIM / PSM | Often collapsed (model → code in one step) |
| Headline benefit | Portability across platforms | Productivity and fit to the domain |
| Typical tooling | Heavy UML CASE tools | Xtext, EMF, MPS, Mendix, OutSystems |
| Human's job | Author UML, define transformations | Define a DSL or assemble in a platform; review AI drafts |
| Main risk | Weight, friction, over-promise | Lock-in (low-code), DSL maintenance burden |

## When to Choose Which

- **Classic MDA discipline** — when portability and standards-compliance are the point: long-lived systems, regulated domains, or where multiple platforms must be targeted from one model.
- **DSL** — when one domain dominates, you want experts to read (or write) the models, and you can afford to build and maintain the language.
- **Low-code** — when speed and a managed runtime matter more than freedom, and you accept the platform's language and lock-in.
- **Executable models** — when early behavioural validation matters before committing to a platform.
- **AI assistance** — add to any of the above as a drafting and review aid, not as the owner of the model.

The common thread, classic or modern, is [[mdd-overview|MDD's principles]]: a model — not the code — is the artefact that drives the build, and a transformation — not a person retyping — carries it down to running software (source: Standard/Classic vs Modern MDD.md).

## Key Takeaways

- Classic MDA is rigorous and portable but heavy; modern MDD trades standards-compliance for fit, speed, and DSL expressiveness.
- Low-code platforms are model-driven under the hood but use fixed, proprietary languages — model-driven convenience at the cost of control.
- Executable models collapse the generate-then-test loop: run the model directly before committing to a platform.
- AI/LLM assistance is a drafting tool inside a human-owned modelling process, not a replacement for the metamodel discipline.
- The right choice depends on whether you need portability (MDA), domain fit (DSL), speed (low-code), early behavioural feedback (executable models), or all of the above gradually (incremental adoption).

## Related

- [[mdd-overview]] — core MDD concepts: principles, abstraction layers, metamodel hierarchy, workflow
- [[../spec-driven-development/sdd-overview]] — AI-era sibling: spec as the durable artefact, agents as implementers
- [[the-model-was-always-the-point|The Model Was Always the Point]] — Max's opinion essay building on this classic-vs-modern spectrum: the asymmetry-of-trust argument for where AI belongs in MDD

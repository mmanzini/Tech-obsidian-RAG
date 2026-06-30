---
type: synthesis
title: Claude Cowork Context Questionnaire — Business & Personal Brain Dump
description: A two-part structured questionnaire used during Claude Cowork onboarding to capture the rich context that makes the AI operating system effective.
bundle: ai-engineering
topic: claude-code-practice
tags: [context-engineering, claude-code, knowledge-management, agent-workflows]
source: ../../../Resources/documents/templates/Business__Personal_Context_Questionnaire.md
resource:
timestamp: 2026-05-17T08:21:13Z
status: active
related:
  - ai-engineering/claude-code-practice/claude-cowork-full-guide.md
  - ai-engineering/claude-code-practice/claude-cowork-setup-guide.md
  - ai-engineering/harness-engineering/harnessing-claude-intelligence.md
---

# Claude Cowork Context Questionnaire — Business & Personal Brain Dump

**Source:** [Business__Personal_Context_Questionnaire.md](../../../Resources/documents/templates/Business__Personal_Context_Questionnaire.md)
**Author:** Ben AI

---

## Summary

A two-part structured questionnaire used during Claude Cowork onboarding to capture the rich context that makes the AI operating system effective. Part 1 covers business context (7 sections: business, customers, positioning, brand/voice, goals/strategy, operations/tools, key relationships) feeding into vault files `organization.md`, `brand.md`, `strategy.md`, `icp.md`, and `stakeholders.md`. Part 2 covers personal context (6 sections) feeding into `Profile.md` and `CLAUDE.md`. Designed for voice input via Wispr Flow; the richer the answers, the better the agent performs.

## Design Philosophy

The questionnaire is deliberately open-ended rather than structured (source: Business__Personal_Context_Questionnaire.md): "Brain dump everything. Don't filter. Don't worry about structure. That's Claude's job." The agent's role is to structure the brain dump into vault files, not to receive pre-structured input.

**Voice-first.** The recommended input method is Wispr Flow — voice transcription directly into any text field. Ten minutes of talking gives Claude more context than 30 minutes of typing (source: Business__Personal_Context_Questionnaire.md).

**Existing docs shortcut.** If the user has onboarding questionnaires, brand briefs, buyer persona docs, content strategy docs, or any prior professional documentation, these should be fed in alongside or instead of answering from scratch. Claude processes them and extracts what's needed (source: Business__Personal_Context_Questionnaire.md).

## Part 1: Business Context

Seven sections, each feeding a specific vault file (source: Business__Personal_Context_Questionnaire.md):

| Section | Vault file | Key questions |
|---------|-----------|---------------|
| Your Business | `organization.md` | What it does, industry/niche, competitors, trends, pain points, recent changes, required expertise |
| Your Customers | `icp.md` | Ideal customer profile, problems solved, awareness level, before/after state, why they choose you |
| Your Positioning | `organization.md` + `icp.md` | Who/what you're against, differentiation, market perception, core value prop, big idea, key messages |
| Your Brand & Voice | `brand.md` | Personality adjectives, tone, signature phrases, things never said, content examples |
| Goals & Strategy | `strategy.md` | Top quarterly priorities, 12-month targets, current metrics, single most important lever, past failures |
| Operations & Tools | `organization.md` | Typical week, tools used, existing SOPs, most time-consuming workflows, what "good output" looks like |
| Key Relationships | `stakeholders.md` | Team members, external partners, important clients, advisors |

## Part 2: Personal Context

Six sections feeding `Profile.md` and `CLAUDE.md` (source: Business__Personal_Context_Questionnaire.md):

| Section | Purpose |
|---------|---------|
| About You | Name, responsibilities, tenure, personal 6–12 month goals |
| Professional Journey | Career milestones, turning points, motivation for current role, proud results |
| Expertise & Point of View | Expert topics, contrarian takes, proprietary knowledge, causes |
| Working Style | Communication style, tools, deep-focus vs. sprints, daily schedule, frustrations |
| How You Want Claude to Work With You | Tone preferences, always-do rules, never-do rules, AI experience level, what frustrates you about AI |
| What Success Looks Like | What would make setup "worth it", first use case, ideal state in 6 months |

## Output Command

After completing each part, the instruction to Claude is (source: Business__Personal_Context_Questionnaire.md):

**Business:** *"Structure this into my vault files: `organization.md`, `brand.md`, `strategy.md`, `icp.md`, and `stakeholders.md`."*

**Personal:** *"Structure my business brain dump into `organization.md`, `brand.md`, `strategy.md`, `icp.md`, and `stakeholders.md`. Structure my personal brain dump into my Profile and `CLAUDE.md`."*

## Relationship to Existing Cowork Articles

This questionnaire is the intake tool for the Claude Cowork setup described in [[claude-cowork-setup-guide|Claude Cowork Setup Guide]] (Step 2: "Configure your global instructions") and [[claude-cowork-full-guide|Claude Cowork Full Guide]]. It is the structured method for building out the `about-me/`, `brand.md`, `strategy.md`, and related vault files that those articles reference (source: Business__Personal_Context_Questionnaire.md).

## Key Takeaways

- Voice input (Wispr Flow) is the intended capture method; 10 minutes talking beats 30 minutes typing.
- Existing agency onboarding docs, brand briefs, and strategy docs are direct inputs — feed them in, don't re-type.
- The questionnaire produces seven vault files (five business + Profile + CLAUDE.md preferences).
- The quality of the AI operating system is directly proportional to the richness of the initial brain dump.
- This is a one-time setup artefact; vault files are updated incrementally afterward as context evolves.

## Related

- [[claude-cowork-full-guide|Claude Cowork — Full Practical Guide]] — the full guide to the AI OS this questionnaire sets up
- [[claude-cowork-setup-guide|Claude Cowork Setup Guide]] — the 7-step setup process where this questionnaire appears
- [[harnessing-claude-intelligence|Harnessing Claude's Intelligence]] — patterns for using Claude knowledge effectively once context is loaded

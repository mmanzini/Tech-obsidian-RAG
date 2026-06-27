---
type: synthesis
title: Claude Agents for Financial Services — 2026 Launch
description: Anthropic launched ten ready-to-run agent templates for financial services, Claude add-ins for Microsoft 365 (Excel, PowerPoint, Word; Outlook coming), and new data connectors for financial data providers.
bucket: ai-engineering
topic: ai-native-banking
tags: [ai-native-business, multi-agent, claude-code, skills-and-hooks, long-running-agents]
source: ../../../Resources/web-clippings/2026-05-06-Agents%20for%20financial%20services%20and%20insurance.md
resource:
timestamp: 2026-05-09T09:03:44Z
status: active
related:
  - ai-engineering/ai-native-banking/ai-native-banking-os.md
  - ai-engineering/ai-native-banking/banking-predictions-2026.md
  - ai-engineering/claude-code-practice/claude-managed-agents-memory.md
---

# Claude Agents for Financial Services — 2026 Launch

**Source:** [2026-05-06-Agents for financial services and insurance.md](../../../Resources/web-clippings/2026-05-06-Agents%20for%20financial%20services%20and%20insurance.md)
**Author:** Anthropic
**Published:** 2026-05-05

---

## Summary

Anthropic launched ten ready-to-run agent templates for financial services, Claude add-ins for Microsoft 365 (Excel, PowerPoint, Word; Outlook coming), and new data connectors for financial data providers. Templates ship as Claude Code/Cowork plugins and as Claude Managed Agents cookbooks. Best paired with Claude Opus 4.7, which leads the Vals AI Finance Agent benchmark at 64.37%.

## Ten Agent Templates

**Research and client coverage:**
- **Pitch builder** — target lists, comparables, pitchbooks
- **Meeting preparer** — client/counterparty briefs before calls
- **Earnings reviewer** — transcript/filing analysis, model updates, thesis flag
- **Model builder** — financial models from filings and data feeds
- **Market researcher** — sector/issuer developments, news synthesis, credit/risk flags

**Finance and operations:**
- **Valuation reviewer** — comparables check, methodology, firm standards
- **General ledger reconciler** — GL reconciliation, NAV calculations
- **Month-end closer** — close checklist, journal entries, close reports
- **Statement auditor** — financial statement consistency and audit-readiness
- **KYC screener** — entity file assembly, source document review, compliance escalations

Each template packages three things: skills (instructions + domain knowledge), connectors (governed data access), and subagents (specialist sub-models called by the main agent). (source: 2026-05-06-Agents for financial services and insurance.md)

## Two Deployment Modes

**Plugin mode (Claude Cowork or Claude Code):** template runs alongside the analyst using desktop software; context carries across Excel → PowerPoint → Outlook without re-explaining. Available on all paid plans. (source: 2026-05-06-Agents for financial services and insurance.md)

**Managed Agent mode (Claude Platform):** same template runs autonomously on the Claude Platform for whole-book or nightly-schedule work. Provides long-running sessions, per-tool permissions, managed credential vaults, and a full audit log in Claude Console. In public beta. (source: 2026-05-06-Agents for financial services and insurance.md)

## Microsoft 365 Integration

Claude add-ins for Excel, PowerPoint, and Word are generally available. Claude for Outlook is coming soon. Context carries automatically between applications — starting a financial model in Excel doesn't require re-explaining it when moving to PowerPoint. Dispatch (assign by text or voice) enables background work on local files while the analyst is away. (source: 2026-05-06-Agents for financial services and insurance.md)

## New Data Connectors

New connectors provide governed real-time access: Dun & Bradstreet (verified business identity), Fiscal AI (fundamentals coverage), Financial Modeling Prep (quotes, filings, transcripts), Guidepoint (100,000+ expert interview transcripts), IBISWorld (industry-level revenue/risk data), SS&C IntraLinks (deal room access), Third Bridge (primary-source expert interviews), Verisk (insurance data). **Moody's** launched an MCP app with proprietary credit ratings on 600M+ entities. (source: 2026-05-06-Agents for financial services and insurance.md)

## Key Takeaways

- Ten production-ready agent templates for the most common finance workflows — available as plugins (immediate) or Managed Agents (autonomous, beta) (source: 2026-05-06-Agents for financial services and insurance.md)
- Microsoft 365 integration closes the last-mile gap: models in Excel flow to decks in PowerPoint without context re-entry (source: 2026-05-06-Agents for financial services and insurance.md)
- Ecosystem of 10+ financial data connectors and Moody's MCP app makes agents as good as the underlying data (source: 2026-05-06-Agents for financial services and insurance.md)
- Opus 4.7 leads Vals AI Finance Agent benchmark (64.37%); recommended for production financial tasks (source: 2026-05-06-Agents for financial services and insurance.md)

## Related

- [[ai-native-banking-os|AI-Native Banking OS]] — Backbase's four-fabric architecture for deploying AI safely at scale
- [[banking-predictions-2026|Banking Predictions 2026]] — segment-level AI adoption outlook
- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — cross-session memory for Managed Agents used in these templates

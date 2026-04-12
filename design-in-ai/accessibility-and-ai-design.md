# Accessibility and AI Design

AI design tools produce visually appealing output but systematically fail accessibility standards. This is the most significant practical limitation of current AI-assisted design.

## Current State

No AI design tool produces WCAG-compliant output without manual review — including Stitch, Figma AI, Framer AI and Builder.io.

### Known Failures in [[google-stitch|Stitch]] Output
- **Colour contrast** — Regularly fails WCAG AA minimum (4.5:1 normal text, 3:1 large text)
- **Touch targets** — Insufficient sizing, fails WCAG 2.1 AA (44x44px minimum)
- **Semantic HTML** — Generic divs instead of button, nav, section, article
- **ARIA attributes** — Missing labels, roles and states on interactive elements
- **Keyboard navigation** — No focus management, tab order or keyboard patterns
- **Alt text** — Missing or placeholder

### What Design.md Can and Cannot Encode

**Can encode:** Visual tokens (hex values, font sizes, spacing)

**Cannot encode:** Contrast ratio requirements, touch target minimums, ARIA generation rules, keyboard navigation patterns, semantic HTML preferences, screen reader behaviour

Text-based accessibility guidance in Design.md (e.g. "Minimum contrast ratio: 4.5:1") is not enforced — relies on the agent choosing to follow it, which is inconsistent.

## How Other Tools Handle Accessibility

- **Figma** — Plugins (Corpowid, Aulys) for WCAG scanning, contrast checking, colour blindness simulation
- **Framer** — Accessibility panel: semantic tags, alt text, custom tab order, reduced motion, contrast checker
- **Builder.io** — "Accessibility Expert" AI persona; requires keyboard-accessible interactive elements; generates ARIA attributes

## Remediation Strategies

### Tier 1: Automated (~70-80% of issues)
- Tools: TestParty, Accesstive, Equally AI
- Handles: missing alt text, heading hierarchy, basic contrast, missing ARIA labels

### Tier 2: Expert Review
- Contextual alt text quality, semantic ARIA correctness, keyboard navigation logic, real assistive technology testing

### Tier 3: Design System Integration (most effective long-term)
1. Generate UI via AI → 2. Immediate accessibility audit → 3. Automated remediation → 4. Escalate failures → 5. Integrate fixes back into design system (prevents future repetition)

### Tier 4: Agentic Monitoring (emerging)
- Dedicated accessibility agents reviewing code commits, flagging violations in PRs, suggesting WCAG-compliant alternatives
- Integrates with CI/CD for always-on monitoring

## Regulatory Context

- **European Accessibility Act (EAA)** — Effective June 2025
- **US Higher Education WCAG 2.1 AA** — Deadline April 2026
- **WCAG 3.0** — In development (expected no earlier than 2028). Explicitly requires documented human review process for AI-generated content

## Emerging Solutions

- **TypeUI** — Bakes WCAG 2.2 AA compliance into generated design skills (better starting point than raw Design.md)
- **Google Research: Natively Adaptive Interfaces (NAI)** — Context-informed, agent-driven interfaces that adapt to user needs
- **Figma MCP Server** — Agents reuse existing (presumably accessible) components rather than generating new ones

## Key Takeaways

- No AI design tool produces accessible output by default — human oversight is non-negotiable
- Design.md cannot enforce accessibility constraints, only suggest them in text
- Integrated workflow (generate → audit → remediate → feed back into system) is the practical recommendation
- Regulatory deadlines (EAA, WCAG 3.0) make this a compliance issue, not optional quality
- AI accessibility agents in CI/CD are the most promising emerging approach
- TypeUI provides the best starting point for accessible Design.md files

## Sources

- [LogRocket: I Tried Google Stitch](https://blog.logrocket.com/ux-design/i-tried-google-stitch-heres-what-i-loved-hated/)
- [Siteimprove: Agentic Accessibility](https://www.siteimprove.com/blog/agentic-accessibility/)
- [Accessible.org: AI Hybrid WCAG Audits](https://accessible.org/ai-hybrid-wcag-audits/)
- [AbilityNet: WCAG 3.0 Update 2026](https://abilitynet.org.uk/resources/digital-accessibility/what-expect-wcag-30-web-content-accessibility-guidelines)
- [Google Research: AI Agents and Universal Design](https://research.google/blog/how-ai-agents-can-redefine-universal-design-to-increase-accessibility/)

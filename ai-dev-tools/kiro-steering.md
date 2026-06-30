---
type: synthesis
title: Kiro Steering
description: Steering files give Kiro persistent knowledge about your workspace via markdown documents.
bundle: ai-engineering
topic: ai-dev-tools
tags: [context-engineering, harness-engineering, skills-and-hooks, knowledge-management, claude-code]
source: https://kiro.dev/docs/steering/
resource: https://kiro.dev/docs/steering/
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# Kiro Steering

**Source:** [Kiro Docs](https://kiro.dev/docs/steering/)

---

## Summary

Steering files give Kiro persistent knowledge about your workspace via markdown documents. Instead of explaining conventions in every chat, steering files ensure the AI consistently follows established patterns, libraries, and standards. Conceptually equivalent to CLAUDE.md / AGENTS.md, but with **multiple inclusion modes** and explicit support for workspace + global + team scopes.

## Key Benefits

- **Consistent code generation** — every component follows team patterns
- **Reduced repetition** — don't re-explain standards every chat
- **Team alignment** — all developers work from the same standards
- **Scalable project knowledge** — documentation that grows with the codebase

## File Scope

### Workspace Steering
- Lives in `.kiro/steering/` at workspace root
- Applies only to that workspace

### Global Steering
- Lives in `~/.kiro/steering/`
- Applies to all workspaces
- Workspace steering wins on conflicts

### Team Steering
- Global steering files distributed via MDM, Group Policy, or central repos
- Same `~/.kiro/steering/` location

## Foundational Steering Files

Three core files Kiro can generate automatically:

- **`product.md`** — Product purpose, users, key features, business objectives (the *why*)
- **`tech.md`** — Frameworks, libraries, dev tools, technical constraints
- **`structure.md`** — File organization, naming conventions, import patterns, architectural decisions

These are included in every interaction by default.

## Inclusion Modes

Configured via YAML front matter:

### Always (default)
```yaml
---
inclusion: always
---
```
Loaded into every interaction. Best for: workspace-wide standards, security policies, universal conventions.

### Conditional (file-match)
```yaml
---
inclusion: fileMatch
fileMatchPattern: "components/**/*.tsx"
---
```
Loaded only when working with matching files. Supports arrays of patterns. Best for: domain-specific standards (component patterns, API design, test conventions).

### Manual
```yaml
---
inclusion: manual
---
```
Available on-demand by referencing `#steering-file-name` in chat (or via `/` slash commands). Best for: troubleshooting guides, migration procedures, occasionally-needed context.

### Auto
```yaml
---
inclusion: auto
name: api-design
description: REST API design patterns. Use when creating or modifying API endpoints.
---
```
Loaded automatically when the request matches the description (similar to skills). Best for: context-heavy guidance that shouldn't always-on.

## File References

Link to live workspace files to keep steering current:
```markdown
#[[file:api/openapi.yaml]]
#[[file:components/ui/button.tsx]]
```

## AGENTS.md Support

Kiro also reads `AGENTS.md` files (the cross-tool standard). These are always included — they don't support inclusion modes.

## Best Practices

- **Keep files focused** — one domain per file
- **Use clear names** — `api-rest-conventions.md`, `testing-unit-patterns.md`
- **Include context** — explain *why* decisions were made, not just what
- **Provide examples** — code snippets and before/after comparisons
- **Security first** — never include API keys or secrets (steering is in your codebase)
- **Maintain regularly** — review during sprint planning; treat changes like code reviews

## Common Steering Strategies

- **API Standards** (`api-standards.md`)
- **Testing Approach** (`testing-standards.md`)
- **Code Style** (`code-conventions.md`)
- **Security Guidelines** (`security-policies.md`)
- **Deployment Process** (`deployment-workflow.md`)

## Key Takeaways

- Steering = persistent agent context, structured into focused, reusable files
- **Four inclusion modes** (always / conditional / manual / auto) let you balance context relevance vs. instruction budget — directly addressing the [[skill-issue-harness-engineering|"too many tools / too much context" problem]]
- **Workspace + global + team scopes** make this distributable across orgs
- **Auto mode mirrors skills** — description-matched activation
- Better than monolithic CLAUDE.md when you need conditional context — though both ETH Zurich findings still apply: keep instructions concise and avoid auto-generation
- File references (`#[[file:...]]`) keep steering linked to live workspace artifacts

## See Also

- [[kiro-hooks|Kiro Hooks]] — event-driven actions that complement steering's persistent context
- [[kiro-specs|Kiro Specs]] — specs use steering context during generation
- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — equivalent CLAUDE.md / AGENTS.md analysis and the ETH Zurich study

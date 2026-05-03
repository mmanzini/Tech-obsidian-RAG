---
source: QuantumBlack / AI by McKinsey — "Agentic workflows for software development" (Feb 2026)
---

# McKinsey — Agentic Workflows for Software Development

McKinsey QuantumBlack's field-tested pattern for moving beyond the "developer + copilot" model to **agents executing the full SDLC autonomously**, with humans entering only at the PR review stage.

## Summary

McKinsey QuantumBlack's field-tested pattern for AI-driven software development centres on a two-layer architecture: a deterministic orchestration engine that enforces phase transitions and dependency tracking, paired with bounded specialised agents (requirements, architecture, coding, knowledge, critic) that execute within each phase. The critical insight is that self-orchestrating agents fail at scale because they skip steps and create circular dependencies — only deterministic orchestration plus per-phase eval gates produces the predictable, traceable SDLC that rivals Agile's original promise while running full requirement-to-implementation cycles in hours rather than weeks.

## The Problem with Copilots
- "Developer with AI assistant" makes individuals faster but **idea-to-live-feature** improvement is small in enterprise contexts.
- **Handoffs between phases are where context goes to die** — Slack threads, assumptions in heads, rationale re-litigated.
- Agent-specific failure modes:
  - **Unpredictable outcomes** — same prompt, different developers, different results. Quality depends on individual skill, not systematic process.
  - **No audit trail** — decisions live in chat windows; no answer to "why did we choose Redis over SQS?"

The fix is workflow design, not better models. Value comes when agents **operate inside conventions, structured specifications, and deterministic processes** — i.e. [[sdd-overview|Spec-Driven Development]].

## The Two-Layer Pattern

### Layer 1: Orchestration (Deterministic)
A rule-based workflow engine — **not** an agent — that:
- **Enforces phase transitions** (requirements → design → tasks → impl).
- **Manages dependencies** between tasks.
- **Tracks artifact state** via frontmatter state machines (draft → in-review → approved → complete).
- **Triggers agents at the right time** ("when REQ-001 is approved, generate technical tasks").

Critical design choice: agents are bad at meta-level decisions about workflow sequencing. They're good at bounded creative work. Early experiments with self-orchestrating agents on larger codebases routinely **skipped steps, created circular dependencies, or got stuck in analysis loops**.

### Layer 2: Execution (Bounded Agents + Evals)
Specialized agents per phase (microservices analogy):
- **Requirements agent** — breaks down features.
- **Architecture agent** — proposes designs with rationale.
- **Coding agent** — implements + tests.
- **Knowledge agent** — called by other agents to query project context and log assumptions.
- **Critic agent** — runs at end of each phase for judgment-call validation.

Each agent is essentially a `SKILL.md` — bounded instructions, templates, and evaluation criteria. Agents bring **zero implicit understanding** (unlike a junior dev) — every guideline must be explicit and machine-readable.

## Evaluation Gates (Per Phase)
1. **Deterministic checks first**: linters, tests, structural validation (frontmatter present? cross-references resolve?). Fast and reliable.
2. **Critic agent second**: judgment calls ("are these acceptance criteria testable?", "does this match established patterns?"). Returns pass/fail + explanation.
3. **Iterate** within the phase (capped at 3–5 attempts) until both pass.
4. **Only then** does the workflow advance.

If iteration limit is hit, workflow **fails and rolls back for human intervention**.

## Folder Convention as Workflow Contract

```
.sdlc/
  context/                  # Persistent project context
    project-overview.md
    architecture.md
    conventions.md
  templates/                # Reusable artifact templates
    requirement-template.md
    task-template.md
  specs/                    # Per-feature specifications
    REQ-001-notification-system/
      requirement.md
      tasks/
        TASK-001-implement-notification-service.md
        TASK-002-create-email-channel.md
  knowledge/                # Accumulated assumptions & answered questions

src/
tests/
AGENTS.md
```

The structure isn't an organizational preference — it's part of the engine's contract:
- **Persistent vs per-feature**: `context/` applies always; `specs/` belongs to one requirement.
- **Co-location**: everything about REQ-001 lives in one folder.
- **Read/write zones**: requirements agent writes to `specs/REQ-001-*/`, reads from `context/`.
- **Parseable relationships**: `REQ-*` / `TASK-*` prefixes let the engine compute dependency graphs programmatically.

## End-to-End: Notification System Example

1. **Branch creation** — workflow engine creates `agent/REQ-001-notification-system`. Git is the state store; commits represent completed phases.
2. **Phase 1: Requirements** — agent produces `requirement.md` with frontmatter, acceptance criteria, external dependencies. Knowledge agent calls log assumptions ("using async queue-based delivery") as **structured, reviewable items**, not freeform notes.
3. **Phase 2: Architecture** — proposes Redis-backed Bull/BullMQ queue with rationale, PostgreSQL with 90-day partitioned retention. Commits architecture proposal.
4. **Phase 3: Tasks** — generates dependency graph of TASK-001…TASK-005. Eval checks for cycles and file coverage.
5. **Phase 4: Implementation** — agent writes code per task; deterministic evals run; engine commits with task ID; PR opens.
6. **Human enters here** — reviews the **complete** feature (specs + architecture + tasks + code + assumptions) in one place. Not partial work mid-phase.

## Traceability in Action
Six months later: "Why Redis instead of SQS?"
- `notification.service.ts` → TASK-001 → REQ-001 → architecture commit with rationale ("Redis already in stack; Bull handles retry/backoff/DLQ out of box") → PR approval = decision documented.

The chain of reasoning from business requirement → architecture → implementation is preserved in folder structure and explicit links. Especially valuable in **regulated industries**.

## "Isn't This Just Waterfall?"
**Yes — and that's the point.**

Waterfall's reputation came from bad economics (specs took months, requirements drifted, docs stale on arrival). Agents change the economics: a full requirements → architecture → tasks → impl cycle runs in **hours**. PMs kick off three competing experiments Monday morning, review working implementations by afternoon. If reqs change, you don't update stale docs — you **run a fresh cycle**.

> "We're not avoiding waterfall's shape. We're solving the problems that made it fail. And ironically in delivering multiple iterations a day, we can deliver on Agile's original promise better than ever before."

## Knowledge Management: Tool Call vs File-Based
- **At scale (multi-repo, complex)**: a dedicated **knowledge agent** called via structured tool calls. Returns either an answer (grounded) or an assumption (logged). Every question/assumption appears as structured data in the PR — reviewers approve or reject assumptions alongside code, and corrections feed back into the knowledge base.
- **Simple cases**: agents write assumptions directly to `.sdlc/knowledge/assumptions/` files via a knowledge skill. Still reviewable in PRs.
- Start file-based; graduate to a dedicated agent when files become unwieldy.

Why not "just give the agent memory"? Retrieved memories consume the same tokens needed for actual work. A dedicated knowledge agent keeps individual agents focused.

## What's Worked — Recommendations

### For Leaders
- **Build the end-to-end factory from day one** — humans only enter at PR. Mid-workflow interruption destroys the speed advantage and leads to worse decisions on partial context.
- **Optimize for throughput** — lean cross-functional teams kick off, agents execute, team reviews together.
- **Rewire for agent-first** — organizational change, not just technical. Decisions in Slack threads can't scale your output: write it down or accept it won't be in the process.

### For Engineers
- **Tight feedback loops via evals** — encode "did the agent misunderstand the requirement?" as an eval, not a human review.
- **Package domain expertise as portable skills** (`SKILL.md`) — modular instructions agents can reuse. Workflows stay stable; skills are the layer that adapts per team. Pair with evals and traces to monitor and roll forward/back like software.

## Key Takeaways
- The unit of value is not "developer with copilot" but **the autonomous SDLC factory** — the speed advantage comes from agents executing the full cycle without mid-workflow interruption.
- **Deterministic orchestration + bounded execution + per-phase evals** is the reliable pattern. Self-orchestrating agents fail at scale; agents-as-microservices behind a rule engine works.
- Folder structure and frontmatter aren't conventions — they're **the engine's machine-readable contract**. This is what makes [[sdd-overview|Spec-Driven Development]] enforceable.
- Knowledge management via **structured tool calls** (or files at small scale) makes assumptions reviewable in the PR rather than buried in chat. This is the audit trail that copilots lack.
- Agents fix waterfall's economics, not its shape. The real win is being able to run **multiple complete cycles per day** — closer to Agile's original promise than Agile-as-practiced.
- Cultural prerequisite: if decisions live informally in Slack/hallways, agents can't scale your output. This is an organizational change, not a technical one.

## Related
- [[sdd-overview|Spec-Driven Development]] — McKinsey's pattern is essentially SDD operationalized with explicit eval gates
- [[sdd-workflow|SDD Workflow]] — same 6-phase shape (Brief → Specify → Design → Task → Implement → Validate)
- [[sdd-roles-and-boundaries|SDD Roles & Boundaries]] — corresponds to McKinsey's IC/critic/knowledge agent split
- [[research-plan-implement|Research Plan Implement (RPI)]] — sibling phased pattern at single-feature granularity
- [[twelve-factor-agents|Twelve-Factor Agents]] — the production engineering principles the bounded execution layer relies on
- [[ai-native-banking-os|AI-Native Banking OS]] — the same "deterministic + probabilistic side-by-side, governed" pattern applied to banking workflows

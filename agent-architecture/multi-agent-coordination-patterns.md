# Multi-Agent Coordination Patterns

Five canonical coordination patterns for multi-agent systems, with decision criteria for choosing between them.

Source: Anthropic, [Multi-agent coordination patterns: Five approaches and when to use them](https://claude.com/blog/multi-agent-coordination-patterns)

## The Five Patterns

### 1. Generator-Verifier
- Simplest multi-agent pattern; most widely deployed
- Generator produces output → Verifier evaluates against explicit criteria → accepts or rejects with feedback → loop until accepted or max iterations
- **Best for:** quality-critical output where evaluation criteria can be made explicit (code gen + tests, fact-checking, compliance verification)
- **Failure mode:** verifier told only to check if output is "good" rubber-stamps everything — criteria must be specific
- Assumes generation and verification are separable skills
- Needs max iteration limit + fallback (escalate to human, return best attempt with caveats)

### 2. Orchestrator-Subagent
- One agent plans, delegates, and synthesises; subagents handle bounded subtasks and terminate after returning results
- Claude Code uses this pattern — main agent dispatches background subagents for codebase search / independent investigation
- **Best for:** clear task decomposition with minimal interdependence (e.g., code review: security + tests + style + architecture checks in parallel)
- **Failure mode:** orchestrator becomes information bottleneck — discoveries in one subagent relevant to another must travel back through orchestrator, losing detail

### 3. Agent Teams
- Coordinator spawns persistent worker agents that stay alive across many assignments, accumulating context
- Unlike orchestrator-subagent, workers don't terminate between tasks
- **Best for:** parallel, independent, long-running subtasks that benefit from sustained context (e.g., framework migration service-by-service)
- **Failure mode:** requires true independence — workers can't share intermediate findings; shared resources (same codebase/files) cause conflicts

### 4. Message Bus
- Shared communication layer: agents publish events and subscribe to topics; router delivers matching messages
- New agent types can be added without rewiring existing connections
- **Best for:** event-driven pipelines where workflow emerges from events, not predetermined sequences; growing agent ecosystems (e.g., security operations automation)
- **Failure mode:** tracing cascades across agents is hard; router misclassification causes silent failures

### 5. Shared State
- Agents coordinate through a persistent store (database, file system, document) — no central coordinator
- Agents read, act, write back; work ends on termination condition (time limit, convergence threshold, designated agent deciding sufficiency)
- **Best for:** collaborative research where agents build on each other's findings in real time; eliminates single point of failure
- **Failure mode:** reactive loops (Agent A writes → B responds → A responds → infinite cycle); needs first-class termination conditions

## Decision Framework

| Question | If yes → | If no → |
|---|---|---|
| Is output quality the bottleneck with explicit eval criteria? | Generator-Verifier | ↓ |
| Are subtasks short, bounded, and clearly decomposable? | Orchestrator-Subagent | ↓ |
| Do workers need sustained context across multiple cycles? | Agent Teams | ↓ |
| Does the workflow emerge from events (not predetermined)? | Message Bus | ↓ |
| Do agents need each other's findings in real time? | Shared State | — |

### Key Comparisons
- **Orchestrator-subagent vs Agent teams** — How long must workers retain context? Short bounded → orchestrator. Multi-step sustained → teams
- **Orchestrator-subagent vs Message bus** — How predictable is the workflow? Fixed pipeline → orchestrator. Variable/event-driven → bus
- **Agent teams vs Shared state** — Do agents need each other's findings? Independent partitions → teams. Collaborative → shared state
- **Message bus vs Shared state** — Do events flow as pipeline or accumulate? Pipeline → bus. Accumulating knowledge → shared state

## Key Takeaways

- Start with the simplest pattern that could work — default recommendation is orchestrator-subagent
- Production systems often combine patterns (orchestrator overall + shared state for collaborative subtasks; message bus routing + agent team workers)
- Choose patterns based on problem structure, not sophistication
- Context-centric decomposition: divide by what context each agent needs, not by work type
- These are building blocks, not mutually exclusive choices

## Related

- [[twelve-factor-agents|Twelve-Factor Agents]] — foundational principles these patterns build on
- [[seeing-like-an-agent|Seeing Like an Agent]] — how Claude Code implements the orchestrator-subagent pattern
- [[subagents-in-claude-code|Subagents in Claude Code]] — practical application of orchestrator-subagent
- [[agentic-engineering-approaches-compared|Agentic Engineering Approaches Compared]] — broader workflow classification
- [[lethal-trifecta-and-agentic-patterns|Lethal Trifecta]] — trust and security constraints relevant to multi-agent coordination

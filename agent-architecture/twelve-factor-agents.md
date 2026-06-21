---
type: synthesis
title: Twelve-Factor Agents
description: A set of 12 principles for building LLM-powered software reliable enough to put in front of production customers.
bucket: ai-engineering
topic: agent-architecture
tags: []
source: https://github.com/humanlayer/12-factor-agents
resource: https://github.com/humanlayer/12-factor-agents
timestamp: 2026-04-29T21:24:51Z
status: active
related: []
---

# Twelve-Factor Agents

**Source:** [humanlayer/12-factor-agents](https://github.com/humanlayer/12-factor-agents)
**Author:** Dex Horthy (HumanLayer)
**Inspired by:** [12 Factor Apps](https://12factor.net/)

---

## Summary

A set of 12 principles for building LLM-powered software reliable enough to put in front of production customers. Core thesis: most products billing themselves as "AI agents" are mostly deterministic software with LLM steps sprinkled in at the right points. Frameworks tend to break down past the 70-80% quality bar — the path forward is to take **small, modular concepts** from agent-building and incorporate them into existing products.

![[12factor-software-dag.png]]
*Software has always been a directed graph — there's a reason we drew flow charts.*

## How We Got Here

![[12factor-dag-orchestrators.png]]
*DAG orchestrators (Airflow, Prefect, Dagster, Inngest, Windmill) added observability and retries to the same graph pattern.*

- ~20 years ago: DAG orchestrators (Airflow, Prefect, Dagster, Inngest, Windmill) became popular for routing work through graphs
- The promise of agents was that you could **throw the DAG away** — give the LLM goals + transitions, let it figure out the path
- The reality: this loop ("here's a prompt, here's tools, loop until done") doesn't scale past 70-80% quality
- The fix: bring engineering discipline back to the loop

![[12factor-agent-dag.png]]
*The agent promise: throw the DAG away, give the LLM goals + transitions, let it figure out the path.*

![[12factor-agent-loop.gif]]
*The agent loop in motion across multiple steps.*

## The Agent Loop

```
initial_event = {"message": "..."}
context = [initial_event]
while True:
  next_step = await llm.determine_next_step(context)
  context.append(next_step)
  if next_step.intent === "done":
    return next_step.final_answer
  result = await execute_step(next_step)
  context.append(result)
```

Three steps: LLM picks next step → deterministic code executes it → result appended to context.

## The Builder's Journey (and where it breaks)

1. Decide to build an agent
2. Product/UX work
3. Grab `$FRAMEWORK` and ship fast
4. Hit 70-80% quality bar
5. Realize 80% isn't good enough for customer-facing features
6. Realize getting past 80% requires reverse-engineering the framework, prompts, flow
7. **Start over from scratch**

## The 12 Factors

1. **Natural Language to Tool Calls** — convert user intent to structured tool invocations
2. **Own Your Prompts** — don't outsource them to a framework
3. **Own Your Context Window** — *the* core context engineering principle; control what's in the window
4. **Tools Are Just Structured Outputs** — strip the magic, they're just JSON
5. **Unify Execution State and Business State** — don't bifurcate
6. **Launch / Pause / Resume with Simple APIs** — agents must be controllable
7. **Contact Humans with Tool Calls** — human interaction is just another tool
8. **Own Your Control Flow** — don't surrender it to the framework
9. **Compact Errors into Context Window** — let the agent learn from failures within the loop
10. **Small, Focused Agents** — narrow scope beats general-purpose
11. **Trigger from Anywhere, Meet Users Where They Are** — Slack, email, webhook, cron
12. **Make Your Agent a Stateless Reducer** — pure function over context, easier to test/scale

**Honorable Mention — Factor 13:** Pre-fetch all the context you might need.

## Key Themes

- **Mostly software, sprinkled with LLM** — the best agents aren't pure agents, they're software with LLM judgment at high-leverage points
- **Modular concepts > full frameworks** — pull individual ideas (own your prompts, own your context) into existing products
- **These principles can be applied by skilled software engineers without an AI background**
- **Even if LLMs get exponentially smarter**, core engineering techniques will still make LLM software more reliable, scalable, and maintainable

## Key Takeaways

- The best agents are mostly deterministic software with LLM judgment at the right points
- **Own your prompts, your context window, and your control flow** — surrendering these to a framework is what breaks past 80%
- Tools are structured outputs; nothing more, nothing less
- Small focused agents beat sprawling general-purpose ones (foreshadows [[skill-issue-harness-engineering|sub-agents as context firewalls]])
- Stateless reducer design makes agents easy to test, replay, and scale
- The fastest path to production-quality AI is incremental — drop modular concepts into existing products, not greenfield rewrites
- **Context engineering** (the broader discipline this sits inside) is the dominant lever for reliability

## See Also

- [[skill-issue-harness-engineering|Skill Issue — Harness Engineering for Coding Agents]] — harness engineering as a subset of context engineering, applied to coding agents specifically
- [[effective-harnesses-long-running|Effective Harnesses for Long-Running Agents]] — Anthropic's application of similar principles
- [[research-plan-implement|Research Plan Implement (RPI)]] — workflow that embodies several factors (own context, small focused phases)
- [[from-hierarchy-to-intelligence|From Hierarchy to Intelligence]] — what becomes possible at the org level when you build production-grade agents
- [[lethal-trifecta-and-agentic-patterns|Lethal Trifecta and Agentic Engineering Patterns]] — the security model these factors must mitigate (private data + untrusted input + external send)

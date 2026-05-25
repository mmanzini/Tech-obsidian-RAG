# Managed Agents Dreams — Async Memory Curation via Session Reflection

**Source:** [Dreams](https://platform.claude.com/docs/en/managed-agents/dreams)

---

## Summary

Dreams is a research preview feature in the Managed Agents API that lets Claude clean up and reorganize agent memory stores asynchronously. A dream reads an existing memory store alongside past session transcripts, then produces a new reorganized store: duplicates merged, stale or contradicted entries replaced with the latest value, and new insights surfaced. The input store is never modified, so the output can be reviewed and discarded if unsatisfactory.

## How Dreams Work

Agents write to memory stores incrementally as they work. Over many sessions a memory store accumulates duplicates, contradictions, and stale entries. A **dream** is an asynchronous job that addresses this by (source: managed-agents-dreams.md):

1. Taking a pre-existing **memory store** as input (the store to verify, deduplicate, and reorganize)
2. Taking 1 to 100 **sessions** as input (past transcripts to mine for patterns and insights)
3. Producing a new **output memory store** — separate from the input

The output store ID appears in the dream's `outputs[]` once it starts running. The input store is never modified (source: managed-agents-dreams.md).

All Managed Agents API requests require the `managed-agents-2026-04-01` beta header. Dreams additionally require `dreaming-2026-04-21` (source: managed-agents-dreams.md).

## Creating and Tracking a Dream

Create a dream by specifying:
- `inputs`: the memory store ID and an array of session IDs
- `model`: `claude-opus-4-7` or `claude-sonnet-4-6` (both supported during research preview)
- `instructions`: optional guidance on what to focus on or ignore (e.g., "Focus on coding-style preferences; ignore one-off debugging notes")

Dreams run asynchronously, typically taking minutes to tens of minutes depending on input size. Lifecycle states: `pending` → `running` → `completed` / `failed` / `canceled` (source: managed-agents-dreams.md).

Once running, the dream's `session_id` field points at the underlying session executing the pipeline. You can stream that session's events to observe what the dream is reading and writing in real time (source: managed-agents-dreams.md).

## Using the Output

When `status` reaches `completed`, the output memory store is an ordinary memory store in your workspace. You can:
- **Leverage it**: attach it to future sessions as a `memory_store` resource in place of (or alongside) the input store
- **Discard it**: delete or archive it

The dream itself never deletes or modifies its inputs. On `failed` or `canceled`, the output store persists with partial contents for inspection (source: managed-agents-dreams.md).

If you only have session transcripts and no existing store, create an empty memory store first and pass it as the input (source: managed-agents-dreams.md).

## Lifecycle Management

- **Cancel**: moves `pending` or `running` dream to `canceled` immediately. Canceling an already-canceled dream is a no-op; canceling `completed` or `failed` returns 400 (source: managed-agents-dreams.md).
- **Archive**: sets `archived_at` on a terminal-state dream; `status` is unchanged. Archived dreams are excluded from default list responses but remain readable by ID. Archiving does not touch the output memory store (source: managed-agents-dreams.md).
- **List**: returns all non-archived dreams newest-first with `limit` (default 20, max 100) and page cursor. Pass `include_archived=true` to include archived dreams (source: managed-agents-dreams.md).

While a dream is `pending` or `running`, archiving or deleting its output store is rejected with 400. Archiving or deleting an input store or session mid-run causes the dream to fail with `input_memory_store_unavailable` or `input_session_unavailable` (source: managed-agents-dreams.md).

## Limits and Billing

| Limit | Value |
|---|---|
| Sessions per dream | 100 |
| `instructions` length | 4,096 characters |
| Supported models | `claude-opus-4-7`, `claude-sonnet-4-6` |

Dreams are billed at standard API token rates for the selected model. Cost scales roughly linearly with the number and length of input sessions (source: managed-agents-dreams.md).

Recommendation: start with a small batch of sessions and scale up once satisfied with curation quality (source: managed-agents-dreams.md).

## Key Takeaways

- Dreams solve memory store degradation: duplicates, contradictions, and stale entries that accumulate over many sessions
- The input store is always preserved — output is a new separate store
- Use `instructions` to steer what the dream focuses on or ignores
- Output is an ordinary memory store that can be attached to future sessions immediately
- Billed at standard token rates — start small to calibrate cost
- Memory is not yet supported with self-hosted sandboxes

## Related

- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — foundational memory store API that dreams clean up
- [[scaling-managed-agents|Scaling Managed Agents]] — broader Managed Agents architecture context
- [[managed-agents-self-hosted-sandboxes|Self-Hosted Sandboxes — Bring-Your-Own Execution Environment]] — execution environment context (memory not yet supported there)

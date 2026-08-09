---
type: reference
title: OKF v0.2 — normative specification
description: What Open Knowledge Format v0.2 adds over v0.1 — queryable trust and provenance in frontmatter (sources, generated, verified, status, stale_after), the actor convention and trust tiers, and the Attested Computation concept type.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- okf
- specification
- conformance
- knowledge-engineering
- interoperability
- provenance
- frontmatter
resource: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
sources:
- id: okf-spec-v0-2
  resource: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md
generated:
  by: claude-code/fable-5
  at: '2026-08-09T00:00:00Z'
status: stable
related:
- ai-engineering/knowledge-engineering/okf-spec-v0-1.md
- ai-engineering/knowledge-engineering/open-knowledge-format.md
- ai-engineering/knowledge-engineering/llm-vault-structure-spec.md
---

# OKF v0.2 — normative specification

**Source:** [knowledge-catalog/okf/SPEC.md at main](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
**Author:** Google Cloud Platform (knowledge-catalog)
**Version:** 0.2

---

## Summary

OKF v0.2 keeps the v0.1 structural core intact — bundle layout, reserved `index.md`/`log.md`, `type` as the only required frontmatter field, permissive consumption — and adds one theme: **trust and provenance as queryable frontmatter**. Where a concept came from (`sources` with per-source credibility signals), who produced and confirmed it (`generated`, `verified`, from which consumers derive a trust tier), and whether it is still current (`status`, `stale_after`) all move into structured fields (source: knowledge-catalog/okf/SPEC.md). The v0.1 companion [[okf-spec-v0-1]] documents the carried-forward base; this article covers what changed.

## Breaking changes vs v0.1

Two v0.1 constructs are renamed or relocated (source: knowledge-catalog/okf/SPEC.md):

| v0.1 | v0.2 |
|---|---|
| `timestamp:` scalar | `generated: {by, at}` — `at` records the last meaningful content change |
| `# Citations` body list | `sources:` frontmatter list |

Consumers MAY still fall back to the legacy forms when the new ones are absent, so v0.1 documents remain readable (source: knowledge-catalog/okf/SPEC.md).

## Provenance — `sources`

Each entry records material the concept derives from. `resource` is the only required subfield; the rest are objective credibility signals the consumer weighs itself — the spec deliberately stores signals, not a trust score (source: knowledge-catalog/okf/SPEC.md).

```yaml
sources:
  - id: stable-key-for-attribution   # SHOULD be present; footnote key for per-claim attribution
    resource: URL | path | scope descriptor   # REQUIRED
    title: human-readable label
    author: actor (see actor convention)
    usage_count: 42                  # liveness signal, framed by usage_window
    last_modified: 2026-07-01        # recency of the source itself
usage_window: { from: 2026-01-01, to: 2026-06-30 }
```

## Trust — `generated`, `verified`, actor convention

`generated: {by, at}` records authorship (`by` required). `verified:` is a list of independent verification events, each `{by, at}`; content changes do not carry verification forward (source: knowledge-catalog/okf/SPEC.md).

Actors follow one convention: `<producer>/<version>` for agents/tools, `human:<id>` for people, `process:<id>` for automated processes. Consumers derive a **trust tier** from `verified`, lowest to highest: no `verified` key → *unverified*; non-`human:` verifiers only → *machine-confirmed*; any `human:` verifier → *human-reviewed* (source: knowledge-catalog/okf/SPEC.md).

## Lifecycle — `status`, `stale_after`

`status` takes `draft | stable | deprecated`; absent means `stable`. `stale_after` is an absolute `YYYY-MM-DD` date — the concept is stale when today reaches it, keeping staleness a plain date comparison rather than a TTL calculation (source: knowledge-catalog/okf/SPEC.md).

## Attested Computation

A new concept type for runnable, checkable knowledge: frontmatter declares a `runtime` (BigQuery, Postgres, dbt, Python, Looker), typed `parameters`, the `computation` (file path or inline `# Computation` fence), an `executor` whose runs return a `receipt`, and a deterministic `attester` that inspects the receipt and returns a verdict — no LLM in the attestation loop (source: knowledge-catalog/okf/SPEC.md). Atlas has no computation concepts, so this type is unused here.

## Conformance

The three v0.1 rules are unchanged: parseable frontmatter on every non-reserved `.md`, non-empty `type`, reserved filenames follow their structures. All trust/provenance fields are optional — "consumers MUST NOT reject" a concept without them (source: knowledge-catalog/okf/SPEC.md). The bundle-root `index.md` MAY declare `okf_version: "0.2"`.

## How Atlas maps to v0.2

Atlas migrated in August 2026 (795 articles backfilled): `timestamp` → `generated` (actor `claude-code/atlas-consolidate`), `source` scalar → `sources:` list, status vocabulary `active`/`needs-verification` → `stable`/`draft`, `okf_version: "0.2"` declared in every bundle-root `index.md`, and strict-YAML frontmatter enforced by `okf_tools.py --check`. `verified:` is written only on explicit human confirmation, so articles default to the *unverified* tier honestly. `stale_after` is not used. Inline `(source: …)` per-claim citations remain as an Atlas extension alongside the `sources:` list (source: knowledge-catalog/okf/SPEC.md).

## Key Takeaways

- v0.2 is additive in spirit: the only breaking changes are two relocations (`timestamp` → `generated.at`, `# Citations` → `sources:`), both with legacy fallback (source: knowledge-catalog/okf/SPEC.md).
- Trust is derived, not declared — the format stores objective signals (who wrote, who confirmed, when, from what) and lets each consumer draw its own tier (source: knowledge-catalog/okf/SPEC.md).
- Conformance stays a three-rule bar with `type` as the sole required field; every v0.2 addition is optional (source: knowledge-catalog/okf/SPEC.md).

## Related

- [[okf-spec-v0-1]] · [OKF v0.1 — normative specification](okf-spec-v0-1.md) — the superseded base revision whose structural core carries forward
- [[open-knowledge-format]] · [Open Knowledge Format (OKF)](open-knowledge-format.md) — the motivation, lineage, and Atlas alignment narrative
- [[llm-vault-structure-spec]] · [LLM Vault Structure Spec](llm-vault-structure-spec.md) — Atlas's own architecture spec that OKF conformance layers onto

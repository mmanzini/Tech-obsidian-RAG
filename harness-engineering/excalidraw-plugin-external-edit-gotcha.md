---
type: synthesis
title: Obsidian Excalidraw plugin — external-edit merge gotcha
description: The Obsidian Excalidraw plugin merges its in-memory cached scene over external file edits: rewriting an open `.excalidraw.md` file from a script or agent produces a corrupted union of old and new drawings on the plugin's next auto-save (source: 2026-06-11-excalidraw-plugin-merge-gotcha.md).
bucket: ai-engineering
topic: harness-engineering
tags: []
source: auto-capture, 2026-06-11 (programmatic diagram generation into Atlas
resource:
timestamp: 2026-06-11T14:50:18Z
status: active
related:
  - ai-engineering/harness-engineering/headless-skill-execution-contracts.md
---

# Obsidian Excalidraw plugin — external-edit merge gotcha

**Source:** auto-capture, 2026-06-11 (programmatic diagram generation into Atlas)
**Author:** Max (discovered behaviour)

---

## Summary

The Obsidian Excalidraw plugin merges its in-memory cached scene over external file edits: rewriting an open `.excalidraw.md` file from a script or agent produces a corrupted union of old and new drawings on the plugin's next auto-save (source: 2026-06-11-excalidraw-plugin-merge-gotcha.md).

## Behaviour

If a `.excalidraw.md` file is open in an Obsidian Excalidraw tab while the file is rewritten externally, the plugin does not reload — it merges its cached scene with the new content on auto-save, and can additionally re-save the file in `compressed-json` format (source: 2026-06-11-excalidraw-plugin-merge-gotcha.md).

## Workflow rule

1. Close the Excalidraw tab before any external regeneration of a diagram file.
2. Verify the file a few seconds after writing.
3. Prefer writing redesigns to a **new filename** rather than rewriting one the plugin may have open.

(source: 2026-06-11-excalidraw-plugin-merge-gotcha.md)

## Key Takeaways

- Agents generating Excalidraw diagrams must treat open tabs as writers, not readers.
- New-filename-per-redesign is the safe default.

## Related

- [[headless-skill-execution-contracts]] — companion workflow rules for agents operating Atlas surfaces unattended

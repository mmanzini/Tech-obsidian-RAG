---
type: synthesis
title: Atlas Sync Architecture — Vault ↔ Public Repos via Unison
description: Atlas is a private Obsidian vault whose content partially overlaps with five public GitHub repositories.
bundle: ai-engineering
topic: knowledge-engineering
tags:
- knowledge-management
- harness-engineering
- agent-workflows
- unison
sources:
- id: sync-architecture
  resource: ../../../Resources/context/sync-architecture.md
generated:
  by: claude-code/atlas-consolidate
  at: '2026-05-28T11:25:51Z'
status: stable
related:
- ai-engineering/claude-code-practice/claude-code-agentic-os.md
- ai-engineering/claude-code-practice/claude-managed-agents-memory.md
- ai-engineering/claude-code-practice/claude-code-routines.md
- ai-engineering/knowledge-engineering/atlas-sync-operations.md
- ai-engineering/harness-engineering/atlas-codebase-intelligence-layer.md
---

# Atlas Sync Architecture — Vault ↔ Public Repos via Unison

**Source:** [sync-architecture.md](../../../Resources/context/sync-architecture.md)
**Type:** Architecture reference

---

## Summary

Atlas is a private Obsidian vault whose content partially overlaps with five public GitHub repositories. Because git submodules break the nightly cloud-consolidate routine (which fresh-clones Atlas and expects real files), the architecture uses a "two physical locations bridged by Unison" pattern: each public repo lives once with `.git` in `~/Documents/repos/<name>/` and once as a plain folder inside Atlas. Unison handles bidirectional sync between the two copies, detecting conflicts rather than silently resolving them.

## The Problem

Five Atlas folders need to exist simultaneously as separate, public-facing GitHub repositories with independent commit histories (source: sync-architecture.md):

| Atlas path | Public repo |
|---|---|
| `Intelligence/tech-research/` | `mmanzini/Tech-obsidian-RAG` |
| `Intelligence/political-economy/` | `mmanzini/Political-economy-obsidian-RAG` |
| `Resources/github-trends/` | `mmanzini/github-trending-digest` |
| `Skills/` | `mmanzini/Skills` |
| `Resources/projects/agentic-knowledge-engine/` | `mmanzini/agentic-knowledge-engine` |

Git submodules don't work here: the nightly cloud routine fresh-clones Atlas from GitHub on a clean runner and expects real files at those paths. A submodule pointer would give it nothing (source: sync-architecture.md).

## The Two-Location Pattern

The architecture satisfies three constraints simultaneously: each public repo keeps its own git history and remote; Atlas's outer git tracks the actual content (not just a pointer) of those folders; and sync is bidirectional with conflict awareness (source: sync-architecture.md).

The result: the same files exist at two physical locations on disk.

```
~/Documents/
├── Atlas/                            # private vault, single git repo
│   ├── Intelligence/
│   │   ├── tech-research/            # plain folder, no .git
│   │   └── political-economy/        # plain folder, no .git
│   ├── Resources/
│   │   ├── github-trends/            # plain folder, no .git
│   │   └── projects/agentic-knowledge-engine/
│   └── Skills/                       # plain folder, no .git
│
└── repos/                            # publishable clones
    ├── tech-research/                # has .git, points to GitHub remote
    ├── political-economy/
    ├── github-trending-digest/
    ├── skills/
    ├── agentic-knowledge-engine/
    └── scripts/
        ├── sync-repo.sh
        └── sync-all.sh
```

## Why Unison

Three approaches were considered (source: sync-architecture.md):

1. **Hardcoded direction per repo with rsync** — simple but loses data if the wrong side is edited accidentally.
2. **Auto-detect direction via `git log -1 --format=%ct`, then one-way rsync** — survives cross-direction edits but loses data on concurrent edits and doesn't propagate deletions cleanly.
3. **Unison** — off-the-shelf bidirectional file synchroniser with conflict detection and deletion tracking via archive state files.

Unison was chosen because it propagates non-conflicting changes in either direction and **skips files changed on both sides** rather than overwriting the older version. It uses content fingerprints (`fastcheck = true`) rather than just mtimes, so a freshly-pulled file that didn't actually change isn't flagged as updated (source: sync-architecture.md).

Accepted limitation: Unison is not git-aware. A series of small commits on the public repo is collapsed into "these files changed" by the time Unison runs. The wrapper script compensates by making one timestamp-stamped commit per sync for any inbound changes (source: sync-architecture.md).

## Data Flow Direction

Direction varies per repo (source: sync-architecture.md):

- **Outbound (Atlas → repos):** `tech-research`, `political-economy`, `agentic-knowledge-engine` — authored inside Atlas via the `consolidate` verb, published outward.
- **Inbound (repos → Atlas):** `github-trending-digest` — generated nightly by a GitHub Action on the public repo, pulled into Atlas as source material for the `consolidate` routine.
- **Mostly-inbound mirror:** `skills` — authored in the public repo (`~/Documents/repos/skills/`), mirrored into `Atlas/Skills/` for vault-wide visibility only. `Atlas/Skills/` is not the same as `~/.claude/skills/`, the runtime location Claude Code reads.

The bidirectional sync is a safety net for all repos: accidental edits on the non-authoritative side are pulled back on the next run rather than silently lost (source: sync-architecture.md).

## The Bridge: `sync-repo.sh` and `sync-all.sh`

`sync-repo.sh <name>` does five things in sequence (source: sync-architecture.md):

1. `git pull --ff-only` inside `~/Documents/repos/<name>/` — gets any new commits from GitHub (especially `github-trending-digest`, where the GitHub Action commits new content).
2. `git pull --ff-only` inside `~/Documents/Atlas/` — gets commits Obsidian Git may not have pulled yet.
3. `unison <name>` — runs the bidirectional sync using the named profile. `batch = true` and `auto = true` make it non-interactive; conflicts are skipped and logged.
4. If the publishable clone now has changes: `git add -A`, commit with a timestamp message, `git push`.
5. Atlas's outer git is left to Obsidian Git, which auto-commits on its own cadence.

`sync-all.sh` loops all configured repos and reports a summary at the end. It continues past per-repo failures so a transient issue on one repo doesn't block the others. The configured-repos list lives at the top of `sync-all.sh`; to enable a new repo, uncomment its name there and add its Unison profile and a `case` branch in `sync-repo.sh` (source: sync-architecture.md).

## Schedule

A launchd agent at `~/Library/LaunchAgents/com.manzini.atlas-sync.plist` runs `sync-all.sh` at **08:00 and 20:00 local time** every day (source: sync-architecture.md):

- **08:00** — catches the GitHub Action's overnight commits (especially `github-trending-digest`) and propagates them into Atlas before the user starts work.
- **20:00** — after the day's manual edits in Atlas have settled; far enough before the nightly cloud-consolidate routine that outbound content lands in the public repos in time.

The agent only fires when the Mac is awake; if the laptop is asleep, the job runs on wake. `RunAtLoad = false` — re-loading the agent at login does not trigger an immediate sync (source: sync-architecture.md).

Logs:
- Per-repo events: `~/Documents/repos/.sync.log`
- launchd stdout/stderr: `~/Documents/repos/.launchd.out.log` / `.launchd.err.log`

## Conflict Handling

Unison detects a conflict when a file has changed on both sides since the last sync. The profile sets `batch = true`, `auto = true`, and no `prefer` directive — so conflicts cause Unison to skip the file and log it. Both versions remain intact until the user resolves it manually (source: sync-architecture.md).

Three resolution paths:

1. **Force one side as winner:** `unison <repo> -prefer /path/to/<root>`
2. **Hand-merge:** open both files, reconcile, save on whichever side, run `sync-repo.sh` again.
3. **Restore from backup:** `~/.unison/backups/<repo>/` holds the last 3 versions per file.

## Per-Repo Configuration

Each repo has three artefacts (source: sync-architecture.md):

- A `case` branch in `sync-repo.sh` defining the Atlas-side folder path.
- A Unison profile at `~/.unison/<name>.prf` with both roots and standard ignore rules (`.git`, `.DS_Store`, `.gitignore`, `.gitattributes`, `README.md`).
- A backup directory at `~/.unison/backups/<name>/` (auto-created if missing).

The profiles share a common skeleton:

```
root = <atlas-side path>
root = <repos-side path>

ignore = Path .git
ignore = Name .DS_Store
ignore = Name .gitignore
ignore = Name .gitattributes
ignore = Path README.md

batch = true
auto = true
fastcheck = true

backup = Name *
backupcurr = Name *
backuploc = central
backupdir = /Users/massimilianomanzini/.unison/backups/<name>
maxbackups = 3

log = true
logfile = /Users/massimilianomanzini/Documents/repos/.sync.log
```

README is excluded from sync because the public repos use a generic README that doesn't need regeneration on every content change, and inside Atlas, `_master-index.md` plays that role (source: sync-architecture.md).

## Failure Modes

| Failure | Detection | Recovery |
|---|---|---|
| Network down, `git pull` fails | Wrapper logs, exits non-zero | Next run retries automatically |
| Unison conflict | Logged, file skipped | Manual resolve via `-prefer` or hand-merge |
| First-run archive missing | Unison warns, treats both sides as empty | Auto-resolves; subsequent runs use the seeded archive |
| Backup dir missing | Wrapper auto-creates | Self-healing |
| Atlas `git pull` fails (divergent history) | Wrapper logs warning, continues | Manual git inspection |
| Public repo `git pull` fails (force-push upstream) | Wrapper exits | Manual `git fetch && git reset --hard origin/main` |
| Mac asleep at scheduled time | launchd defers | Job runs on wake |
| Obsidian Git mid-commit during Atlas `git pull` | Race; worst case stale read | Next sync picks up missed change |

## State Files

| What | Path |
|---|---|
| Unison archive | `~/.unison/ar*` (binary; remove only as last resort) |
| Backups | `~/.unison/backups/<repo>/` |
| Wrapper log | `~/Documents/repos/.sync.log` |
| launchd stdout | `~/Documents/repos/.launchd.out.log` |
| launchd stderr | `~/Documents/repos/.launchd.err.log` |
| LaunchAgent plist | `~/Library/LaunchAgents/com.manzini.atlas-sync.plist` |

## Onboarding a New Repo

1. Clone into `~/Documents/repos/<name>/`.
2. Add a `case` branch in `sync-repo.sh` for the Atlas-side path.
3. Create the Atlas-side folder; if both sides have pre-existing content, do a one-time merge before letting Unison take over.
4. Write `~/.unison/<name>.prf` (copy an existing profile, change the roots).
5. Run `sync-repo.sh <name>` once to seed Unison's archive.
6. Add the name to `REPOS=()` in `sync-all.sh`.
7. Wait for the next scheduled run or trigger one manually.

Full step-by-step procedure: `Resources/prompts/sync-operations.md` (source: sync-architecture.md).

## Key Takeaways

- The "two physical locations" pattern is the core architectural decision; it exists because git submodules break the cloud-consolidate routine.
- Unison provides bidirectional sync with conflict detection — neither side auto-wins.
- Data flow is not uniform: two repos are outbound, one inbound, one a mirror.
- `sync-repo.sh` wraps git + Unison + git into one atomic operation per repo; `sync-all.sh` loops all repos and continues past failures.
- The schedule (08:00 / 20:00) is designed around the GitHub Action's overnight generation cycle and the nightly cloud-consolidate routine.

## Related

- [[claude-code-agentic-os|Claude Code Agentic OS]] — the broader agentic OS pattern that Atlas's nightly cloud routine implements
- [[claude-managed-agents-memory|Built-in Memory for Claude Managed Agents]] — cross-session memory patterns that complement the sync architecture
- [[claude-code-routines|Claude Code Routines]] — scheduled cloud-based routines that consume the synced content
- [[atlas-sync-operations|Atlas Sync — Operations Reference]] — the operational commands and schedule for running the sync
- [[../harness-engineering/atlas-codebase-intelligence-layer|Atlas Model Applied to Codebase Intelligence]] — extending this sync architecture to serve codebase intelligence via MCP

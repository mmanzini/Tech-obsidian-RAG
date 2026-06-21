---
type: synthesis
title: Atlas Sync Operations — Practical Command Reference
description: Operational companion to [[atlas-sync-architecture|Atlas Sync Architecture]], providing the practical commands, procedures, and troubleshooting playbook for running, debugging, and extending the Atlas ↔ public-repos sync.
bucket: ai-engineering
topic: knowledge-engineering
tags: []
source: ../../../Resources/documents/sync-operations.md
resource:
timestamp: 2026-05-28T11:25:51Z
status: active
related:
  - ai-engineering/knowledge-engineering/atlas-sync-architecture.md
  - ai-engineering/claude-code-practice/claude-code-routines.md
---

# Atlas Sync Operations — Practical Command Reference

**Source:** [sync-operations.md](../../../Resources/documents/sync-operations.md)

---

## Summary

Operational companion to [[atlas-sync-architecture|Atlas Sync Architecture]], providing the practical commands, procedures, and troubleshooting playbook for running, debugging, and extending the Atlas ↔ public-repos sync. Covers manual sync, schedule management, log inspection, conflict resolution, onboarding a new repo, README handling, and common scenarios. Architecture rationale lives in the architecture article; this article is the ops runbook.

## Quick Reference

| Task | Command |
|------|---------|
| Sync everything now | `~/Documents/repos/scripts/sync-all.sh` |
| Sync one repo | `~/Documents/repos/scripts/sync-repo.sh <name>` |
| Tail the live log | `tail -f ~/Documents/repos/.sync.log` |
| Check schedule status | `launchctl print gui/$(id -u)/com.manzini.atlas-sync` |
| Pause the schedule | `launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.manzini.atlas-sync.plist` |
| Resume the schedule | `launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.manzini.atlas-sync.plist` |
| Show last 5 backup versions | `ls -lt ~/.unison/backups/<repo>/` |

(source: sync-operations.md)

## Manual Sync

Run all configured repos once immediately (source: sync-operations.md):

```bash
~/Documents/repos/scripts/sync-all.sh
```

`sync-all.sh` loops through every repo in its `REPOS=()` array, runs `sync-repo.sh` for each, and prints a one-line summary. Per-repo failures don't abort the whole run.

Run a single repo:

```bash
~/Documents/repos/scripts/sync-repo.sh tech-research
~/Documents/repos/scripts/sync-repo.sh skills
~/Documents/repos/scripts/sync-repo.sh political-economy
~/Documents/repos/scripts/sync-repo.sh github-trending-digest
~/Documents/repos/scripts/sync-repo.sh agentic-knowledge-engine
```

## Schedule

A launchd agent runs `sync-all.sh` at **08:00 and 20:00 local time** (source: sync-operations.md):

- **08:00** — picks up overnight cloud-generated content from public repos (especially `github-trending-digest`).
- **20:00** — propagates the day's Atlas-side changes outward.

If the Mac is asleep, the job runs on wake. Sleeping past a sync is harmless.

Verify the agent is loaded: `launchctl list | grep atlas-sync` — expected output has a line with `com.manzini.atlas-sync`.

## Managing the Schedule

Pause: `launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.manzini.atlas-sync.plist`

Resume: `launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.manzini.atlas-sync.plist`

To change firing times, edit `~/Library/LaunchAgents/com.manzini.atlas-sync.plist` (`StartCalendarInterval` block), then bootout + bootstrap to reload (launchd does not pick up plist edits automatically) (source: sync-operations.md).

## Log Inspection

```bash
tail -f ~/Documents/repos/.sync.log
```

Log patterns to look for (source: sync-operations.md):
- `[<repo>] begin` ... `[<repo>] done` — clean run.
- `[<repo>] unison: clean` — nothing to do or changes applied cleanly.
- `[<repo>] unison: some files skipped (likely conflicts)` — at least one file changed on both sides; Unison left both alone.
- `[<repo>] FAIL: ...` — wrapper-level failure. Wrapper exits non-zero; `sync-all.sh` notes it and moves on.

For launchd failures: `tail ~/Documents/repos/.launchd.err.log`

## Resolving Conflicts

Unison skips a file (not overwrite) when it changed on both sides since last sync. To find conflicts: `grep -i "conflict\|skipped" ~/Documents/repos/.sync.log` (source: sync-operations.md).

**Three resolution paths:**

1. **Pick a winner via `-prefer`:**

```bash
unison <repo-name> -prefer /Users/massimilianomanzini/Documents/Atlas/<atlas-path>
# or prefer repos side:
unison <repo-name> -prefer /Users/massimilianomanzini/Documents/repos/<repo-name>
```

Then run `sync-repo.sh <repo-name>` to commit + push.

2. **Hand-merge:** open both versions, reconcile, save on either side, delete the other copy, run `sync-repo.sh`.

3. **Restore from backup:** `ls -lt ~/.unison/backups/<repo-name>/` — last 3 versions of every file. Copy back to original location and re-run sync.

(source: sync-operations.md)

## Onboarding a New Repo

Seven steps (source: sync-operations.md):

1. **Clone:** `git clone <url> ~/Documents/repos/<name>/`
2. **Add case branch in `sync-repo.sh`** — set `ATLAS_PATH` to the right Atlas folder.
3. **Handle pre-existing content** — if both sides have content, do a one-time manual merge before letting Unison take over. Run `rsync -avn ...` to preview what would change.
4. **Create Unison profile:** copy an existing `.prf`, edit both `root` lines and `backupdir`.
5. **Run first sync:** `sync-repo.sh <name>` — expect a "no archive files found" warning (normal, first run only).
6. **Enable in `sync-all.sh`:** add `<name>` to the `REPOS=()` array.
7. **Verify:** run `sync-all.sh` and confirm both repos appear in the log with `done`.

Atlas-side path conventions: wiki buckets → `$ATLAS/Intelligence/<name>`; source folders → `$ATLAS/Resources/<name>`; top-level mirrors → `$ATLAS/<Name>`.

## README Handling

Public repos get a generic README that doesn't reference the inner folder structure. READMEs are **excluded from Unison sync** (`ignore = Path README.md` in each profile) so they stay repos-only. To update a public repo's README: edit it directly in `~/Documents/repos/<name>/README.md` and push manually — the next sync run won't touch it (source: sync-operations.md).

## Common Scenarios

**"I edited a file in Atlas; when does it land on GitHub?"** — Next scheduled run (08:00 or 20:00). Worst case ~12 hours. Force immediate propagation: `sync-repo.sh <repo>` (source: sync-operations.md).

**"I made a typo fix on the public repo via GitHub's web editor."** — Next scheduled run pulls it via `git pull`, Unison propagates back into Atlas, Obsidian Git auto-commits Atlas. Total round trip ~12 hours via schedule (source: sync-operations.md).

**"I want to disable sync for one repo without removing it."** — Comment its name out in `REPOS=()` in `sync-all.sh`. Manual `sync-repo.sh <name>` still works (source: sync-operations.md).

**"Unison is reporting a conflict on a file I don't care about."** — Use `-prefer` to pick a winner, or add the path to the profile's `ignore` rules.

**"The schedule didn't fire when I expected it to."** — Check if the Mac was awake. `launchctl print gui/$(id -u)/com.manzini.atlas-sync | head -50` shows last run time (source: sync-operations.md).

**"I want to see what Unison would do without doing it."** — No clean dry-run for `batch = true` profiles. Workaround: `rsync -avn --delete --exclude='.git/' --exclude='.DS_Store' <atlas-path>/ <repo-path>/` for a file-level preview (source: sync-operations.md).

## Removing a Repo

1. Comment or remove its name from `REPOS=()`.
2. (Optional) Remove its Unison profile, case branch, archive, backups, publishable clone, and Atlas-side folder.

The optional steps are independent — you can disable sync by editing `sync-all.sh` alone without removing any files (source: sync-operations.md).

## Key Takeaways

- `sync-repo.sh <name>` and `sync-all.sh` are the two operational levers; both are in `~/Documents/repos/scripts/`.
- Conflicts cause Unison to skip (not overwrite); resolve with `-prefer`, hand-merge, or backup restore.
- The "no archive files found" warning on first run is normal and self-healing.
- To change the schedule, edit the plist and bootout/bootstrap — edits are not picked up automatically.
- README files are excluded from sync by design; update them manually in the repos-side clone.

## Related

- [[atlas-sync-architecture|Atlas Sync Architecture]] — the architecture and design rationale for this setup
- [[claude-code-routines|Claude Code Routines]] — scheduled cloud routines that consume content synced into Atlas

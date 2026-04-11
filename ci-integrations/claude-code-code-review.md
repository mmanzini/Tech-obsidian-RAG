# Claude Code — Code Review

**Source:** [code.claude.com/docs/en/code-review](https://code.claude.com/docs/en/code-review) (Anthropic docs, 2026-04-11)

Managed GitHub PR review service (research preview, Team/Enterprise only, no ZDR). A fleet of specialized agents analyzes the diff against the full codebase in parallel, a verification step filters false positives, and findings post as inline comments tagged by severity. Never blocks merges — the check always completes with a neutral conclusion.

## How reviews run

- **Triggers:** on PR open, on every push, on manual `@claude review`, or a mix — configured per repo.
- **Mechanics:** parallel specialized agents → verification pass → dedup → severity ranking → inline comments + annotations + check-run output.
- **Time:** ~20 minutes average. Cost scales with PR size and complexity.
- **Infrastructure:** runs on Anthropic infra. For self-hosted CI, use [[ci-integrations/claude-code-github-actions|GitHub Actions]] or GitLab CI/CD.

## Severity levels

| Marker | Severity | Meaning |
|---|---|---|
| 🔴 | Important | Bug that should be fixed before merging |
| 🟡 | Nit | Minor issue, worth fixing but not blocking |
| 🟣 | Pre-existing | Bug in the codebase, not introduced by this PR |

Each finding includes a collapsible extended-reasoning section explaining why it was flagged and how Claude verified it.

## Manual triggers

| Command | Effect |
|---|---|
| `@claude review` | Starts a review **and** subscribes the PR to push-triggered reviews going forward |
| `@claude review once` | One-shot review, no future subscription |

Rules:
- Post as a top-level PR comment (not an inline diff comment).
- Command must be at the start of the comment.
- Requires owner/member/collaborator access.
- PR must be open; manual triggers work on drafts (unlike automatic triggers).
- Queues if a review is already running.

## Customization: CLAUDE.md vs REVIEW.md

Two additive layers on top of default correctness checks:

- **`CLAUDE.md`** — shared project instructions used across all Claude Code tasks. Newly-introduced violations become nit-level findings. Works bidirectionally: if a PR makes a `CLAUDE.md` statement outdated, Claude flags that the docs need updating. Hierarchical — rules in a subdir `CLAUDE.md` apply only under that path.
- **`REVIEW.md`** (repo root) — review-only rules, auto-discovered. Use for:
  - Team style: "prefer early returns over nested conditionals"
  - Always-flag rules: "any new API route must have an integration test"
  - Skip rules: "don't comment on formatting in `src/gen/`"

Rule of thumb: if guidance also applies to interactive sessions, put it in `CLAUDE.md`; if it's only about what to flag in review, put it in `REVIEW.md`.

## Rating & feedback loop

- Every inline comment ships with 👍/👎 attached so both buttons appear in GitHub's UI.
- Anthropic collects reaction counts **after the PR merges** to tune the reviewer.
- Reactions do not trigger re-reviews. Replying to inline comments does nothing — fix the code and push.

## Check run output for gating

Though the managed check is always neutral, the **Claude Code Review** check run exposes a machine-readable severity summary. Parse it in your own CI to gate merges:

```bash
gh api repos/OWNER/REPO/check-runs/CHECK_RUN_ID \
  --jq '.output.text | split("bughunter-severity: ")[1] | split(" -->")[0] | fromjson'
# → {"normal": 2, "nit": 1, "pre_existing": 0}
```

`normal` = count of Important findings. Non-zero means at least one bug to fix before merge.

## Findings locations (when inline comments don't show)

If the check run says issues exist but inline comments are missing:

- **Check run Details** — full severity table with file:line regardless of inline-comment acceptance.
- **Files changed tab** — annotations attached to diff lines (red/yellow/gray markers).
- **Review body** — "Additional findings" section for findings on lines that no longer exist after push-during-review.

## Pricing & spend controls

- Billed as extra usage ($15-25 per review average), separate from plan inclusions.
- "After every push" mode multiplies cost per push — `@claude review` (not `once`) also subscribes to this.
- Set monthly spend cap at `claude.ai/admin-settings/usage`.
- Per-repo average cost visible in admin settings; weekly spend chart in `claude.ai/analytics/code-review`.

## Failure modes

- Failed/timed-out runs never retry automatically and never block merge.
- Recover by commenting `@claude review once` — GitHub's **Re-run** button does NOT retrigger Code Review.
- Failed runs still report a neutral check conclusion, so PRs remain mergeable.

## Key Takeaways

- Multi-agent parallel analysis with verification step; findings tagged 🔴/🟡/🟣, never blocks merge by default.
- Two config files — `CLAUDE.md` for general standards (also used by other Claude tools), `REVIEW.md` for review-only rules.
- Always-neutral check run, but severity counts are machine-readable via `gh api` + jq — use that to gate merges in your own CI.
- Cost lever: `@claude review` subscribes to push-triggered reviews; `@claude review once` is the cheap one-shot alternative.
- For self-hosted or custom workflows, fall back to [[ci-integrations/claude-code-github-actions|Claude Code GitHub Actions]].

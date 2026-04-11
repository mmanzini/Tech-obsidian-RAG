# Hooks for Deterministic CLI Enforcement — Don't Use CLAUDE.md

**Source:** [How to actually force Claude Code to use the right CLI (don't use CLAUDE.md)](https://www.youtube.com/watch?v=3CSi8QAoN-s) — Matt Pocock (YouTube)
**Companion article:** [aihero.dev — How to Use Claude Code Hooks to Enforce the Right CLI](https://www.aihero.dev/how-to-use-claude-code-hooks-to-enforce-the-right-cli)

---

## The question

How do you force Claude Code to adopt *your* workflow — `pnpm` instead of `npm`, a wrapper script instead of `npx`, a blocklist of destructive commands — instead of whatever the model defaults to?

## Why CLAUDE.md is the wrong answer

Most people's first instinct is to add the rule to `CLAUDE.md`. Pocock argues against it:

- **Globality.** `CLAUDE.md` is injected into *every* action. A rule about the package manager is irrelevant 95% of the time, but it's on every prompt.
- **Instruction budget.** LLMs only absorb roughly **~500 instructions** before getting confused. You want that budget spent on your plan and implementation, not "use pnpm, not npm".
- **Not deterministic.** Instructions like "never run `git push`" only *reduce the odds* of the bad command — they don't prevent it. Negative instructions are especially bad: you're burning budget on something you aren't actually blocking.

Pocock's previous video advocated **deleting CLAUDE.md** almost entirely, keeping it near-empty.

## The right answer — PreToolUse hooks

Claude Code **hooks** run deterministic code at lifecycle events. Relevant ones:

- **SessionStart** — run something when the session opens
- **Permission dialogue** — auto-approve/deny tool calls
- **PreToolUse** — run *before* a tool executes; can **block** the call

The enforcement pattern: on `PreToolUse` for any `Bash` call, inspect the command. If it starts with `npm`, exit with code `2` and an error message. Claude reads the error and self-corrects — usually by running the allowed command instead.

The hook itself is a small `.sh` file wired into `settings.json`:

```
hooks:
  PreToolUse:
    - matcher: Bash
    - command: ./block-npm.sh
```

And the script:

```sh
# pseudocode
if command starts with "npm":
    echo "Blocked: use pnpm, not npm."
    exit 2
```

## The meta-trick — let Claude write its own hooks

Rather than writing hooks by hand, Pocock prompts Claude Code to convert its own `CLAUDE.md` into hooks:

> Take the instructions in your CLAUDE.md file and turn them into deterministic Claude Code hooks in this project directory. Only do the ones that *can* be deterministic — like preferring one CLI over another, or disallowing certain commands. Use separate bash scripts per hook. First, confirm which hooks will be created. Second, implement them. Third, give me instructions to test them.

Claude identifies the enforceable rules (npm block, git push block, …), proposes them, writes the script, runs `chmod +x`, wires it into `settings.json`, and verifies it inline. Then you delete the now-redundant instructions from `CLAUDE.md`.

## The demo result

- Fresh session, `CLAUDE.md` cleared of the rule.
- User: *"npm install @tanstack/react-query"*
- Hook fires: **"Bash hook error: blocked. Use pnpm not npm."**
- Claude self-corrects: runs `pnpm install @tanstack/react-query`.

One command prevented, another steered to, with **zero bytes of instruction budget spent** at the global level.

## Why this approach is better

- **Deterministic.** Blocked means blocked. No probabilistic steering.
- **Frees the instruction budget.** The `CLAUDE.md` stays small so Claude focuses on planning and implementation.
- **Just-in-time context.** The rule *only* surfaces at the moment the forbidden command is attempted — via the error message. Claude progressively discovers the constraint instead of carrying it everywhere.
- **Generalises to other feedback loops.** Pocock applies the same idea to **ESLint rules** — turning personal style preferences (e.g. "no positional parameters") into lint errors rather than prompt instructions. "Lint Claude into the right thing."

## When *not* to use hooks

Not every instruction is deterministic. Things like "use encyclopedic tone" or "match the language already used in the file" can't be enforced by a bash script — they belong in `CLAUDE.md` or a skill. Only hook the rules that have a clear string/command-level check.

## Transferability

Hooks are not Claude-specific as a concept. The same pattern (PreToolUse-style interception + deterministic enforcement) can be applied to Codex, Cursor, or any agent runtime that exposes lifecycle events.

## Key Takeaways

- **CLAUDE.md is the wrong place for CLI/command enforcement.** It's global, burns instruction budget, and is non-deterministic.
- **PreToolUse hooks** intercept bash calls before execution and can block them deterministically via exit code 2.
- **Let Claude bootstrap its own hooks** by feeding it a prompt to convert `CLAUDE.md` rules into `.sh` files + `settings.json` entries.
- **Just-in-time context beats always-on instructions.** Hooks surface the rule at the moment it matters, not on every prompt.
- **Extend the idea** to ESLint, custom lints, typecheck configs — any deterministic feedback loop that steers Claude without instructing it.
- Rule of thumb: **if it can be a lint or a hook, it should be — not a prompt instruction.**

## See Also

- [[skill-issue-harness-engineering]] — hooks and back-pressure as core harness levers (silent on success, errors on failure)
- [[deep-modules-codebase-for-ai]] — same author, designing the codebase itself to be AI-friendly
- [[harnessing-claude-intelligence]] — Anthropic's "what can I stop doing?" pattern, same spirit of removing instructions
- [[subagents-in-claude-code]] — another tool for context isolation and determinism

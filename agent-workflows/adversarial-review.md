# Adversarial Review

**Source:** [BMad Method docs](https://docs.bmad-method.org/explanation/adversarial-review/)

---

## Summary

Adversarial Review is a forced-reasoning technique where the reviewer **must** find issues — no "looks good" allowed. The reviewer adopts a cynical stance and assumes problems exist. Zero findings triggers a halt: re-analyze or explain why. It prevents lazy rubber-stamp reviews by breaking confirmation bias.

## Why It Works

Normal reviews suffer from confirmation bias: skim, nothing jumps out, approve. The "must find problems" mandate breaks this:

- **Forces thoroughness** — can't approve until you've looked hard enough
- **Catches missing things** — "what's not here?" becomes a natural question
- **Improves signal quality** — findings are specific and actionable, not vague concerns
- **Information asymmetry** — runs with fresh context (no access to original reasoning), so the artifact is evaluated, not the intent

## Where It's Used

Throughout BMad workflows: code review, implementation readiness checks, spec validation. Sometimes required, sometimes optional. The pattern adapts to whatever artifact needs scrutiny.

## Human Filtering Required

Because the AI is *instructed* to find problems, it will find problems — even when they don't exist. Expect false positives: nitpicks dressed as issues, misunderstood intent, hallucinated concerns.

**You decide what's real.** Review each finding, dismiss noise, fix what matters.

## Example

Instead of:
> "The authentication implementation looks reasonable. Approved."

Adversarial review produces:
> 1. **HIGH** — `login.ts:47` — No rate limiting on failed attempts
> 2. **HIGH** — Session token stored in localStorage (XSS vulnerable)
> 3. **MEDIUM** — Password validation happens client-side only
> 4. **MEDIUM** — No audit logging for failed login attempts
> 5. **LOW** — Magic number `3600` should be `SESSION_TIMEOUT_SECONDS`

The first review might miss a security vulnerability. The second caught four.

## Iteration

After addressing findings, run it again. A second pass usually catches more. A third isn't always useless either. But each pass takes time, and eventually you hit diminishing returns — just nitpicks and false positives.

## Key Takeaways

- **The core rule:** the reviewer must find issues; zero findings is a halt
- **Cynical stance is intentional** — assume problems exist and find them
- **Run with fresh/no context** to evaluate the artifact, not the author's intent
- **Expect false positives** — human filtering is non-negotiable
- Parallel insight from Anthropic's [[harness-design-long-running-apps|GAN-inspired harness]]: separating generation from evaluation, and tuning the evaluator to be skeptical, is far easier than making a generator self-critical
- Used as a building block in [[quick-dev|Quick Dev]]'s review/triage phase

## See Also

- [[quick-dev|Quick Dev]] — uses adversarial review as its review subagent
- [[harness-design-long-running-apps|Harness Design for Long-Running App Development]] — Anthropic's evaluator agent works on the same separation principle
- [[research-plan-implement|Research Plan Implement (RPI)]] — verification gates serve a similar but less adversarial function
- [[ai-product-development/ai-product-development-framework|AI Product Development Framework]] — applies the Generator/Critic pattern one level up, to product decisions and prototyping (Adversarial Iteration component)

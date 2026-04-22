# Nicole Forsgren — DORA, SPACE, and Why Speed and Stability Move Together

**Source:** Lenny's Podcast — Co-author *Accelerate*, creator of DORA and SPACE

Forsgren built the DORA research program and later SPACE at Microsoft. Her counterintuitive finding, proven across thousands of orgs: elite performers are faster *and* more stable — the tradeoff most teams assume is a myth. The frameworks work because they pair quantitative throughput/stability with qualitative signals.

## DORA — Four Metrics That Correlate with Business Outcomes

- **Throughput:** deployment frequency, lead time for changes.
- **Stability:** change failure rate, time to restore (MTTR).
- Elite benchmarks: deploy on-demand; lead time <1 day; MTTR <1 hour; change failure 0-15%.
- "Speed and stability move together" — elite teams dominate on both; low performers lose on both.
- No statistically significant difference between SMB and enterprise performance (retail is the only outlier).
- Teams gaming a single metric (e.g. deploy count) get caught because the other metrics regress.

## SPACE — Why Metrics Alone Lie

- **S**atisfaction / well-being, **P**erformance, **A**ctivity, **C**ommunication, **E**fficiency / flow.
- Rule: pick metrics from **≥3 dimensions** to prevent gaming.
- Activity (commits, PRs) is the easiest to measure and the most misleading in isolation.
- Pair every quantitative metric with a qualitative one — e.g. deploy frequency + developer-reported flow.

## Words Before Data

- "Words → words, data → data" — Four-Box framework.
- Most teams lunge for dashboards; they should first ask people what's broken, then measure it.
- Qualitative interviews surface the hypothesis; telemetry only confirms or refutes it.
- Applies identically to AI-era productivity measurement — don't just count AI-accepted suggestions.

## The Decision-Making Spreadsheet

- When evaluating a new tool or practice, list: expected benefit, cost, evidence, risk, reversibility.
- Force quantification of each row, even with rough ranges.
- Output: a defensible written record — makes it harder for HiPPOs to override data.
- Gets used for tooling adoption, org redesigns, and AI-tool rollouts.

## AI and Productivity Measurement

- Same playbook: throughput + stability + well-being; do not reduce to "lines of AI code accepted."
- Acceptance rate alone is noise — 70% acceptance on trivial completions tells you nothing.
- Measure whether AI-assisted work still hits the stability and MTTR bar.
- Warn against vanity metrics tied to executive narratives ("% of code written by AI").

## The Company-Size Myth

- Common excuse: "our org is too big / too regulated to be elite."
- DORA data: elite performers exist at every size and in every industry.
- Retail is the one consistent underperformer — hypothesis: seasonal freeze windows + legacy POS.
- Regulatory constraints are solvable; most underperformance is cultural, not structural.

## Key Takeaways

- Speed and stability are correlated, not a tradeoff — elite teams win on both.
- Use ≥3 SPACE dimensions so teams can't game a single metric.
- Start with qualitative words before quantitative data (Four-Box).
- AI productivity needs DORA-style framing, not acceptance rate vanity metrics.
- Company size and industry are poor excuses — retail is the only real outlier.

## Related

- [[way-of-working/_index|Way of Working]]
- [[ai-organization/_index|AI & Organisation Design]]
- [[harness-engineering/_index|Harness Engineering]]
- [[lennys-podcast/_index|Lenny's Podcast]]

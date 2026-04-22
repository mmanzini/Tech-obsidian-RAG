# Hamel Husain & Shreya Shankar — Error Analysis Is the Whole Eval Game

**Source:** Lenny's Podcast — AI eval practitioners / course instructors

Hamel (ex-Airbnb, GitHub) and Shreya (Berkeley PhD) teach the most popular AI evals course. Their core claim: 90% of eval value comes from systematic error analysis, not benchmark scores. The workflow is qualitative social science applied to LLM traces.

## The Nurture Boss Workflow — Open Coding → Axial Coding

- Real-estate AI assistant case study: Nurture Boss went from unclear failures to shipping fixes in weeks.
- Step 1: **Sample ~100 traces** stratified across user segments and time.
- Step 2: **Open coding** — for each trace, write the first/most upstream error in free text. No taxonomy yet.
- Step 3: **Axial coding** — cluster open codes into a small taxonomy (5-15 categories).
- Step 4: Measure category frequencies → prioritize by volume × severity.
- Step 5: Ship fix, re-sample, recount.

## The Benevolent Dictator Rule

- **One person — a domain expert — owns the taxonomy and does the labelling.**
- Committees produce muddy categories; the BD keeps the taxonomy sharp.
- Must be someone who understands the product and the user, not a random annotator.
- Rotate annually if needed, but never split the role in parallel.

## Write the First/Most Upstream Error Only

- Traces often contain cascading failures; labelling them all creates phantom categories.
- Rule: write only the **first root-cause error** — the one that would have prevented the rest.
- Forces the labeller to reason about causality rather than symptoms.
- Hamel: "If you fix the first error, the downstream ones often vanish for free."

## Theoretical Saturation — When You Can Stop

- Sociology concept imported to evals: stop sampling when new traces stop producing new categories.
- Practical signal: 3-5 consecutive traces yield no new axial codes.
- Usually lands around 50-150 traces per product area.
- Don't trust a fixed sample size — let the data tell you.

## LLMs Cannot Do Open Coding for You

- LLM-as-judge works for **evaluating** against a taxonomy once it exists.
- LLMs **cannot generate** the taxonomy — they lack product context, user intent, and business stakes.
- Counterintuitive to teams racing to automate evals: the hardest, most valuable step stays human.
- Attempts to auto-cluster produce generic buckets ("hallucination", "tone") that hide the real bugs.

## Evals Are Product Research

- Framing shift: evals aren't QA, they're qualitative user research with machine output instead of interviews.
- Same discipline — grounded theory, inter-rater reliability, sampling — applied to traces.
- PMs and domain experts are the right owners, not ML engineers.
- The eval doc becomes the product requirements doc for the next iteration.

## Key Takeaways

- 100 stratified traces + open coding + axial coding → prioritised fix list.
- Always write the first/most upstream error, not every symptom.
- A single benevolent dictator owns the taxonomy; committees dilute it.
- Stop when new traces yield no new categories (theoretical saturation).
- LLMs can judge against a taxonomy but cannot create one — open coding is irreducibly human.
- Evals are qualitative research, not automated QA.

## Related

- [[harness-engineering/_index|Harness Engineering]]
- [[ai-product-development/_index|AI Product Development]]
- [[rpi-methodology/_index|RPI Methodology]]
- [[lennys-podcast/_index|Lenny's Podcast]]

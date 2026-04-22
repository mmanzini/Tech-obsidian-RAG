# Duolingo's Growth Reignition — The Story Behind 4.5× DAU

**Source:** Jorge Mazal (former CPO, Duolingo), guest post on [Lenny's Newsletter](https://www.lennysnewsletter.com/p/how-duolingo-reignited-user-growth) (2023-02-28)

How Duolingo went from single-digit DAU YoY to 4.5× DAU in four years — built on a retention-focused growth model, leaderboards, streaks, and disciplined push notifications.

## The starting problem

Late 2017: most-downloaded education app, hundreds of millions of users, but DAU growth decaying into single digits YoY. Investors anxious ahead of monetisation. New user acquisition was already 100% organic — no obvious lever. Team bet on **retention**.

## Phase 1 — Gamification (failed)

- Inspiration: *Gardenscapes* match-3. Ported the **moves counter** into Duolingo lessons (finite wrong-answers before restart).
- Result: flat. No change in retention or DAU. Team dissented, disbanded.
- Lesson: Gardenscapes moves work because each move is strategic. A Duolingo question is "know it or don't" — no strategy. The counter became a nuisance, not a mechanic.

## Phase 2 — Referrals (failed)

- Ported Uber's referral program, reward = 1 month Super Duolingo.
- Result: +3% new users. Noise.
- Lesson: Uber referrals work because rides are recurring pay-as-you-go; a free ride is a constant incentive. Duolingo's best/most-active users already *had* Super — the strategy excluded the exact cohort that would spread it.

### The meta-lesson — adapt when adopting

Before copying a feature, ask:

1. **Why** is this feature working in that product?
2. Will it translate to our context (user motivation, business model, lifecycle)?
3. What **adaptations** are needed?

## Phase 3 — Data and the growth model

Inspired by Zynga + MyFitnessPal, Mazal built a MECE bucket model: every user lives in exactly one bucket on any given day.

**Buckets (top four add up to DAU):**
- New users — first day ever
- Current users — active today + ≥1 of prior 6 days
- Reactivated users — first day after 7–29 day absence
- Resurrected users — first day after 30+ day absence
- At-risk WAU — inactive today, active in prior 6
- At-risk MAU — inactive in past 7, active in prior 23
- Dormant — 31+ days inactive

**Arrows = daily retention rates** (CURR/NURR/RURR/SURR) — the levers product teams pull.

### The sensitivity simulation

Moved each lever 2% per quarter for three years, held others constant. Finding:

- **CURR had 5× the DAU impact of the second-best metric.**
- Why: current users who stay active *return to the same bucket* — CURR compounds.
- CURR's impact on DAU was 6× its impact on MAU (huge asymmetry).

**Decision: CURR becomes the North Star metric. New team: Retention.** Massive mindset shift for a company used to optimising new-user retention first.

## Three CURR-moving vectors

### Leaderboards (the first breakthrough)
Ported from FarmVille 2, adapted:
- Key bet: **competitor closeness > social closeness** in mature products (verified at Zynga). Most users' friends aren't active anyway.
- Added **leagues** (Bronze → Silver → Gold) — progression + fresh competition each week.
- **Removed** the "extra tasks" requirement — Duolingo leaderboards trigger automatically from existing lessons. No added friction.
- Results: overall learning time +17%, highly-engaged learners (1 hr/day × 5 days/week) **tripled**. Traditional retention (D1, D7) lifted with stat-sig.

### Push notifications (the disciplined optimisation)
Rule from Groupon's cautionary tale: **protect the channel.** Aggressive email testing at Groupon permanently destroyed the channel via opt-outs.
- Team had wide latitude on timing, templates, images, copy, localisation, bandit algorithms.
- Could NOT increase notification quantity without CEO approval.
- Dozens of small/medium wins compounded into substantial YoY DAU gains.

### Streaks (optimisation of an existing feature)
APM discovered: hitting a **10-day streak** sharply reduced drop-off risk (partly causal, partly selection bias — worth investing in anyway).
- First win: **streak-saver notification** ("you're about to lose your streak").
- Followed by: calendar views, animations, streak freezes, streak rewards.
- Today one of Duolingo's strongest engagement mechanics. Streak length compounds motivation day-over-day — the exact psychology retention needs.

## Results

- CURR **+21%** → 40% reduction in daily churn of best users
- **DAU 4.5×** over four years
- Share of DAU with 7+ day streak **≈3×** — higher-quality base that refers, subscribes, retains
- IPO in 2021

## Growth beyond CURR

Healthy paranoia about CURR hitting a ceiling → parallel investments in social features (the old Acquisition Team pivoted here), course content acceleration, international expansion, influencer marketing, schools, light paid UA, viral TikTok.

## Key Takeaways

- **Retention compounds; acquisition doesn't.** CURR's recursive bucket is why it dominates DAU.
- **MECE bucket + rate model** turns "which metric matters" into sensitivity analysis — pick the lever with the biggest simulated impact, not the sexiest.
- **Adapt when adopting.** Three questions: why does it work there, will it translate, what to change.
- **Competitor-closeness beats social-closeness** in leaderboards for mature products.
- **Protect the channel.** Push/email A/B tests can cause permanent opt-out damage even if you stop the test.
- **Milestones drive motivation.** A streak's power increases with its length — the mechanic improves retention mathematically.
- Sometimes the highest-ROI work is optimising a dormant feature you already shipped (streaks) rather than building a new one.

## Related

- [[product-management/_index|Product Management]]
- [[product-management/product-strategy-cycle|Product Strategy Cycle]]
- [[product-management/strategy-and-discovery|Strategy and Discovery]]
- [[lennys-podcast/elena-verna|Elena Verna on Lenny's Podcast]] — flip to 95% innovation / 5% optimization (adjacent growth-lever debate)
- [[lennys-podcast/grant-lee|Grant Lee on Lenny's Podcast]] — first-30-seconds-is-the-product; activation-focused growth
- [[lennys-podcast/albert-cheng|Albert Cheng on Lenny's Podcast]] — 1000 experiments/year and loss-aversion (Grammarly growth)

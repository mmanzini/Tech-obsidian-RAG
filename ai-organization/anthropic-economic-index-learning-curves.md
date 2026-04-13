# Anthropic Economic Index: Learning Curves

Anthropic's third Economic Index report (March 2026) studies how Claude usage evolved between November 2025 and February 2026, with a special focus on learning curves — how experience with Claude correlates with success.

Source: Massenkoff et al., [Anthropic Economic Index report: Learning curves](https://www.anthropic.com/research/economic-index-march-2026-report) (March 2026)

## What Changed Since the Previous Report

### Usage Diversification
- Top 10 tasks on Claude.ai dropped from 24% → 19% of traffic (less concentrated)
- Coding tasks migrating from Claude.ai to 1P API (Claude Code splits work into smaller agentic API calls)
- Personal use rose 35% → 42%; coursework fell 19% → 12% (academic calendars + new casual users)
- Almost all tasks in the sample had appeared before — very few genuinely novel task types

### Task Value Declining on Claude.ai
- Average task value (measured as hourly wage of US workers who perform that task) dropped from $49.3 → $47.9
- Driven by increase in simple factual queries (sports, weather) and coding shift to API
- Consistent with standard adoption curve: early adopters favour high-value tasks, later adopters bring wider range
- Average education level of inputs declined from 12.2 → 11.9 years

### Collaboration Modes
- Augmentation (collaborative interaction) increased slightly in Claude.ai
- Automation decreased sharply in 1P API data
- More experienced users are *more collaborative*, not more automated — contradicting earlier hypothesis

### Geographic Patterns
- Within US: usage convergence continues (top 5 states: 30% → 24%) but pace slowed; equal per-capita usage estimated in 5–9 years
- Across countries: usage became *more* concentrated — top 20 countries went from 45% → 48% of per-capita usage

## Learning Curves (Main Finding)

### Model Selection Matches Task Complexity
- Users choose Opus (most capable) for higher-value tasks
- Claude.ai: Opus used +4pp more for coding, -7pp for tutoring
- API users show ~2x stronger model-switching behaviour
- Per $10 increase in task hourly wage: +1.5pp Opus share (Claude.ai), +2.8pp (API)

### Higher Tenure = Higher Success
- Users with 6+ months tenure have **10% higher success rate** in conversations
- This holds after controlling for task type, country, language, model choice, and use case (4pp effect with full controls)
- High-tenure users also:
  - Use Claude 7pp more for work (vs personal)
  - Bring tasks requiring ~1 more year of education per year of Claude usage
  - Use more diverse tasks (top 10 O*NET tasks = 20.7% vs 22.2% for new users)
  - Collaborate more (task iteration), delegate less (directive patterns)

### Possible Explanations
- **Learning-by-doing:** users genuinely get better at extracting value from AI through experience
- **Selection effects:** early adopters may be more technical
- **Survivorship bias:** only users who find Claude useful continue
- Controlled regressions rule out simple confounding (task type, country, etc.) but cannot fully isolate learning from cohort effects yet

## Economic Implications

- Experienced users may be simultaneously the most exposed to AI disruption and most aided by AI — a potential channel for skill-biased technological change
- If effective AI use requires complementary skills acquired through experience, early-adoption benefits may be self-reinforcing
- "Facility with these platforms may be a key determinant of success that appears to scale with experience"

## Emerging API Automation Patterns

Two workflow categories at least doubled in share since previous report:
- **Business sales & outreach:** lead qualification, data enrichment, cold-email drafting
- **Automated trading & market ops:** market monitoring, investment proposals, trader alerts

## Key Takeaways

- Claude usage is diversifying away from coding dominance on Claude.ai, while coding concentrates in the API via agentic workflows
- Experienced users are more successful, more collaborative, and tackle more complex tasks — evidence consistent with learning-by-doing
- Users actively match model capability to task complexity (Opus for harder work)
- The most advanced users iterate *with* Claude rather than delegating *to* Claude — collaboration over automation
- Early-adoption advantages may compound, potentially deepening labour market inequality

## Related

- [[anthropic-growth-takeaways|Anthropic Growth Takeaways]] — earlier economic analysis of Claude adoption
- [[from-hierarchy-to-intelligence|From Hierarchy to Intelligence]] — organisational implications of AI adoption
- [[ai-fluency-curriculum|AI Fluency Curriculum]] — building the skills that correlate with higher tenure success
- [[product-job-market-2026|State of the Product Job Market 2026]] — labour market demand data that contextualises the inequality findings

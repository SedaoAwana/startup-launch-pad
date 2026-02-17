---
name: financial-modeling
description: "Use when evaluating whether an application is financially viable, setting pricing, projecting revenue, estimating costs, or making build-vs-buy decisions. Activates after product-strategy defines the business model and before go-to-market finalizes pricing. Also use when the user asks 'can I afford this?' or 'will this make money?'"
---

# Financial Modeling

Turn product ideas into numbers. This skill ensures that every application launched through the framework has a clear financial picture — not a fantasy spreadsheet, but an honest assessment of costs, revenue potential, and break-even timeline.

## Who This Is For

Solo founders and small teams launching applications with AI agent support. This is NOT corporate finance. No DCF models, no Series A projections, no 50-tab Excel nightmares. This is practical financial thinking for people who need to know: "Does this business math work?"

Announce at start: "I'm using the financial-modeling skill to evaluate the financial viability."

## Prerequisites

Read `domain-config.yaml` — specifically:
- `business.model` — freemium, subscription, one-time, marketplace
- `business.pricing_tiers` — current pricing thinking
- `tech.infrastructure` — hosting and service costs
- `tech.ai` — AI model usage (cost per call matters)
- `constraints.budget` — what the founder can spend
- `integrations` — third-party services with costs

Read the output of product-strategy if it exists (`docs/product-strategy.md`).

## Process

```
digraph financial_modeling {
  "Read domain-config.yaml" [shape=doublecircle];
  "Phase 1: Cost Structure" [shape=box];
  "Phase 2: Revenue Model" [shape=box];
  "Phase 3: Unit Economics" [shape=box];
  "Phase 4: Break-Even Analysis" [shape=box];
  "Phase 5: Scenario Planning" [shape=box];
  "Generate Financial Brief" [shape=doublecircle];

  "Read domain-config.yaml" -> "Phase 1: Cost Structure";
  "Phase 1: Cost Structure" -> "Phase 2: Revenue Model";
  "Phase 2: Revenue Model" -> "Phase 3: Unit Economics";
  "Phase 3: Unit Economics" -> "Phase 4: Break-Even Analysis";
  "Phase 4: Break-Even Analysis" -> "Phase 5: Scenario Planning";
  "Phase 5: Scenario Planning" -> "Generate Financial Brief";
}
```

---

### Phase 1: Cost Structure

Map every cost the application will incur. Categorize as fixed or variable.

**Fixed Costs (Monthly — incurred regardless of usage):**

| Category | Item | Estimated Cost | Notes |
|----------|------|---------------|-------|
| Infrastructure | Hosting (Vercel, Railway, AWS) | $ | Check free tiers |
| Infrastructure | Database (managed PostgreSQL, etc.) | $ | |
| Infrastructure | Domain + SSL | $ | Annual, amortize monthly |
| Infrastructure | Monitoring (Sentry, etc.) | $ | Free tier available? |
| Services | Auth provider (Clerk, Auth0, etc.) | $ | Free tier limits |
| Services | Email service (Resend, SendGrid) | $ | |
| Services | Analytics (PostHog, Mixpanel) | $ | |
| Business | LLC/Corp filing (annual) | $ | Amortize monthly |

**Variable Costs (Per-user or per-action):**

| Category | Item | Cost Per Unit | Trigger |
|----------|------|--------------|---------|
| AI | API calls (Claude, GPT, etc.) | $ per call | Per user action |
| Storage | File/image storage (S3, Cloudflare R2) | $ per GB | Per upload |
| Bandwidth | CDN / data transfer | $ per GB | Per request |
| Third-party | External API calls (eBay, Stripe, etc.) | $ per call | Per transaction |
| Payments | Stripe/payment processing fees | 2.9% + $0.30 | Per transaction |

**AI Cost Calculation (Critical for AI-powered apps):**

This is where most AI apps get their math wrong. Calculate:
1. Average API calls per user session
2. Average tokens per call (input + output)
3. Cost per 1K tokens for your model
4. Multiply: `calls × tokens × cost_per_1K_token × sessions_per_month`

Example:
```
Card grading: 1 image analysis per grade
Claude Sonnet: ~$0.003 per image analysis call (estimated)
Free user: 5 grades/month = $0.015/month AI cost per free user
Pro user: 30 grades/month = $0.09/month AI cost per Pro user
```

Present cost structure to user. Ask: "Does this match your understanding? Any costs I'm missing?"

---

### Phase 2: Revenue Model

Based on domain-config `business.model`, build the revenue structure.

**For Freemium/Subscription:**

| Tier | Monthly Price | Annual Price | Key Limits |
|------|-------------|-------------|------------|
| Free | $0 | $0 | [usage caps] |
| Paid Tier 1 | $ | $ (discount?) | [limits] |
| Paid Tier 2 | $ | $ | [limits] |

**For Marketplace/Transaction:**

| Transaction Type | Fee Structure | Average Transaction |
|-----------------|--------------|-------------------|
| [type] | % or flat fee | $ |

**For One-Time Purchase:**

| Product | Price | Upsells |
|---------|-------|---------|
| [product] | $ | [what] |

**Revenue Assumptions (be conservative):**
- Month 1 users: [realistic number — usually 50-200 for a new app]
- Monthly growth rate: [10-20% is aggressive but achievable]
- Free-to-paid conversion: [2-5% is typical for freemium]
- Monthly churn: [5-10% for early-stage apps]
- Average Revenue Per User (ARPU): [calculated from tier mix]

Present revenue model. Challenge unrealistic assumptions. If the user says "10,000 users in month 1" — push back with market reality.

---

### Phase 3: Unit Economics

This is the most important phase. Unit economics tell you whether each customer is profitable.

**Key Metrics:**

**Customer Acquisition Cost (CAC):**
```
CAC = Total marketing/acquisition spend ÷ New customers acquired
```
For organic/content-driven launch: CAC can be near $0 initially.
For paid acquisition: Track ad spend carefully.

**Lifetime Value (LTV):**
```
LTV = ARPU × Average Customer Lifespan (months)
Average Lifespan = 1 ÷ Monthly Churn Rate
```
Example:
```
ARPU = $9.99/month
Churn = 8%/month
Lifespan = 1 ÷ 0.08 = 12.5 months
LTV = $9.99 × 12.5 = $124.88
```

**LTV:CAC Ratio:**
```
Healthy: LTV:CAC > 3:1
Dangerous: LTV:CAC < 1:1 (losing money on every customer)
```

**Gross Margin Per User:**
```
Gross Margin = Revenue per user - Variable costs per user
Gross Margin % = Gross Margin ÷ Revenue per user × 100
```
For software: Target 70%+ gross margin.
For AI-heavy apps: AI costs can push margin below 50% — flag this.

**The AI Margin Trap:**
If your AI cost per user is $2/month and your price is $9.99/month, your AI cost alone is 20% of revenue. Add hosting, payment processing, and other variable costs — margin shrinks fast. Calculate this explicitly.

Present unit economics. Flag any metric that's unhealthy.

---

### Phase 4: Break-Even Analysis

When does the app start covering its costs?

**Monthly Break-Even:**
```
Break-Even Users = Fixed Monthly Costs ÷ Gross Margin Per User
```
Example:
```
Fixed costs: $50/month (hosting, services)
Gross margin per paid user: $7.50/month
Break-even: 50 ÷ 7.50 = 7 paid users
```

**Time to Break-Even:**
Using the growth assumptions from Phase 2, project when you'll hit break-even user count.

```
Month 1: 100 free users, 3 paid (2-5% conversion) → Revenue: $29.97
Month 2: 120 free, 5 paid → Revenue: $49.95
Month 3: 145 free, 7 paid → Revenue: $69.93 ← Break-even
```

**Runway Calculation (if self-funded):**
```
Monthly Burn = Fixed Costs + (Variable Costs × Users) - Revenue
Runway = Available Budget ÷ Monthly Burn
```

Present break-even timeline. If break-even is beyond 12 months, flag this as a risk and discuss options: reduce costs, increase price, or validate demand before building.

---

### Phase 5: Scenario Planning

Build three scenarios to stress-test the model:

**Pessimistic (things go wrong):**
- 50% of projected users
- 2% conversion rate
- 12% monthly churn
- AI costs 50% higher than estimated

**Realistic (base case):**
- Projected users as estimated
- 3-5% conversion
- 8% churn
- Costs as estimated

**Optimistic (things go right):**
- 200% of projected users
- 7% conversion
- 5% churn
- Volume discounts reduce AI costs

For each scenario, calculate:
- Monthly revenue at Month 6 and Month 12
- Break-even timeline
- Cash required to reach break-even

**The Kill Criteria:**
Define upfront what would make you stop:
- "If we don't hit X paid users by month Y, we pivot or shut down"
- "If CAC exceeds $Z, we stop paid acquisition"
- "If churn exceeds X%, we pause growth and fix retention"

Present all three scenarios. The user should feel confident about the realistic case and prepared for the pessimistic case.

---

## Output

After all phases, produce:

1. **Updated `domain-config.yaml`** — refine `business.pricing_tiers` and `business.revenue_targets` based on analysis
2. **Financial Brief** — saved to `docs/financial-model.md`

### Financial Brief Format

```markdown
# [Product Name] — Financial Model

## Cost Structure
### Fixed Monthly Costs: $X
[table]
### Variable Costs Per User: $X
[table]
### AI Cost Per User Per Month: $X

## Revenue Model
[pricing tiers, conversion assumptions]

## Unit Economics
- ARPU: $X
- CAC: $X
- LTV: $X
- LTV:CAC Ratio: X:1
- Gross Margin: X%

## Break-Even
- Break-even point: X paid users
- Estimated timeline: Month X
- Cash required to reach break-even: $X

## Scenarios
| Metric | Pessimistic | Realistic | Optimistic |
|--------|------------|-----------|------------|
| Month 6 Revenue | $ | $ | $ |
| Month 12 Revenue | $ | $ | $ |
| Break-even Month | X | X | X |
| Cash Required | $ | $ | $ |

## Kill Criteria
[defined stop conditions]

## Recommendation
[ ] FINANCIALLY VIABLE — proceed to build
[ ] VIABLE WITH ADJUSTMENTS — [what needs to change]
[ ] NOT VIABLE — [why, and what would need to change]
```

Commit: `docs: add financial model for [product name]`

## Rules

- Be conservative. Founders are naturally optimistic — your job is to ground the numbers.
- AI costs are the #1 blind spot for AI-powered apps. Always calculate per-user AI cost explicitly.
- Do NOT build complex spreadsheet models. The goal is clarity, not precision. A simple model you actually use beats a complex one you ignore.
- Challenge pricing that doesn't cover variable costs. If each user costs you $3/month in AI and you're charging $5/month, say so.
- Free tiers must have limits that prevent abuse. Calculate what an abusive free user would cost you.
- If the user doesn't know their costs yet, help them estimate using current market rates. Flag estimates clearly.
- Round to reasonable precision. "$9.99/month" is fine. "$9.9923/month" is noise.
- This is a living document. Update it monthly once the app is live with real data.

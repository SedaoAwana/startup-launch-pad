---
name: go-to-market
description: "Use when planning or executing an application launch. Covers launch strategy, channel selection, pricing execution, pre-launch activities, launch day playbook, and post-launch momentum. Activates after product-strategy and financial-modeling have validated the product and business model."
---

# Go-To-Market

Turn a built product into a launched product with users. This skill bridges the gap between "the app works" and "people are using it." It's the execution layer for getting your application into the hands of real users.

## Who This Is For

Solo founders launching applications with limited budget and no marketing team. The strategies here prioritize organic growth, community engagement, and content-driven acquisition over paid advertising. When you have $0-100/month for marketing, every action needs to count.

Announce at start: "I'm using the go-to-market skill to plan the launch strategy."

## Prerequisites

These must exist before this skill activates:
- `domain-config.yaml` — product identity, users, business model, launch section
- `docs/product-strategy.md` — validated product vision and positioning
- `docs/financial-model.md` — pricing validated, unit economics understood

If any are missing, redirect to the appropriate skill first.

## Process

```
digraph gtm {
  "Read prerequisites" [shape=doublecircle];
  "Phase 1: Launch Strategy" [shape=box];
  "Phase 2: Channel Selection" [shape=box];
  "Phase 3: Pre-Launch Execution" [shape=box];
  "Phase 4: Launch Day Playbook" [shape=box];
  "Phase 5: Post-Launch Momentum" [shape=box];
  "Generate GTM Brief" [shape=doublecircle];

  "Read prerequisites" -> "Phase 1: Launch Strategy";
  "Phase 1: Launch Strategy" -> "Phase 2: Channel Selection";
  "Phase 2: Channel Selection" -> "Phase 3: Pre-Launch Execution";
  "Phase 3: Pre-Launch Execution" -> "Phase 4: Launch Day Playbook";
  "Phase 4: Launch Day Playbook" -> "Phase 5: Post-Launch Momentum";
  "Phase 5: Post-Launch Momentum" -> "Generate GTM Brief";
}
```

---

### Phase 1: Launch Strategy

Define the type of launch. Not every app needs a Product Hunt splash. Choose based on product stage and goals.

**Launch Types:**

| Type | Best For | Effort | Risk |
|------|---------|--------|------|
| **Soft Launch** | MVPs, unvalidated ideas | Low | Low — gather feedback quietly |
| **Community Launch** | Niche products with existing communities | Medium | Medium — need community trust |
| **Platform Launch** | Developer tools, open source | Medium | Medium — need quality README/docs |
| **Content Launch** | Products with a story to tell | High | Low — content has lasting value |
| **Event Launch** | Products with demo-worthy features | High | High — one shot to impress |

Ask the user: "Which launch type fits your product and comfort level?"

**Launch Goals (pick 2-3, not all):**
- Acquire first X users
- Validate willingness to pay
- Get user feedback on core feature
- Build waitlist for full release
- Establish presence in target community
- Generate social proof (testimonials, reviews)

Present launch type and goals. Get approval.

---

### Phase 2: Channel Selection

Every product has 2-3 channels that will drive 80% of initial users. Find those channels. Ignore the rest.

**Channel Evaluation Framework:**

For each potential channel, score 1-5 on:
1. **Audience fit** — Are your target users actually here?
2. **Cost** — Can you reach them for free or cheap?
3. **Speed** — How fast can you get traction?
4. **Sustainability** — Will this channel keep working after launch week?
5. **Your capability** — Can you actually execute this well?

**Common Channels for Solo Founders:**

| Channel | Best For | Cost | Speed | Sustainability |
|---------|---------|------|-------|---------------|
| Reddit (subreddits) | Niche products with passionate communities | Free | Fast | Medium — community memory is short |
| Twitter/X | Dev tools, AI products, tech audience | Free | Medium | High — builds over time |
| Hacker News | Developer tools, open source, technical products | Free | Fast (if it hits) | Low — spike then fade |
| Product Hunt | Consumer apps, SaaS, tools | Free | Fast | Low — one-day spike |
| YouTube | Products that demo well visually | Time | Slow | High — videos keep working |
| Discord/Slack communities | Niche B2B, gaming, specific hobbies | Free | Medium | High — relationships compound |
| SEO/Blog | Information products, tools people search for | Time | Slow (3-6 months) | Very high — compounding |
| Email/Newsletter | Products with educational angle | Low | Medium | High — owned audience |
| GitHub | Open source, developer tools | Free | Medium | High — stars compound |
| TikTok/Instagram Reels | Visual products, younger demographics | Free | Fast if viral | Medium — algorithmic |

**Selection Process:**
1. Score each relevant channel on the 5 criteria above
2. Pick the top 3 channels by total score
3. Designate 1 as PRIMARY (where you invest most effort)
4. The other 2 are SECONDARY (repurpose primary content)

Present channel recommendations with reasoning. Get approval.

---

### Phase 3: Pre-Launch Execution

Start 2-4 weeks before launch date. Each activity builds momentum.

**Week -4 to -3: Foundation**

- [ ] Landing page live with clear value proposition
  - Headline: What it does (not what it is)
  - Sub-headline: For whom and why it's different
  - Visual: Screenshot or demo GIF
  - CTA: Waitlist signup or "Get Early Access"
  - Social proof placeholder (if any beta testers)
- [ ] Email capture working (Resend, ConvertKit, or simple form)
- [ ] Social accounts created on primary channels
- [ ] GitHub repo public with quality README (if open source)

**Week -2 to -1: Warming**

- [ ] Engage authentically in target communities (DO NOT pitch yet)
  - Answer questions related to your product's domain
  - Share helpful content (not your product)
  - Build recognition before you need it
- [ ] Create 2-3 pieces of content for launch:
  - "How I built X" — technical/founder story
  - "The problem with Y" — problem-awareness content
  - Demo video or GIF walkthrough (60-90 seconds max)
- [ ] Recruit 5-10 beta testers from target community
  - Give them early access
  - Ask for honest feedback (not testimonials yet)
  - Fix critical issues they find
- [ ] Prepare launch day posts for each channel (drafts ready)

**Week -1: Final Prep**

- [ ] Beta tester feedback incorporated
- [ ] Landing page updated with any beta testimonials
- [ ] Launch posts finalized and scheduled
- [ ] Email to waitlist drafted
- [ ] All links tested (signup, login, core flows, payment if applicable)
- [ ] Monitoring/error tracking active
- [ ] Run through quality-assurance skill's pre-release checklist

Present pre-launch timeline. Adjust based on user's actual launch date.

---

### Phase 4: Launch Day Playbook

A checklist for launch day. Execute in order.

**Morning (Before Posts Go Live):**
- [ ] Verify production is stable — check error monitoring
- [ ] Verify signup/login flow works
- [ ] Verify payment flow works (if applicable) — do a test transaction
- [ ] Clear any test data from production
- [ ] Confirm all links in launch posts are correct

**Launch Window (Post During Peak Hours for Your Audience):**

Timing guidance:
- Reddit: 8-10 AM EST weekdays
- Hacker News: 8-10 AM EST weekdays
- Twitter/X: 9-11 AM EST or 1-3 PM EST
- Product Hunt: launches at 12:01 AM PT (prep the night before)
- LinkedIn: 8-10 AM EST Tuesday-Thursday

**Post Sequence:**
1. Primary channel post goes live first
2. Wait 15-30 minutes — respond to any immediate comments
3. Secondary channel posts go live
4. Email waitlist
5. Personal social media (LinkedIn, Twitter)

**During Launch Day:**
- [ ] Monitor and respond to EVERY comment within 1 hour
- [ ] Monitor error tracking — fix anything critical immediately
- [ ] Track signups in real-time (even a simple counter)
- [ ] Screenshot any positive feedback (social proof for later)
- [ ] Do NOT disappear after posting — engagement drives algorithms

**Launch Day Metrics to Track:**
- Unique visitors to landing page/app
- Signups (free + paid)
- Activation rate (signed up → completed core action)
- Any errors or broken flows

---

### Phase 5: Post-Launch Momentum

Launch day is a spike. The goal is to convert that spike into sustained growth.

**Week 1 Post-Launch:**
- [ ] Send thank-you email to all signups with quick-start guide
- [ ] Reach out personally to first 10-20 users for feedback
- [ ] Fix any bugs reported during launch
- [ ] Post a "Week 1 update" in communities where you launched
- [ ] Compile launch metrics

**Week 2-4 Post-Launch:**
- [ ] Implement highest-priority user feedback
- [ ] Start content cadence (1 post/week minimum on primary channel)
  - Share user stories or interesting use cases
  - Write about technical decisions or challenges
  - Create tutorials or how-to content
- [ ] Ask happy users for testimonials or reviews
- [ ] Identify power users — they're your future advocates

**Content Flywheel (Ongoing):**
The sustainable growth engine for solo founders is content. Each piece of content should:
1. Address a real question your users have
2. Demonstrate your product naturally (not as an ad)
3. Be findable via search (SEO basics: title, headings, keywords)
4. Be repurposable across channels (blog → Twitter thread → YouTube short)

**Growth Levers to Evaluate (Week 4+):**
- Referral mechanism: Can users invite others? Is there incentive?
- SEO: Are people searching for your solution? Can you rank?
- Integrations: Can you piggyback on another platform's distribution?
- Community: Is there a community forming around your product?
- Paid acquisition: Only consider after organic channels are working and unit economics are proven

---

## Output

After all phases, produce:

1. **Updated `domain-config.yaml`** — refine `launch` section with concrete dates, channels, and pre-launch tasks
2. **GTM Brief** — saved to `docs/go-to-market.md`

### GTM Brief Format

```markdown
# [Product Name] — Go-To-Market Plan

## Launch Strategy
- **Type:** [soft / community / platform / content / event]
- **Target Date:** [date]
- **Goals:**
  1. [goal with specific number]
  2. [goal with specific number]

## Channels
| Channel | Role | Target Metric |
|---------|------|--------------|
| [primary] | Primary | [metric] |
| [secondary 1] | Secondary | [metric] |
| [secondary 2] | Secondary | [metric] |

## Pre-Launch Timeline
| Week | Activities | Owner |
|------|-----------|-------|
| -4 to -3 | [activities] | [who] |
| -2 to -1 | [activities] | [who] |
| -1 | [activities] | [who] |

## Launch Day Playbook
[Condensed checklist with times]

## Post-Launch Plan
- Week 1: [priorities]
- Week 2-4: [priorities]
- Content cadence: [frequency and topics]

## Metrics to Track
| Metric | Launch Day Target | Month 1 Target |
|--------|------------------|----------------|
| Signups | X | X |
| Activation Rate | X% | X% |
| Paid Conversions | X | X |

## Budget
| Item | Cost | Timing |
|------|------|--------|
| [item] | $ | [when] |
| **Total** | **$** | |
```

Commit: `docs: add go-to-market plan for [product name]`

## Rules

- Be realistic about timeline. A solo founder cannot execute a 20-step pre-launch in 1 week. Spread it out.
- Community engagement CANNOT be faked. If the user hasn't been active in a community, they need 2-4 weeks of genuine participation before launching there. Suggest starting earlier.
- Never recommend paid acquisition before organic channels are proven and unit economics are validated.
- Launch is not a single day. It's a 6-week process: 4 weeks pre-launch, launch day, 2+ weeks post-launch.
- Respond to every single comment in the first 48 hours. Engagement signals boost visibility on every platform.
- Content > ads for solo founders. Content compounds. Ads stop when you stop paying.
- Do NOT recommend too many channels. 3 maximum. Focus beats breadth.
- If the user has no existing audience anywhere, the first priority is joining and participating in communities for 2-4 weeks before any launch activity.
- The best launch content tells a story: what problem you hit, why existing solutions failed, what you built, and what happened. People share stories, not product announcements.

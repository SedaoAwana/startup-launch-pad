---
name: product-strategy
description: "Use when defining or refining a product's vision, user personas, MVP scope, competitive positioning, or feature prioritization. Activates before brainstorming to establish business context. Also use when pivoting, re-scoping, or evaluating product-market fit."
---

# Product Strategy

Establish the business foundation before any code gets written. This skill ensures that technical decisions are driven by product thinking, not the other way around.

## When This Activates

- Before the brainstorming skill — to establish what we're building and why
- When the domain-config `problem` or `solution` sections are empty or vague
- When the user says "I have an idea for an app" or similar
- When pivoting or re-evaluating product direction
- When prioritizing features for a release

Announce at start: "I'm using the product-strategy skill to define the product foundation."

## Prerequisites

Read `domain-config.yaml` if it exists. If it doesn't exist, this skill will help create one.

## Process

```
digraph product_strategy {
  "Read domain-config.yaml" [shape=doublecircle];
  "Config exists?" [shape=diamond];
  "Phase 1: Problem Discovery" [shape=box];
  "Phase 2: User Definition" [shape=box];
  "Phase 3: Solution Framing" [shape=box];
  "Phase 4: MVP Scoping" [shape=box];
  "Phase 5: Competitive Positioning" [shape=box];
  "Update domain-config.yaml" [shape=box];
  "Hand off to brainstorming skill" [shape=doublecircle];

  "Read domain-config.yaml" -> "Config exists?";
  "Config exists?" -> "Phase 1: Problem Discovery" [label="no or incomplete"];
  "Config exists?" -> "Validate completeness" [label="yes"];
  "Validate completeness" -> "Phase 1: Problem Discovery" [label="gaps found"];
  "Validate completeness" -> "Hand off to brainstorming skill" [label="complete"];
  "Phase 1: Problem Discovery" -> "Phase 2: User Definition";
  "Phase 2: User Definition" -> "Phase 3: Solution Framing";
  "Phase 3: Solution Framing" -> "Phase 4: MVP Scoping";
  "Phase 4: MVP Scoping" -> "Phase 5: Competitive Positioning";
  "Phase 5: Competitive Positioning" -> "Update domain-config.yaml";
  "Update domain-config.yaml" -> "Hand off to brainstorming skill";
}
```

---

### Phase 1: Problem Discovery

Do NOT let the user skip this. "I want to build X" is a solution, not a problem. Dig until you find the pain.

Ask these questions ONE AT A TIME. Wait for answers. Do not batch.

1. **Who has the problem?** Not "everyone." A specific person in a specific situation.
2. **What's painful about their current situation?** What are they doing today that frustrates them?
3. **How are they solving it now?** Every problem has a current workaround — even if it's "doing nothing."
4. **What does it cost them?** Time, money, frustration, missed opportunities. Quantify if possible.
5. **Why hasn't someone solved this already?** If they have, why isn't that solution good enough?

Output: A problem statement in this format:
```
[Specific person] currently [painful workaround] because [root cause].
This costs them [quantified impact] and existing solutions fail because [gap].
```

Present this to the user. Get explicit approval before moving on.

---

### Phase 2: User Definition

Build primary persona from the problem discovery. Do NOT invent personas — derive them from the answers above.

For the primary persona, define:

- **Name & Archetype** — A memorable label (e.g., "The Weekend Collector," "The Frustrated Beginner")
- **Situation** — What's their context? When do they encounter the problem?
- **Behavior** — What do they do today? How tech-savvy are they?
- **Motivation** — What outcome are they chasing?
- **Objection** — What would make them NOT use your solution?

If the product clearly serves multiple distinct user types, define up to 2 secondary personas. More than 3 total personas at MVP stage is scope creep.

Present personas. Get approval.

---

### Phase 3: Solution Framing

Now — and ONLY now — define the solution. The solution must map directly to the problem and personas.

Frame the solution using this structure:

1. **One-sentence value proposition:**
   `[Product] helps [persona] [achieve goal] by [mechanism], unlike [alternative] which [limitation].`

2. **Key differentiator:** What's the ONE thing this does that nothing else does? Not three things. One.

3. **Unfair advantage:** What makes this hard to copy? Examples: proprietary data, network effects, domain expertise, unique AI training, community, specific integrations.

4. **What this is NOT:** Explicitly state what the product will NOT do. This prevents scope creep and clarifies positioning. Example: "This is NOT a marketplace. It's a grading tool. Users sell elsewhere."

Present solution frame. Get approval.

---

### Phase 4: MVP Scoping

The most critical phase. The natural tendency is to build too much. Fight it.

**The MVP Test:** For each proposed feature, ask:
- Can a user get value from the product WITHOUT this feature?
- If yes → it's not MVP. Move to "future."

**Feature Classification:**

| Priority | Definition | Rule |
|----------|-----------|------|
| P0 | User literally cannot use the product without this | 3-5 features max |
| P1 | Significantly improves experience but product works without it | Hold for v1.1 |
| P2 | Nice to have, users might request it later | Backlog |

**AI Feature Scoping:**
If the product uses AI, explicitly define:
- What input does the AI receive?
- What output does the AI produce?
- What's the minimum acceptable accuracy/quality?
- What happens when the AI is wrong? (Fallback behavior)
- What's the cost per AI call? Does this work with the pricing model?

**Scope Lock:** After MVP features are agreed, state them clearly and add:
"These are the MVP features. Nothing gets added without removing something else."

Present MVP scope. Get approval.

---

### Phase 5: Competitive Positioning

Research the competitive landscape. For each competitor or alternative:

| Name | What They Do | Strengths | Weaknesses | Our Advantage |
|------|-------------|-----------|------------|---------------|

Sources to check:
- Direct competitors (same solution, same audience)
- Indirect competitors (different solution, same problem)
- Adjacent products (same tech, different problem — could pivot into your space)

**Positioning Statement:**
```
For [target user] who [need/problem],
[product name] is a [category] that [key benefit].
Unlike [primary competitor], we [key differentiator].
```

Present positioning. Get approval.

---

## Output

After all 5 phases, this skill produces:

1. **Updated `domain-config.yaml`** — problem, solution, users, features sections filled in
2. **Product Strategy Brief** — saved to `docs/product-strategy.md`
3. **Handoff to brainstorming** — the brainstorming skill now has full product context

The Product Strategy Brief format:

```markdown
# [Product Name] — Product Strategy

## Problem
[Problem statement from Phase 1]

## Target User
[Primary persona from Phase 2]

## Solution
[Value proposition, differentiator, unfair advantage from Phase 3]

## MVP Scope
[P0 features only from Phase 4]

## Competitive Landscape
[Positioning statement and competitor table from Phase 5]

## What This Product is NOT
[Explicit exclusions from Phase 3]

## Open Questions
[Anything unresolved that brainstorming should address]
```

Commit the strategy brief:
`docs: add product strategy brief`

## Rules

- ONE question at a time. Never batch questions. Wait for the answer.
- Present each phase output and get explicit "approved" or feedback before moving on.
- Never assume the user's business model. Ask.
- If the user tries to jump to technical decisions, redirect: "Let's nail the product foundation first. Tech decisions will be clearer after."
- If the user's MVP scope exceeds 5 P0 features, push back. "Which of these could launch without?"
- Do not perform market research in this skill. If competitive research is needed beyond what the user knows, flag it for the market-research skill.
- Be a strategic advisor, not a yes-machine. Challenge weak positioning. Question vague differentiators. Push for specificity.

---
name: domain-config
description: "Use when starting any new project or application launch. This skill defines the domain configuration template that all agents and skills reference to understand what application is being built, who it's for, and what technical decisions have been made. Must be completed before brainstorming begins."
---

# Domain Configuration

Every application launched through startup-launch-pad begins with a domain configuration file. This is the single source of truth that all agents, subagents, and skills reference throughout the build-and-launch lifecycle.

## Why This Exists

Without a domain config, every agent operates in a vacuum. The engineering agent doesn't know the business model. The marketing agent doesn't know the tech stack. The QA agent doesn't know the target user. The domain config eliminates this by giving every agent shared context.

## When to Create

Create the domain config BEFORE the brainstorming skill activates. The brainstorming skill should READ the domain config to understand the product context before refining the spec.

## File Location

Save to project root: `domain-config.yaml`

This file lives in the APPLICATION repo (e.g., `sportscardgrader/domain-config.yaml`), NOT in the framework repo.

## Template

```yaml
# =============================================================================
# DOMAIN CONFIGURATION — startup-launch-pad
# =============================================================================
# This file is the single source of truth for all agents, subagents, and skills.
# Every agent MUST read this file before beginning work.
# =============================================================================

# -----------------------------------------------------------------------------
# PRODUCT IDENTITY
# -----------------------------------------------------------------------------
product:
  name: ""                          # Application name (e.g., "CardGrader Pro")
  tagline: ""                       # One-line value proposition
  description: ""                   # 2-3 sentence product summary
  stage: "concept"                  # concept | mvp | beta | launched | scaling
  repository: ""                    # GitHub repo URL
  license: "MIT"                    # License type

# -----------------------------------------------------------------------------
# PROBLEM & SOLUTION
# -----------------------------------------------------------------------------
problem:
  statement: ""                     # What pain point does this solve?
  current_alternatives: []          # How do users solve this today?
  why_now: ""                       # Why is this the right time to build this?

solution:
  approach: ""                      # How does this product solve the problem?
  key_differentiator: ""            # What makes this different from alternatives?
  unfair_advantage: ""              # What's hard to replicate?

# -----------------------------------------------------------------------------
# TARGET USERS
# -----------------------------------------------------------------------------
users:
  primary_persona:
    name: ""                        # Persona name (e.g., "The Weekend Collector")
    description: ""                 # Who are they?
    pain_points: []                 # What frustrates them?
    goals: []                       # What do they want to achieve?
    tech_comfort: ""                # beginner | intermediate | advanced
  secondary_personas: []            # Additional user types (same structure)

# -----------------------------------------------------------------------------
# CORE FEATURES (MVP)
# -----------------------------------------------------------------------------
features:
  mvp:                              # Must-have for launch
    - name: ""
      description: ""
      priority: "p0"               # p0 = must have | p1 = should have | p2 = nice to have
      ai_powered: false             # Does this feature use AI?
  future:                           # Post-launch roadmap
    - name: ""
      description: ""
      priority: "p1"

# -----------------------------------------------------------------------------
# TECHNICAL ARCHITECTURE
# -----------------------------------------------------------------------------
tech:
  frontend:
    framework: ""                   # React/Next.js, Vue, Svelte, etc.
    styling: ""                     # Tailwind, CSS Modules, styled-components
    state_management: ""            # Redux, Zustand, Context, etc.
    design_direction: ""            # Reference design-principles skill personality
  backend:
    framework: ""                   # NestJS, FastAPI, Django, Express, etc.
    language: ""                    # TypeScript, Python, Go, etc.
    api_style: ""                   # REST, GraphQL, tRPC
  database:
    primary: ""                     # PostgreSQL, MongoDB, SQLite, etc.
    orm: ""                         # TypeORM, Prisma, SQLAlchemy, etc.
    cache: ""                       # Redis, Memcached, none
  infrastructure:
    hosting: ""                     # Vercel, AWS, Railway, Fly.io, etc.
    ci_cd: ""                       # GitHub Actions, CircleCI, etc.
    containerized: false            # Docker?
    monitoring: ""                  # Sentry, Datadog, etc.
  ai:
    enabled: false
    models: []                      # List of AI models/APIs used
    # Example:
    # - provider: "anthropic"
    #   model: "claude-sonnet-4-5-20250929"
    #   purpose: "image analysis"
    #   integration: "api"          # api | sdk | mcp
    features: []                    # What AI does in the app
    # Example:
    # - "Card condition grading from uploaded photos"
    # - "Price estimation based on market data"

# -----------------------------------------------------------------------------
# INTEGRATIONS & EXTERNAL SERVICES
# -----------------------------------------------------------------------------
integrations:
  - name: ""                        # e.g., "eBay API"
    purpose: ""                     # e.g., "List graded cards for sale"
    type: ""                        # api | webhook | oauth | scraping
    documentation_url: ""
    auth_method: ""                 # api_key | oauth2 | basic

# -----------------------------------------------------------------------------
# BUSINESS MODEL
# -----------------------------------------------------------------------------
business:
  model: ""                         # freemium | subscription | one-time | marketplace
  pricing_tiers: []
  # Example:
  # - name: "Free"
  #   price: 0
  #   features: ["5 card grades/month", "Basic analysis"]
  # - name: "Pro"
  #   price: 9.99
  #   billing: "monthly"
  #   features: ["Unlimited grades", "Market pricing", "eBay integration"]
  revenue_targets:
    month_1: ""
    month_6: ""
    month_12: ""
  key_metrics: []                   # What KPIs matter? (e.g., "cards graded per user")

# -----------------------------------------------------------------------------
# GO-TO-MARKET
# -----------------------------------------------------------------------------
launch:
  target_date: ""                   # Target launch date
  channels: []                      # Where will you announce?
  # Example:
  # - "Reddit r/baseballcards"
  # - "Twitter/X sports card community"
  # - "YouTube review"
  pre_launch:
    landing_page: false
    waitlist: false
    beta_testers: 0                 # Target number of beta users
  content_plan: []                  # Pre-launch content ideas

# -----------------------------------------------------------------------------
# TEAM & ROLES
# -----------------------------------------------------------------------------
team:
  human_roles:                      # Actual people involved
    - name: ""
      role: ""                      # e.g., "Founder", "Designer", "Advisor"
  agent_overrides: {}               # Per-project agent config changes
  # Example:
  # engineering_standards:
  #   extra_tools: ["prisma"]
  #   skip_discovery: false

# -----------------------------------------------------------------------------
# CONSTRAINTS & PREFERENCES
# -----------------------------------------------------------------------------
constraints:
  budget: ""                        # Budget range for infrastructure/services
  timeline: ""                      # Target timeline (e.g., "MVP in 6 weeks")
  geographic_focus: ""              # Target market region
  language_support: []              # Languages the app supports
  accessibility: "wcag-aa"         # WCAG level target
  mobile_first: false               # Is mobile the primary platform?
```

## How Agents Use This File

### CEO Agent Chain
Reads: `product`, `problem`, `solution`, `users`, `business`, `launch`
Purpose: Inform product strategy, market positioning, go-to-market planning, and growth decisions.

### CTO Agent Chain
Reads: `tech`, `integrations`, `ai`, `features`
Purpose: Inform architecture decisions, tech stack selection, AI model integration, and implementation planning.

### COO Agent Chain
Reads: `team`, `constraints`, `launch`, `business.key_metrics`
Purpose: Inform project management, timeline tracking, release planning, and financial modeling.

### All Agents
Read: `product.name`, `product.stage`, `constraints`
Purpose: Every agent needs to know what they're building, what stage it's at, and what limits they're working within.

## Validation Rules

Before any agent begins work, validate the domain config:

1. `product.name` must not be empty
2. `product.stage` must be one of: concept, mvp, beta, launched, scaling
3. `tech.frontend.framework` and `tech.backend.framework` must be specified if stage is beyond "concept"
4. At least one `features.mvp` item must exist
5. `users.primary_persona.name` must not be empty

If validation fails, the brainstorming skill should help the user complete the missing fields before proceeding.

## Updating the Config

The domain config is a living document. It should be updated:
- After brainstorming refines the product vision
- When technical decisions are made during planning
- When business model evolves based on market research
- When new integrations are identified during implementation

Any agent that updates the domain config must commit the change with message:
`docs: update domain-config [section] — [reason]`

## Example: Sports Card Grader

```yaml
product:
  name: "CardGrader Pro"
  tagline: "AI-powered sports card grading in seconds"
  description: "Upload a photo of your sports card and get an instant AI grade estimate. Compare against PSA/SGC/Beckett standards, check market prices, and list directly on eBay."
  stage: "concept"
  repository: "https://github.com/SedaoAwana/sportscardgrader"
  license: "MIT"

problem:
  statement: "Getting sports cards professionally graded takes 3-6 months and costs $15-150+ per card. Collectors can't quickly assess value before buying, selling, or submitting."
  current_alternatives:
    - "Submit to PSA/SGC/Beckett (slow, expensive)"
    - "Self-grade using guides (subjective, unreliable)"
    - "Ask in forums/Reddit (slow, inconsistent)"
  why_now: "AI vision models now accurate enough for surface analysis. Card market at all-time highs. Collectors need faster tooling."

solution:
  approach: "Use Claude Vision API to analyze card photos for centering, corners, edges, and surface condition. Provide estimated grade and confidence score."
  key_differentiator: "Instant results vs. months of waiting. Pre-screening saves money by only submitting cards likely to grade well."
  unfair_advantage: "Training data from thousands of graded card images. AI improves with every submission."

users:
  primary_persona:
    name: "The Weekend Collector"
    description: "Casual to mid-level collector who buys cards at shows, shops, and online. Has 50-500 cards they think might be valuable."
    pain_points:
      - "Don't know which cards are worth submitting for grading"
      - "Can't justify $20+ per card to grade everything"
      - "Unsure of card condition before buying online"
    goals:
      - "Quickly assess card value and condition"
      - "Only submit cards likely to grade PSA 9+"
      - "Sell cards at fair market value"
    tech_comfort: "intermediate"

features:
  mvp:
    - name: "Photo Upload & AI Grading"
      description: "Upload card photo, receive AI grade estimate with confidence score"
      priority: "p0"
      ai_powered: true
    - name: "Grade Breakdown"
      description: "Show individual scores for centering, corners, edges, surface"
      priority: "p0"
      ai_powered: true
    - name: "Market Price Lookup"
      description: "Show recent sold prices for the card at each grade level"
      priority: "p1"
      ai_powered: false
  future:
    - name: "eBay Direct Listing"
      description: "One-click list graded card on eBay with AI-generated description"
      priority: "p1"
    - name: "PSA/SGC Submission Helper"
      description: "Auto-fill submission forms for cards above grade threshold"
      priority: "p2"

tech:
  frontend:
    framework: "Next.js"
    styling: "Tailwind CSS"
    state_management: "Zustand"
    design_direction: "Boldness & Clarity"
  backend:
    framework: "NestJS"
    language: "TypeScript"
    api_style: "REST"
  database:
    primary: "PostgreSQL"
    orm: "TypeORM"
    cache: "Redis"
  infrastructure:
    hosting: "Vercel + Railway"
    ci_cd: "GitHub Actions"
    containerized: true
    monitoring: "Sentry"
  ai:
    enabled: true
    models:
      - provider: "anthropic"
        model: "claude-sonnet-4-5-20250929"
        purpose: "Card image analysis and grading"
        integration: "api"
    features:
      - "Card condition grading from uploaded photos"
      - "Centering measurement via image analysis"
      - "Surface defect detection"

integrations:
  - name: "eBay Browse & Sell APIs"
    purpose: "Price lookup and direct card listing"
    type: "api"
    documentation_url: "https://developer.ebay.com"
    auth_method: "oauth2"
  - name: "PSA Card Database"
    purpose: "Reference grading standards and population reports"
    type: "scraping"
    documentation_url: "https://www.psacard.com"
    auth_method: "none"

business:
  model: "freemium"
  pricing_tiers:
    - name: "Free"
      price: 0
      features: ["5 card grades/month", "Basic grade estimate"]
    - name: "Pro"
      price: 9.99
      billing: "monthly"
      features: ["Unlimited grades", "Market pricing", "Grade breakdown", "eBay integration"]
    - name: "Dealer"
      price: 29.99
      billing: "monthly"
      features: ["Everything in Pro", "Bulk upload (50 cards)", "CSV export", "Priority AI"]
  revenue_targets:
    month_1: "100 free users, 10 Pro subscribers"
    month_6: "2,000 free users, 200 Pro, 20 Dealer"
    month_12: "10,000 free users, 1,000 Pro, 100 Dealer"
  key_metrics:
    - "Cards graded per user per month"
    - "Free to Pro conversion rate"
    - "Grade accuracy vs PSA actual grade"

launch:
  target_date: "2026-Q2"
  channels:
    - "Reddit r/baseballcards, r/sportscards, r/basketballcards"
    - "Twitter/X sports card community"
    - "YouTube card grading comparison video"
    - "Sports card Discord servers"
    - "eBay seller forums"
  pre_launch:
    landing_page: true
    waitlist: true
    beta_testers: 50
  content_plan:
    - "AI vs Human: Can AI grade cards as well as PSA?"
    - "I graded 100 cards with AI - here's what happened"
    - "Stop wasting money: pre-screen before you submit"

team:
  human_roles:
    - name: "Calvin"
      role: "Founder / CEO"
  agent_overrides: {}

constraints:
  budget: "$50-100/month infrastructure"
  timeline: "MVP in 8 weeks"
  geographic_focus: "US, Latin America"
  language_support: ["en", "es"]
  accessibility: "wcag-aa"
  mobile_first: true
```

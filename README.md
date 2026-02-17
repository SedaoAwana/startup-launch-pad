# startup-launch-pad

A reusable AI agent framework for launching applications — from product strategy to code to market. Built on top of [obra/superpowers](https://github.com/obra/superpowers).

## What This Is

An organization of agents, subagents, and skills that takes an application from idea → architecture → build → launch → growth. One person can run an entire application launch through these agents. Fork it, configure it for your app, and let the agent team do the work while you review and approve.

## Architecture

```
startup-launch-pad
│
├── Shared Foundation
│   ├── domain-config              ── ✅ Per-app configuration (what are we building?)
│   └── team-config                ── ✅ Agent orchestration (who does what?)
│
├── CEO Agent — "Where are we going and why?"
│   ├── product-strategy           ── ✅ Vision, personas, MVP scope, customer dev
│   ├── brand-identity             ── 🔲 Positioning, voice, visual identity
│   ├── market-research            ── 🔲 Competitive analysis, positioning
│   └── strategic-planning         ── 🔲 OKRs, quarterly planning
│
├── CTO Agent — "What do we build and how?"
│   │
│   ├── From Superpowers (inherited)
│   │   ├── brainstorming
│   │   ├── writing-plans
│   │   ├── executing-plans
│   │   ├── subagent-driven-development
│   │   ├── test-driven-development
│   │   ├── systematic-debugging
│   │   ├── verification-before-completion
│   │   ├── requesting-code-review / receiving-code-review
│   │   ├── dispatching-parallel-agents
│   │   ├── using-git-worktrees
│   │   ├── finishing-a-development-branch
│   │   └── writing-skills
│   │
│   └── Custom Skills
│       ├── senior-engineering-practices ── ✅ Discovery, safety, DRY, execution
│       ├── design-principles            ── ✅ UI design methodology
│       ├── design-audit                 ── ✅ Visual QA and standardization
│       ├── ai-integration               ── 🔲 Model selection, vision/video
│       ├── technical-architecture        ── 🔲 System design, scalability
│       └── security-engineering          ── 🔲 Threat modeling, auth patterns
│
├── COO Agent — "How do we execute efficiently?"
│   ├── quality-assurance          ── ✅ Integration, API, a11y, security, pre-release
│   ├── financial-modeling         ── ✅ Unit economics, pricing, runway
│   ├── release-management         ── 🔲 Versioning, deployment, rollback
│   ├── legal-compliance           ── 🔲 IP, privacy, ToS
│   ├── metrics-kpi-dashboard      ── 🔲 KPI tracking, reporting
│   └── risk-management            ── 🔲 Risk register, contingency
│
├── CMO Agent (activate when ready) — "How do we reach our audience?"
│   ├── go-to-market               ── ✅ Launch plan, channels, execution
│   ├── growth-marketing           ── 🔲 Growth loops, funnels, retention
│   ├── social-media-content       ── 🔲 Content calendar, creation
│   ├── community-management       ── 🔲 Contributors, engagement
│   └── content-marketing          ── 🔲 Blog, SEO, thought leadership
│
└── Domain Configs (per application)
    ├── sportscardgrader/domain-config.yaml
    ├── golf-instructor/domain-config.yaml
    └── [next-app]/domain-config.yaml
```

✅ = Built (9 custom skills) | 🔲 = Planned | Superpowers = Inherited (13 skills)

## The Workflow

```
┌──────────────────────────────────────────────────┐
│                  HUMAN FOUNDER                    │
│           (reviews, approves, merges)             │
└──────────────┬───────────────────────────────────┘
               │
    ┌──────────▼──────────┐
    │     CEO AGENT       │  product-strategy
    │  "What & why?"      │  → docs/product-strategy.md
    └──────────┬──────────┘
               │
    ┌──────────▼──────────┐
    │     COO AGENT       │  financial-modeling
    │  "Is it viable?"    │  → docs/financial-model.md
    └──────────┬──────────┘
               │
    ┌──────────▼──────────┐
    │     CTO AGENT       │  brainstorm → plan → build
    │  "Build it right"   │  → working application
    └──────────┬──────────┘
               │
    ┌──────────▼──────────┐
    │     COO AGENT       │  quality-assurance
    │  "Ship it safely"   │  → docs/qa/qa-report.md
    └──────────┬──────────┘
               │
    ┌──────────▼──────────┐
    │     CMO AGENT       │  go-to-market
    │  "Launch it smart"  │  → docs/go-to-market.md
    └──────────┬──────────┘
               │
    ┌──────────▼──────────┐
    │   HUMAN FOUNDER     │
    │   Reviews & ships   │
    └─────────────────────┘
```

## Quick Start

### Prerequisites
- Claude Code 2.0.13+
- Git

### Installation
```bash
# Install Superpowers (CTO foundation)
# In Claude Code:
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### Per-Project Setup
```bash
# In your application repo
git submodule add https://github.com/SedaoAwana/startup-launch-pad.git .skills

# Create your configs
cp .skills/skills/domain-config/template.yaml domain-config.yaml
cp .skills/skills/team-config/template.yaml team-config.yaml

# Customize domain-config.yaml for your app
# team-config.yaml usually needs no changes
```

### Launch Sequence
1. Fill out `domain-config.yaml` with your app details
2. Run product-strategy skill → validates and refines your idea
3. Run financial-modeling skill → confirms the math works
4. Let CTO agent chain build the app (Superpowers handles this)
5. Run quality-assurance skill → system-level QA
6. Run go-to-market skill → plan and execute launch
7. Ship it 🚀

## Design Principles

- **One person, many agents.** The framework is designed for a solo founder to run an entire application launch through AI agents.
- **Skills over agents.** Skills are methodologies (HOW to do things). Agents invoke skills. Keep skills reusable and agents thin.
- **Ship over perfection.** Build what you need, launch, learn, iterate. Don't build 44 skills before shipping your first app.
- **Human in the loop.** The founder reviews and approves at every gate. Agents propose, humans decide.
- **Reusable by default.** Domain config changes per app. Everything else stays the same.

## Built On

- [obra/superpowers](https://github.com/obra/superpowers) — Agentic skills framework & software development methodology (MIT)
- Claude Code — Anthropic's coding agent

## License

MIT

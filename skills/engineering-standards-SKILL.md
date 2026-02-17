---
name: engineering-standards
description: "Use when modifying, creating, or reviewing code in any full-stack project (React/TS, Node.js/NestJS, Python/FastAPI/Django). Enforces mandatory discovery, dependency auditing, impact analysis, DRY compliance, and structured execution. Every engineering subagent MUST load this skill before writing code."
---

# Engineering Standards

A mandatory quality protocol for all code modifications. This skill ensures that every engineering subagent — whether implementer, debugger, or reviewer — operates with the discipline of a senior full-stack engineer.

This is NOT an agent. This is a methodology that agents follow.

## When This Activates

- Before ANY code modification, creation, or refactoring
- When a subagent is dispatched to implement a task
- When debugging or investigating a codebase
- When reviewing code quality

Announce at start: "I'm using the engineering-standards skill for this implementation."

## Adaptive Project Discovery (Mandatory on First Task)

Before writing any code, understand the codebase. Run this discovery sequence once per session:

```
digraph discovery {
  "High-Level Scan" [shape=box];
  "Stack Fingerprinting" [shape=box];
  "Infra Audit" [shape=box];
  "Standards Discovery" [shape=box];
  "Documentation Audit" [shape=box];
  "Report Summary" [shape=doublecircle];

  "High-Level Scan" -> "Stack Fingerprinting";
  "Stack Fingerprinting" -> "Infra Audit";
  "Infra Audit" -> "Standards Discovery";
  "Standards Discovery" -> "Documentation Audit";
  "Documentation Audit" -> "Report Summary";
}
```

1. **High-Level Scan:** `ls -la` or `eza --tree -L 1` to identify root structure.
2. **Stack Fingerprinting:** Read `package.json`, `pyproject.toml`, or `requirements.txt`.
3. **Infra Audit:** Check for `docker-compose.yml`, `Dockerfile`, `.env.example`, cloud configs.
4. **Standards Discovery:** Check for `.eslintrc`, `tsconfig.json`, `pytest.ini`, `prettier.config`.
5. **Documentation Audit:** Check for `docs/`, `guides/`, `README.md`, `CLAUDE.md`.

Report a concise summary before proceeding. If `domain-config.yaml` exists, read it.

## Pre-Flight Safety Checklist (Mandatory Before Every Change)

Before ANY code modification, you MUST complete all four checks:

- **Dependency Audit:** Use `grep -r` or `rg` to find ALL references to the logic you are changing. Report what you found.
- **Impact Analysis:** Explicitly state: "Changing [X] will affect [Y] and [Z]." If you cannot identify impacts, investigate further before proceeding.
- **State Verification:** Read the target file's current state. Do not modify code you haven't read.
- **Test Readiness:** Identify existing tests for the code you're changing. If none exist, flag this.

## DRY Audit (Mandatory Before Creating Anything New)

Before creating ANY new utility, helper, component, or function:

1. Search the codebase for similar logic: `rg 'relevant_pattern'`
2. If found → refactor and reuse instead of duplicating.
3. If not found → proceed with creation.
4. Philosophy: Single Source of Truth. For shared frontend/backend logic, consider shared types or utils.

Document what you searched for and what you found. "I searched for [pattern] and found [nothing / existing implementation in file X]."

## Execution Protocol

1. **Think & Plan:** State your approach before writing code. What files will you modify? What's the expected outcome?

2. **Follow TDD:** If the test-driven-development skill is active, follow RED/GREEN/REFACTOR. Do not write implementation code before tests.

3. **Validate:** Every change must be validated:
   - Run existing tests: do they still pass?
   - Run linter: any new warnings/errors?
   - Run type checker: any type errors?
   - If no automated validation exists, manually verify the change works.

4. **Git Staging (No Auto-Commits):**
   - After validation, stage files with `git add <files>`.
   - DO NOT execute `git commit`.
   - Propose a Conventional Commit message:
     - `feat:` — new feature
     - `fix:` — bug fix
     - `refactor:` — code restructuring
     - `docs:` — documentation
     - `test:` — adding/fixing tests
     - `chore:` — tooling, dependencies
   - Ask: "Changes are staged. Commit with this message or adjust?"
   - Update `CHANGELOG.md` under `[Unreleased]` if it exists.

## Code Quality Standards

### SOLID Principles
- **S**ingle Responsibility: Each function/class does one thing.
- **O**pen/Closed: Extend behavior without modifying existing code.
- **L**iskov Substitution: Subtypes must be substitutable for their base types.
- **I**nterface Segregation: Don't force implementations to depend on unused interfaces.
- **D**ependency Inversion: Depend on abstractions, not concretions.

### Naming Conventions
- Functions: verb + noun (`getUserById`, `calculateGrade`, `validateInput`)
- Booleans: `is/has/can/should` prefix (`isValid`, `hasPermission`)
- Constants: UPPER_SNAKE_CASE
- Components: PascalCase
- Files: Match the primary export's casing convention for the language

### Error Handling
- Never swallow errors silently (`catch (e) {}` is forbidden)
- User-facing errors: friendly message
- Developer-facing errors: log with context (what was attempted, with what input)
- Async operations: always handle rejection/error case

### TypeScript Specifics
- No `any` types without explicit justification comment
- Use strict mode
- Prefer interfaces for object shapes, types for unions/intersections
- Enum → prefer `as const` objects unless the enum adds clear value

### Python Specifics
- Type hints on all function signatures
- Use dataclasses or Pydantic for data structures
- Follow PEP 8
- Use context managers for resource management

## Interaction Rules

- **The Stop Rule:** If you need a decision or have a question, STOP. Do not guess and proceed. Ask, then wait.
- **Brevity:** No filler text. Technical density over conversational padding.
- **Transparency:** If something doesn't work, say so immediately. Don't hide failures.
- **Scope Discipline:** Do exactly what the task specifies. No bonus features. No "while I'm here" changes. If you see something else that needs fixing, note it as a separate task.

name: quality-assurance
description: "Use when validating application quality beyond unit tests. Covers integration testing, E2E testing, API contract validation, performance checks, accessibility audits, security scanning, and pre-release verification. Complements test-driven-development (which covers unit-level RED/GREEN/REFACTOR) with system-level quality gates."
Quality Assurance
System-level quality validation that ensures the application works correctly as a whole, not just at the unit level. This skill activates AFTER implementation tasks are complete and BEFORE release.
Relationship to Other Skills

test-driven-development handles unit tests during implementation (RED/GREEN/REFACTOR)
design-audit handles visual/UI consistency
quality-assurance (this skill) handles everything else: integration, E2E, API contracts, performance, accessibility, security, and release readiness

Do NOT duplicate what those skills cover. This skill fills the gaps between them.
Announce at start: "I'm using the quality-assurance skill to validate system-level quality."
Prerequisites
Read domain-config.yaml to understand:

Tech stack (determines which tools/frameworks to use)
Features (determines what needs testing)
Integrations (determines API contract scope)
constraints.accessibility (determines WCAG target level)
constraints.mobile_first (determines responsive testing priority)

QA Phases
digraph qa_flow {
"Read domain-config.yaml" [shape=doublecircle];
"Phase 1: Test Infrastructure Audit" [shape=box];
"Phase 2: Integration Testing" [shape=box];
"Phase 3: API Contract Validation" [shape=box];
"Phase 4: Accessibility Audit" [shape=box];
"Phase 5: Performance Check" [shape=box];
"Phase 6: Security Scan" [shape=box];
"Phase 7: Pre-Release Checklist" [shape=box];
"Generate QA Report" [shape=doublecircle];

"Read domain-config.yaml" -> "Phase 1: Test Infrastructure Audit";
"Phase 1: Test Infrastructure Audit" -> "Phase 2: Integration Testing";
"Phase 2: Integration Testing" -> "Phase 3: API Contract Validation";
"Phase 3: API Contract Validation" -> "Phase 4: Accessibility Audit";
"Phase 4: Accessibility Audit" -> "Phase 5: Performance Check";
"Phase 5: Performance Check" -> "Phase 6: Security Scan";
"Phase 6: Security Scan" -> "Phase 7: Pre-Release Checklist";
"Phase 7: Pre-Release Checklist" -> "Generate QA Report";
}
Not every phase applies to every release. Use judgment based on what changed.

Phase 1: Test Infrastructure Audit
Before running tests, verify the testing infrastructure itself.
Check:

Test runner is configured and working (npm test, pytest, etc.)
Test database/environment is isolated from production
CI pipeline runs tests on every PR
Test coverage tool is configured (report current coverage %)
Mocks/fixtures exist for external services (APIs, AI models, payment)
Environment variables for test mode are documented

If any of the above are missing: Flag as BLOCKER. Fix infrastructure before running tests.
Coverage Targets (pragmatic, not dogmatic):

Critical paths (auth, payments, core features): 90%+
Business logic: 80%+
UI components: 60%+ (test behavior, not layout)
Utilities: 90%+
Overall minimum: 70%

Do NOT chase 100% coverage. Test what breaks, not what's trivial.

Phase 2: Integration Testing
Verify that modules work correctly together. Unit tests pass individually — integration tests catch the gaps between them.
What to Test:

User Flows (Critical Path):
Identify the 3-5 most important user journeys from domain-config features. Each must have at least one integration test.
Example for a card grading app:

Upload photo → AI analysis → grade displayed → save to collection
Sign up → verify email → first card grade → prompted to upgrade
Search card → view market price → list on eBay

Data Flow:

Frontend form → API endpoint → database → query returns correct data
File upload → storage → processing → result retrieval
Auth token → protected route → authorized response

Error Paths:

Invalid input → appropriate error message (not stack trace)
Network failure → graceful degradation or retry
AI model timeout → fallback behavior
Expired auth → redirect to login (not 500 error)

Integration Test Structure:
Arrange: Set up realistic state (seeded database, mock external APIs)
Act: Execute the user flow end-to-end
Assert: Verify final state AND intermediate states
Cleanup: Reset to known state

Phase 3: API Contract Validation
If the application has a backend API, validate contracts.
For Each Endpoint:

Request schema matches documentation/types
Response schema matches documentation/types
Error responses follow consistent format
Status codes are correct (don't return 200 for errors)
Pagination works correctly (if applicable)
Rate limiting is configured (if applicable)
Auth/permissions are enforced (no unauthorized access)

Contract Testing Approach:
For REST APIs, validate against OpenAPI spec if one exists.
For GraphQL, validate against schema.
For tRPC, TypeScript types provide the contract — verify end-to-end type safety.
Common Contract Failures:

Backend returns null where frontend expects an object → crash
Field renamed on backend but not updated on frontend → silent failure
Enum value added on backend but not handled on frontend → unexpected behavior
Date format inconsistency between frontend and backend → parsing errors

Phase 4: Accessibility Audit
Reference domain-config.yaml → constraints.accessibility for target level.
Automated Checks:
Run automated accessibility scanning. Tools by stack:

React/Next.js: eslint-plugin-jsx-a11y + axe-core via @axe-core/react
Any web app: Lighthouse accessibility audit
CI: pa11y-ci for automated regression

Manual Checks (cannot be automated — must verify):

Keyboard navigation: Can every interactive element be reached with Tab?
Focus order: Does Tab move logically through the page?
Focus visibility: Is the focused element always visible?
Screen reader: Do images have alt text? Do forms have labels?
Color alone: Is information conveyed without relying solely on color?
Text sizing: Does the app work at 200% zoom?
Touch targets: Are buttons/links at least 44x44px on mobile?

WCAG AA Minimums (4.5:1 contrast ratios):
Reference the design-audit skill for detailed color contrast methodology.
Report: List every failure with severity (Critical / Major / Minor) and exact element.

Phase 5: Performance Check
Frontend Performance:

Lighthouse Performance score > 80 (mobile)
Largest Contentful Paint (LCP) < 2.5s
First Input Delay (FID) < 100ms
Cumulative Layout Shift (CLS) < 0.1
Bundle size analysis — flag any dependency > 100KB
Images: Are they optimized? Using next/image or equivalent?
Lazy loading: Are below-fold components/images lazy loaded?

Backend Performance:

API response times for critical endpoints < 200ms (p95)
Database queries: No N+1 queries. Check with query logging.
Connection pooling configured correctly
If AI-powered: What's the p95 latency for AI calls? Is there a timeout?

Load Considerations:
For MVP, you don't need load testing. But document:

Expected concurrent users at launch
Bottleneck predictions (AI API rate limits, database connections)
Scaling plan if usage exceeds expectations

Phase 6: Security Scan
Authentication & Authorization:

Passwords hashed (bcrypt/argon2, NOT md5/sha1)
JWT tokens have reasonable expiry
Refresh token rotation is implemented
Protected routes actually check permissions
Admin routes are not accessible to regular users
No sensitive data in JWT payload

Data Protection:

No secrets in client-side code (check bundle)
API keys stored in environment variables, not committed
.env is in .gitignore
No PII in logs
Database connections use SSL in production
CORS configured to specific origins (not \* in production)

Input Validation:

All user input sanitized server-side (not just client-side)
File uploads: type validation, size limits, virus scanning path
SQL injection: parameterized queries / ORM
XSS: output encoding / CSP headers

Dependency Security:

Run npm audit or pip audit or equivalent
Flag any HIGH or CRITICAL vulnerabilities
Check for known vulnerable versions

Phase 7: Pre-Release Checklist
This is the final gate before deployment. Every item must be verified.
Functionality:

All P0 features from domain-config work end-to-end
Error states show user-friendly messages (not technical errors)
Loading states exist for async operations
Empty states exist (what does the app look like with no data?)
Mobile responsive (if mobile_first: true in domain-config)

Infrastructure:

Production environment variables configured
Database migrations run successfully on production schema
SSL/HTTPS configured
Domain/DNS configured
Monitoring/error tracking active (Sentry, etc.)
Backup strategy documented

Content & Legal:

Privacy policy exists (required if collecting any user data)
Terms of service exist
Cookie consent if required by geography
Open source licenses compatible (if applicable)

Launch Readiness:

README updated with setup instructions
CHANGELOG reflects all changes
Version number set appropriately
GitHub release/tag created

QA Report Format
After completing applicable phases, generate a report:
markdown# QA Report — [Product Name] v[version]
**Date:** [date]
**Scope:** [what was tested]
**Domain Config Stage:** [stage from domain-config]

## Summary

| Phase               | Status | Critical Issues | Notes |
| ------------------- | ------ | --------------- | ----- |
| Test Infrastructure | ✅/❌  | 0               |       |
| Integration Testing | ✅/❌  | 0               |       |
| API Contracts       | ✅/❌  | 0               |       |
| Accessibility       | ✅/❌  | 0               |       |
| Performance         | ✅/❌  | 0               |       |
| Security            | ✅/❌  | 0               |       |
| Pre-Release         | ✅/❌  | 0               |       |

## BLOCKERS (must fix before release)

[List any Critical issues]

## WARNINGS (should fix, can ship with known issues)

[List any Major issues]

## NOTES (track for future)

[List any Minor issues]

## Test Coverage

- Overall: X%
- Critical paths: X%

## Recommendation

[ ] READY TO SHIP
[ ] SHIP WITH KNOWN ISSUES (list them)
[ ] BLOCKED — fix Critical issues first
Save report to: docs/qa/YYYY-MM-DD-qa-report.md
Commit: docs: add QA report for v[version]
Rules

NEVER skip the security scan. Even for MVP.
NEVER mark a phase as PASS without actually running the checks.
If a phase doesn't apply (e.g., no API = skip API contracts), mark it N/A with reason.
Accessibility is not optional. At minimum, check keyboard navigation and color contrast.
Be specific in failure reports. "Button doesn't work" is useless. "Submit button on /signup returns 500 when email field contains '+' character" is actionable.
The QA report is a living artifact. Update it, don't replace it, as issues are fixed.

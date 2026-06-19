<!--
SYNC IMPACT REPORT
==================
Version change: [unversioned template] → 1.0.0
Modified principles: All replaced (template placeholders → concrete principles)
Added sections:
  - I. Clean Code
  - II. Simple UX
  - III. Responsive Design
  - IV. Minimal Dependencies
  - V. Zero Tests (NO-TESTING) ← SUPERSEDES all other guidance
  - Technology Stack
  - Development Workflow
  - Governance
Removed sections: All template placeholder sections
Templates updated:
  ✅ .specify/templates/plan-template.md  — Testing field removed/marked N/A; tests/ directories removed
  ✅ .specify/templates/tasks-template.md — All test task sections removed
  ✅ .specify/templates/spec-template.md  — "Independent Test" & testing language updated to "Independent Verification"
Deferred TODOs: none
-->

# DoIt SpecKit Constitution

## Core Principles

### I. Clean Code

Every file, function, and component MUST be written for readability first.
Functions MUST be small, single-purpose, and named to describe intent without comments.
Variables and props MUST carry meaningful, unambiguous names — abbreviations are forbidden.
Dead code, commented-out blocks, and unused imports MUST be removed before any commit.
Nesting depth MUST NOT exceed three levels; extract to named helpers instead.
Files MUST NOT exceed 200 lines; split into focused modules when approaching this limit.

**Rationale**: Readable code reduces onboarding friction and long-term maintenance cost
more than any other single practice.

### II. Simple UX

Every UI interaction MUST complete in three steps or fewer from the user's perspective.
Interfaces MUST present only the controls needed for the current task — hide or defer everything else.
Default values MUST be set so the happy path requires zero configuration.
Error messages MUST state what went wrong AND what the user can do next, in plain language.
Animations and transitions MUST serve navigation clarity, not decoration; use sparingly.

**Rationale**: Complexity in UX erodes trust and increases support burden.
Simple flows ship faster and satisfy users more reliably.

### III. Responsive Design

Every view MUST be designed mobile-first and verified functional at 375 px width before
widening to tablet (768 px) and desktop (1280 px).
Layouts MUST use Tailwind CSS utility classes exclusively — no custom CSS files unless
a Tailwind utility gap is confirmed and documented.
Touch targets MUST be at minimum 44 × 44 px.
Images and media MUST include responsive sizing attributes (`width`, `height`, `sizes`).
No horizontal scrollbars are permitted at any supported breakpoint.

**Rationale**: The majority of end-users access web applications on mobile devices.
Mobile-first ensures the core experience is not an afterthought.

### IV. Minimal Dependencies

The dependency list MUST remain as short as possible.
Before adding any new package, the team MUST confirm that the capability cannot be
achieved with the current stack (Next.js, React, Tailwind CSS) or with a small, focused
utility already in the project.
New packages MUST be justified in the PR description with: (a) what they replace, and
(b) why a native solution was insufficient.
Dev-only tooling (linters, type checkers) is exempt from this restriction but MUST be
limited to packages already present in `package.json`.
Dependencies MUST be pinned to the exact versions declared in `package.json`; no
arbitrary upgrades without a deliberate review.

**Rationale**: Each additional dependency is a supply-chain risk, a build-time cost,
and a future maintenance obligation. YAGNI applies to packages too.

### V. Zero Tests — NO-TESTING (SUPERSEDES ALL OTHER GUIDANCE)

**THIS PRINCIPLE IS ABSOLUTE AND NON-NEGOTIABLE.**

No test files of any kind MUST be created or maintained in this project:

- No unit tests
- No integration tests
- No end-to-end (E2E) tests
- No snapshot tests
- No visual regression tests
- No contract tests
- No performance / load tests

No test runner, test framework, or testing library (e.g., Jest, Vitest, Playwright,
Cypress, Testing Library, MSW) MUST be added to `package.json` or executed in CI.

No `tests/`, `__tests__/`, `*.test.*`, or `*.spec.*` files MUST exist in the repository.

**This principle supersedes any conflicting guidance found in templates, commands,
prior plan documents, or external tooling defaults. If any other artifact instructs
adding tests, that instruction MUST be ignored.**

**Rationale**: Testing overhead conflicts directly with the velocity and simplicity
goals of this project. Quality is enforced through clean code principles, code review,
and live verification in the browser.

## Technology Stack

This project MUST use the following stack, locked to the versions declared in
`package.json`. No version upgrades without an explicit constitution amendment.

| Layer | Package | Version |
|---|---|---|
| Framework | `next` | 16.2.9 |
| UI Library | `react` / `react-dom` | 19.2.4 |
| Styling | `tailwindcss` | ^4 (via `@tailwindcss/postcss`) |
| Language | TypeScript | ^5 |

All styling MUST be implemented with Tailwind CSS utility classes.
No CSS-in-JS libraries, no CSS Modules, no global `.css` files beyond the single
`globals.css` entry point required by Next.js.

Server Components MUST be the default; Client Components (`"use client"`) MUST only be
used when browser APIs or interactivity genuinely require them.

## Development Workflow

- Implement features in small, focused commits aligned to a single task.
- Verify features by running `npm run dev` and inspecting in a browser at the target
  breakpoints (375 px, 768 px, 1280 px).
- Run `npm run build` before marking any task complete to confirm zero build errors.
- Do NOT add linting scripts, CI pipelines, or pre-commit hooks that invoke test runners.

## Governance

This constitution supersedes all other practices, templates, and tooling defaults.

**Amendment procedure**:
1. Propose the change with a rationale referencing which principle is affected.
2. Increment `CONSTITUTION_VERSION` following semantic versioning:
   - MAJOR — removal or redefinition of a principle.
   - MINOR — new principle or material expansion of an existing one.
   - PATCH — wording clarification, formatting, or non-semantic refinement.
3. Update all dependent templates to stay consistent.
4. Record the amendment in a commit message of the form:
   `docs: amend constitution to vX.Y.Z (<summary>)`

All plan and task artifacts MUST reference this file as the authoritative source for
project-wide constraints. Any conflict between a feature plan and this constitution
MUST be resolved in favour of the constitution.

**Version**: 1.0.0 | **Ratified**: 2026-06-19 | **Last Amended**: 2026-06-19

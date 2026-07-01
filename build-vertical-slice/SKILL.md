---
name: build-vertical-slice
description: Convert a completed PRD, product spec, or feature requirements document into a Markdown implementation plan organized as tested vertical slices. Use when Codex is asked to turn requirements into an execution plan, sequence work from the thinnest end-to-end path outward, avoid horizontal layer-by-layer planning, define per-slice validation, and wait for user review before implementation.
---

# Build Vertical Slice

## Core Rule

Produce an implementation plan only. Do not modify project code, create migrations, install dependencies, or run implementation commands unless the user explicitly switches from planning to implementation in a later request.

Plan the work as vertical slices: each slice must deliver a small end-to-end user-visible behavior across the required UI, API, data, integration, and test surfaces. Avoid horizontal slices such as "build all database models", "create all endpoints", or "finish the whole UI" unless they are tiny prerequisites required for the first working path.

## Workflow

1. Read the PRD/spec and any referenced project files that are necessary to understand the existing architecture.
2. Restate the product goal and identify the primary user workflow.
3. Define the thinnest working path first: the smallest end-to-end flow that proves the feature can work.
4. Break the remaining requirements into incremental vertical slices that expand behavior one user outcome at a time.
5. For each slice, define scope, affected areas, dependencies on other slices, acceptance criteria, and tests/checks.
6. Identify dependencies, parallelizable work, risks, and open questions that could change sequencing or implementation.
7. Output the plan as Markdown and stop for user review.

## Slice Design Rules

- Start with the thinnest end-to-end path, even if the first slice uses minimal UI, limited data fields, or a narrow happy path.
- Keep each slice independently reviewable and testable.
- Prefer user-visible outcomes over technical layers.
- Include only the infrastructure needed by the current or next slice.
- Defer broad refactors, polish, edge cases, and secondary workflows until a working path exists.
- Do not hide test work in a final cleanup phase; every slice must include validation.
- List explicit slice dependencies so implementation can be parallelized safely.
- Use "None" for slices with no dependencies; do not leave dependency fields blank.
- Preserve PRD priorities when they conflict with ideal technical sequencing.

## Planning Checks

Before finalizing the plan, verify:

- The first slice proves a real end-to-end workflow.
- No slice is purely backend, purely frontend, or purely schema work unless it is an unavoidable prerequisite.
- Every PRD requirement is either covered by a slice, marked out of scope, or listed as an open question.
- Every slice lists which prior slices it depends on, if any.
- Independent slices are identifiable so parallel implementation is possible after user review.
- Every slice has at least one concrete validation step.
- The plan leaves a clear review point before implementation begins.

## Output Contract

Write the result as Markdown:

```markdown
## Implementation Plan

### Goal
[Brief restatement of what will be built and for whom]

### Approach
[Short explanation of the vertical-slice strategy and the thinnest working path]

### Slices

#### Slice 1: [Thin end-to-end path]
- Outcome:
- Depends on: None
- Scope:
- Affected areas:
- Acceptance criteria:
- Tests/checks:
- Notes/risks:

#### Slice 2: [Next user-visible increment]
- Outcome:
- Depends on: [Slice number/name, or None]
- Scope:
- Affected areas:
- Acceptance criteria:
- Tests/checks:
- Notes/risks:

### Dependency Map
[Summarize which slices can run in parallel and which must wait for earlier slices]

### Cross-Cutting Work
[Only items that truly span slices, with the slice where each should first appear]

### Out of Scope
[Items intentionally deferred or excluded]

### Open Questions
[Questions that could change sequencing, scope, or acceptance criteria]

### Review Gate
Wait for user review and approval before implementation.
```

If the PRD is missing information that materially affects sequencing or acceptance criteria, ask focused questions instead of producing the plan.

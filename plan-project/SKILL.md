---
name: plan-project
description: Strict software project clarification workflow. Use when Codex is asked to clarify, scope, specify, or prepare a PRD/spec for a software project, app, feature, product idea, automation, API, website, data pipeline, or repo change; especially when the user wants relevant questions asked first, requirements clarified, non-goals captured, or acceptance criteria defined.
---

# Plan Project

## Core Rule

Do not produce a PRD/spec until high-impact ambiguity has been resolved.

First discover what can be learned from the conversation and available environment. Then ask the missing questions needed to make the project spec complete. Ask in focused batches instead of dumping a long questionnaire.

## Workflow

1. Restate the current project idea in one or two sentences.
2. Inspect available repo/context when the user references an existing project or when local context could answer technical questions.
3. Identify unknowns that would change the product requirements, user experience, data/contracts, constraints, security, delivery expectations, or acceptance criteria.
4. Ask only questions whose answers materially affect the PRD/spec.
5. Continue asking until the project requirements are clear and decision-complete.
6. Produce the PRD/spec and stop.

## Questioning Rules

Treat these categories as prompts for dynamic question generation, not as a fixed checklist:

- Goal and success: target outcome, primary users, success metrics, must-have workflows.
- Scope: MVP boundaries, explicit non-goals, nice-to-have items, future phases.
- Existing context: current repo, tech stack, constraints, reusable systems, ownership boundaries.
- UX and behavior: screens, actions, states, permissions, error handling, empty/loading states.
- APIs and integrations: external services, credentials, rate limits, webhooks, data contracts.
- Data and persistence: entities, relationships, lifecycle, retention, import/export, migrations.
- Security and privacy: authentication, authorization, sensitive data, auditability, compliance.
- Operations: deployment target, environments, observability, background jobs, rollback needs.
- Quality: acceptance criteria, test expectations, performance, accessibility, reliability.
- Delivery constraints: deadline, budget, preferred tools, prohibited tools, compatibility needs.

For each batch:

- Group related questions under a short heading.
- Ask no more than 5 questions unless the user explicitly requests a comprehensive checklist.
- Prefer multiple-choice options when they would reduce effort without hiding important tradeoffs.
- Explain assumptions only when they are low impact.
- Do not assume answers for high-impact unknowns.

## Completeness Gate

Before writing the PRD/spec, verify that these statements are answerable:

- What problem is being solved and for whom.
- What is included in the first build and what is excluded.
- What user-visible behavior must exist.
- What data, APIs, integrations, or repo systems are involved.
- What constraints or preferences must shape the product requirements.
- How success will be accepted or tested.

If any statement is not answerable, ask more questions instead of drafting the PRD/spec.

## Output Contract

When clarification is incomplete, output:

```markdown
## Current Understanding
[Brief restatement]

## Open Questions
[Focused question batch]
```

When clarification is complete, output:

```markdown
## PRD / Spec
[Goal, users, requirements, non-goals, UX/API behavior, data/contracts, constraints, risks, acceptance criteria]

## Assumptions
[Only low-impact assumptions]
```

## Guardrails

- Do not ask questions that can be answered by inspecting available files, configs, schemas, types, or docs.
- Do not use a generic checklist when the project details imply more specific questions.
- Do not over-plan low-risk details; record them as assumptions if they will not affect product requirements.
- Keep the final PRD/spec decision-complete enough that product decisions are explicit rather than assumed.

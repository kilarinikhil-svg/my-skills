---
name: plan-project
description: Strict software project clarification workflow. Use when Codex is asked to clarify, scope, specify, or prepare a PRD/spec for a software project, app, feature, product idea, automation, API, website, data pipeline, or repo change; continue using it when the user answers planning questions, adds project details, says to continue planning, or has not yet received the final PRD/spec. Use especially when requirements, non-goals, acceptance criteria, or implementation-impacting choices must be resolved before implementation.
---

# Plan Project

## Operating Rule

Clarify before drafting. Do not produce a PRD/spec until high-impact ambiguity is resolved.

Explore before asking. Use the conversation and available repo/context to answer discoverable questions, especially when the user references an existing project, codebase, API, data model, or deployment target.

Continue across turns. Treat answers, corrections, option selections, "continue", "next", and added details as continuation input until the PRD/spec is produced or the user explicitly cancels, switches away from planning, or requests another workflow.

## Workflow

1. Determine whether this is a new planning request or a continuation.
2. Incorporate the user's latest input before asking anything new.
3. Briefly restate the project idea when starting or when understanding changes materially.
4. Inspect local context when it can answer technical, scope, integration, or constraint questions.
5. Identify unknowns that affect requirements, UX, data/contracts, security, operations, delivery constraints, or acceptance criteria.
6. Ask only material questions, in focused batches, until the requirements are decision-complete.
7. If the user asks to implement before the completeness gate passes, state the missing decision briefly and ask the next focused question batch.
8. When complete, output the standalone PRD/spec and stop. Do not ask whether to implement it.

## Questioning

Use these categories to generate specific questions, not as a checklist: goal and users, MVP scope and non-goals, current repo/stack, UX and behavior, APIs and integrations, data and persistence, security and privacy, operations, quality, delivery constraints.

For each batch:

- Ask no more than 5 questions unless the user requests a comprehensive checklist.
- Group related questions under a short heading.
- Prefer selectable choices when they reduce effort without hiding tradeoffs.
- Do not ask questions that inspection can answer.
- Do not assume high-impact answers; record only low-impact defaults as assumptions.

When `request_user_input` is available, use it for selectable clarification:

- Ask 1-3 short questions per call.
- Provide 2-3 mutually exclusive, meaningful options.
- Put the recommended/default option first and suffix its label with `(Recommended)`.
- Do not add an `Other` option; the client provides it.
- Use `autoResolutionMs` only when the choice is helpful but non-blocking.

When `request_user_input` is unavailable, use concise Markdown questions. Use free text only when the answer space is genuinely open-ended or domain-specific.

## Completeness Gate

Before writing the PRD/spec, verify that these are answerable:

- What problem is being solved and for whom.
- What is included in the first build and what is excluded.
- What user-visible behavior must exist.
- What data, APIs, integrations, or repo systems are involved.
- What constraints or preferences shape the requirements.
- How success will be accepted or tested.

If any item is not answerable, ask more questions instead of drafting.

## Output

When clarification is incomplete and `request_user_input` is available, provide only brief context needed for the user, then call `request_user_input`. Do not include a Markdown option list in the same response.

When clarification is incomplete and `request_user_input` is unavailable, use:

```markdown
## Current Understanding
[Brief restatement]

## Clarified So Far
[Durable decisions already answered, if any]

## Open Questions
[Focused question batch]
```

When clarification is complete, end the response with:

```markdown
# PRD / Spec
[Goal, users, requirements, non-goals, UX/API behavior, data/contracts, constraints, risks, acceptance criteria]

## Assumptions
[Only low-impact assumptions]
```

Do not append implementation offers, approval prompts, next-step commands, or extra questions after the final PRD/spec unless the user explicitly requested them.

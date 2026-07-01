---
name: plan-project
description: Strict software project clarification workflow. Use when Codex is asked to clarify, scope, specify, or prepare a PRD/spec for a software project, app, feature, product idea, automation, API, website, data pipeline, or repo change; continue using it when the user answers prior planning questions, provides more project details, says to continue planning, or has not yet received the final PRD/spec. Especially use when relevant questions must be asked before implementation, requirements clarified, non-goals captured, acceptance criteria defined, or selectable planning options presented instead of open-ended prompts.
---

# Plan Project

## Core Rule

Do not produce a PRD/spec until high-impact ambiguity has been resolved.

First discover what can be learned from the conversation and available environment. Then ask the missing questions needed to make the project spec complete. Ask in focused batches instead of dumping a long questionnaire.

Once this workflow has started, remain in planning mode across turns until the PRD/spec is produced or the user explicitly cancels, switches to implementation, or asks to use a different skill. Treat short answers, option selections, corrections, and added details as continuation input for the same clarification loop.

## Workflow

1. Determine whether this is a new planning request or a continuation of an active clarification loop.
2. For a continuation, incorporate the user's latest answers into the current understanding before asking anything new.
3. Restate the current project idea in one or two sentences when starting or when the understanding materially changes.
4. Inspect available repo/context when the user references an existing project or when local context could answer technical questions.
5. Identify unknowns that would change the product requirements, user experience, data/contracts, constraints, security, delivery expectations, or acceptance criteria.
6. Ask only questions whose answers materially affect the PRD/spec.
7. Continue asking until the project requirements are clear and decision-complete.
8. Produce the PRD/spec and stop.

## Continuation Rules

- If the previous assistant message asked planning questions and the user answers them, continue this skill even if the user does not mention `$plan-project` again.
- If the user says "continue", "next", "here are answers", "that sounds right", or gives partial project details before a PRD/spec exists, treat it as continuation input.
- Do not enter implementation, execution loops, code editing, or vertical-slice planning until the PRD/spec has been produced or the user explicitly overrides the planning workflow.
- If the user asks to implement before the Completeness Gate passes, explain the missing planning decision briefly and ask the next focused question batch.
- If the user explicitly switches away from planning, stop this workflow and follow the newer instruction.

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
- Prefer selectable multiple-choice options when they would reduce effort without hiding important tradeoffs.
- Explain assumptions only when they are low impact.
- Do not assume answers for high-impact unknowns.

## Selection UX

Use selection-style clarification by default when a question has a small set of realistic answers.

- If the current Codex surface exposes an allowed native option-selection/user-input tool, use it for up to 3 short questions at a time.
- If no native selection tool is available, present numbered choices in Markdown and ask the user to reply with numbers, short labels, or corrections.
- Include 2-4 options per question, and make the recommended/default option first only when there is a defensible default.
- Always include a way for the user to provide a custom answer when the listed options may not fit.
- Use free-text questions only when the answer space is genuinely open-ended or the user needs to describe domain-specific details.
- Treat numeric replies, labels, and short confirmations as valid continuation input.

Format fallback selection prompts like this:

```markdown
## Open Questions

### Scope
1. What should the MVP include?
   - A. [Recommended option, if any]
   - B. [Alternative]
   - C. [Alternative]
   - Custom: [Ask for a brief description]
```

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

## Clarified So Far
[Short bullets for durable decisions already answered, if any]

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

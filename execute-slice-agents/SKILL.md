---
name: execute-slice-agents
description: Goal-driven execution coordinator for approved Markdown implementation plans organized into vertical slices. Use when Codex is asked to run, execute, or continue a build plan produced by build-vertical-slice; use /goal or goal tooling; spawn multiple subagents; parallelize independent slices; or coordinate worker agents for disjoint vertical-slice development after user approval.
---

# Execute Slice Agents

## Core Rule

Execute only an approved vertical-slice implementation plan. Use goal tooling and worker subagents when available, but keep one coordinator responsible for dependency ordering, ownership boundaries, integration, validation, and final completion.

Do not start implementation from a plan that still has unresolved sequencing, acceptance, or dependency questions. Do not spawn workers until the user has approved the implementation Markdown or explicitly says to run it.

## Expected Input

The input should be a Markdown implementation plan from `build-vertical-slice` with:

- Goal and approach
- Numbered or named slices
- `Depends on` for every slice, using `None` when independent
- Scope, affected areas, acceptance criteria, and tests/checks for every slice
- Dependency map or enough dependency detail to derive execution batches
- Review gate or clear user approval to proceed

If this structure is missing, ask for the missing plan details instead of executing.

## Workflow

1. Read the implementation plan and any referenced project files needed to understand ownership.
2. Confirm user approval exists. If the plan contains a review gate and approval is absent, ask for approval before spawning agents.
3. Start or reuse a Codex goal when goal tooling is available. If only slash commands are available, provide a ready-to-run `/goal` prompt and stop.
4. Build a slice dependency graph from each slice's `Depends on` field.
5. Identify ready slices whose dependencies are already complete.
6. Group independent ready slices into batches, with at most 3 worker agents by default unless the user specifies a different limit.
7. Assign each worker exactly one slice with explicit ownership and validation instructions.
8. While workers run, perform only non-overlapping coordinator work such as reviewing shared context, preparing integration checks, or inspecting untouched areas.
9. Review each worker result, changed files, and validation notes before accepting the work.
10. Integrate the batch, resolve conflicts, and run the relevant tests/checks for that batch.
11. Mark slices complete only after integration and validation pass.
12. Continue batching ready slices until all required slices are complete or a blocker prevents progress.
13. Complete the goal when every required slice is integrated and validated.

## Goal Handling

When goal tools are available:

- Create a goal with an objective that names the implementation plan and completion criteria.
- Keep goal progress aligned to validated slice completion, not worker completion alone.
- Mark the goal complete only after all required slices pass validation.
- Mark the goal blocked only when a real blocker prevents meaningful progress and cannot be resolved locally.

When goal tools are unavailable:

- Output a concise `/goal` prompt that includes the plan path or pasted plan, the default worker limit, approval status, and the coordinator rules.
- Do not claim that a goal has started.

## Worker Assignment Rules

Spawn workers only for slices that can proceed independently.

Each worker prompt must include:

- The slice name/number and outcome
- The relevant plan excerpt
- Dependencies already satisfied
- Explicit file, module, or responsibility ownership when discoverable
- Acceptance criteria and tests/checks for that slice
- Instruction to edit directly in its workspace when asked to implement
- Instruction that other workers may be editing the codebase in parallel
- Instruction not to revert, overwrite, or refactor unrelated work
- Required final report: changed files, validation run, failures, risks, and any handoff notes

Prefer worker agents for implementation tasks. Use explorer agents only when ownership or dependency safety cannot be determined from the plan and repository inspection.

## Ownership And Parallelism

- Default to a maximum of 3 worker agents per batch.
- Do not assign two workers overlapping files, migrations, generated artifacts, shared schemas, public APIs, or package/dependency manifests in the same batch unless one is explicitly read-only.
- If two ready slices overlap in likely write scope, run them sequentially or split ownership before spawning workers.
- Keep cross-cutting work with the first slice that needs it; do not create an unowned cleanup phase.
- Preserve user changes and other workers' changes.

## Integration Rules

The coordinator owns final integration.

- Inspect worker summaries before accepting changes.
- Review changed files enough to catch scope drift, overlapping ownership, and missing acceptance criteria.
- Run the slice's listed checks plus any relevant repo checks discovered locally.
- Fix integration issues caused by the current batch before starting the next batch.
- If a worker partially completes a slice, decide whether to finish it locally, retry with a narrower worker prompt, or report a blocker.
- Do not start dependent slices until dependency slices are integrated and validated.

## Failure Handling

- If the plan lacks clear dependencies, ask for a corrected plan or derive a conservative sequential order and state the assumption.
- If approval is missing, ask for approval and stop.
- If goal tooling is missing, provide a `/goal` fallback prompt.
- If worker changes conflict, resolve the conflict in the coordinator or rerun the smaller conflicting slice after integration.
- If validation fails, diagnose and fix current-batch failures before continuing.
- If the blocker is external, report the exact blocker, what was tried, and what user action is required.

## Output Contract

When starting:

```markdown
## Execution Start
- Plan:
- Goal:
- Worker limit:
- First batch:
- Approval:
```

After each batch:

```markdown
## Batch Completed
- Slices:
- Workers:
- Integrated changes:
- Validation:
- Next ready slices:
- Blockers:
```

When goal tools are unavailable:

```markdown
## Manual Goal Prompt
> /goal [ready-to-run prompt]
```

When complete:

```markdown
## Goal Complete
- Completed slices:
- Validation summary:
- Remaining risks:
```

When blocked:

```markdown
## Blocked
- Current slice or batch:
- Blocking issue:
- What was tried:
- Required action:
```

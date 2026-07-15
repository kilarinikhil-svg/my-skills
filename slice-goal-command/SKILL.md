---
name: slice-goal-command
description: Create one paste-ready `/goal` command from an approved Markdown vertical-slice implementation plan. Use when a user wants to hand off a `build-vertical-slice` plan file to `$execute-slice-agents` without starting execution.
---

# Slice Goal Command

## Workflow

1. Identify the Markdown implementation-plan file path and an optional worker limit. Default the limit to `3`; require an integer of at least `1` when the user supplies an override.
2. Resolve the supplied path to an absolute path, confirm that it is a readable Markdown file, and read it. Do not generate a command until this succeeds.
3. Validate the plan before handoff. Require all of the following:
   - A `Goal` section and an `Approach` section.
   - A `Slices` section containing named or numbered slice headings.
   - For every slice: `Depends on`, `Scope`, `Affected areas`, `Acceptance criteria`, and `Tests/checks` fields with non-empty values. Treat `Depends on: None` as valid.
4. If validation fails, do not emit a `/goal` command. Identify every missing or empty section, slice, and field, then request a corrected plan file.
5. Treat a valid supplied plan as approved. Do not ask for a separate review gate or approval confirmation.
6. Output exactly one plain-text `/goal` command and nothing else. Use the resolved absolute plan path and the selected worker limit. Instruct the receiving goal to use `$execute-slice-agents`, preserve dependency ordering and worker ownership boundaries, keep coordinator-owned integration, run the plan's validation, and complete only after all slices are integrated and validated.
7. Do not invoke goal tooling, spawn workers, or start implementation. Produce only the handoff command.

## Output Template

Replace the bracketed values and emit the command on one line:

`/goal Execute the approved vertical-slice implementation plan at "[absolute-plan-path]" using $execute-slice-agents with a maximum of [worker-limit] workers. Treat the plan as approved; preserve slice dependency ordering and worker ownership boundaries, keep coordinator-owned integration, run every listed test/check, and complete only after every slice is integrated and validated.`

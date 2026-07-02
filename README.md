# Codex Skills

This folder contains local Codex skills. Each skill lives in its own directory with a `SKILL.md` file that defines when the skill should be used and the workflow Codex should follow.

## Skills

- `plan-project`: Clarifies and scopes a software project, feature, app, automation, API, website, data pipeline, or repository change before drafting a PRD/spec.
- `build-vertical-slice`: Converts an approved PRD/spec into a Markdown implementation plan organized as tested vertical slices.
- `execute-slice-agents`: Coordinates implementation of an approved vertical-slice plan, including dependency ordering, worker assignment, integration, and validation.

## Suggested Workflow

1. Use `plan-project` to clarify requirements and produce a standalone PRD/spec.
2. Use `build-vertical-slice` to turn the PRD/spec into an implementation plan.
3. Use `execute-slice-agents` after the implementation plan has been approved.

## Folder Layout

```text
.
|-- plan-project/
|   `-- SKILL.md
|-- build-vertical-slice/
|   `-- SKILL.md
|-- execute-slice-agents/
|   `-- SKILL.md
|-- .system/
|-- .agents/
`-- .gitignore
```

## Skill Authoring Notes

- Keep each skill focused on one repeatable workflow.
- Put the trigger conditions in the frontmatter `description`.
- Make the workflow explicit enough that Codex can follow it without guessing.
- Include output contracts when the skill should produce a specific format.
- Avoid mixing planning-only and implementation behavior in the same skill unless the skill explicitly coordinates both.

## Local Metadata

- `.system/` contains system-provided skills and should generally be treated as managed content.
- `.agents/` is local agent metadata.
- `__pycache__/` and compiled Python files are ignored by `.gitignore`.

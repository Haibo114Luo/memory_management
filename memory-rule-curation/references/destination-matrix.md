# Destination Matrix

Use this matrix only after deciding what should be remembered and whether the memory is global or task-specific.

## Scope first

- Keep the existing global structure unchanged for machine-wide guidance.
- Use `task-specific` only for memory that belongs to the current Codex conversation's project.
- Default to the current working directory as the target root only when it obviously looks like a project root, such as when it contains `.git`, `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or another clear project manifest.
- If the current working directory does not obviously look like a project root, do not auto-create task-specific `memories`. Ask the user to confirm the target project root first.
- When the scope is `task-specific`, create `memories` at the chosen project root if it does not already exist, then mirror the same three layers inside that project-local folder:
  - `memories\short_term_memory`
  - `memories\long_term_memory`
  - `memories\rules`

## Minimal entry formats

Use the same minimal format for both short-term memory and long-term memory:

- `Date`
- `Source Type`
- `Trigger`
- `Memory`
- `Why`

Use this minimal format for curated rules:

- `Date`
- `Source Type`
- `Trigger`
- `Rule`
- `Why`

Rules for all formats:

- Keep each entry focused on one point only.
- Keep entries compact and execution-oriented.
- Use the same format for task-specific memory; only the destination path changes.
- Prefer `Rule` for curated guidance and `Memory` for candidate or archival material.
- Do not add extra fields by default. Add one only when the current task genuinely needs it.

## Send to `short_term_memory`

Choose `D:\CodexData\memories\short_term_memory\rule-inbox.md` when:

- the rule is still a candidate
- the information came from a fresh correction or one-off mistake
- the correct long-term destination is not settled yet
- the note is operationally interesting but not yet stable enough for curated rules

Use the shared memory-entry format above.
Use `Source Type` values such as `user correction`, `failure case`, `review feedback`, or `other`.
If the current task needs a routing hint, add `Suggested Destination` as an extra field. Do not add it by default.

## Send to `rules`

Choose `D:\CodexData\memories\rules` when:

- the rule is stable and repeatedly useful
- the guidance should shape future tasks on this machine
- the content is execution-oriented rather than historical
- the rule fits an existing topic file or justifies a new coherent topic file

Keep `index.md` short and use it as the entry point. Put detailed operational content in topic files.
Default to candidate-first handling: promote only after at least two independent triggers unless the rule is a low-dispute machine/environment or structure rule that is clearly stable enough for direct promotion.
For style, workflow, abstraction, or governance rules, ask the user before promotion even when the recurrence threshold has been met.
Prefer reusing an existing topic file. Create a new topic file only when the new area has a clear boundary and is likely to grow.

## Send to `long_term_memory`

Choose `D:\CodexData\memories\long_term_memory` when:

- the material is mainly archival or investigative
- it is useful for future recall but not something that should load by default
- the content explains history, context, evidence, or background rather than current execution rules

Use the shared memory-entry format above.

## Send to `AGENTS.md`

Update `D:\CodexData\AGENTS.md` only when the change belongs in the active prompt layer:

- a short always-on trigger
- a scope or path rule
- a small entry-point instruction that points to this skill or to the curated rules index

Do not move detailed memory workflow, inbox formatting guidance, or long-form rule content into `AGENTS.md`.

## User-facing write summary

When any write happens to short-term memory, long-term memory, or rules, add a lightweight summary line at the end of the reply:

`Memory update triggered: X writes total, including X short-term memory / X long-term memory / X rules.`

If no memory write happened, do not print this line.

## Conflict Handling

Treat conflicts across all three layers as blocking:

- `D:\CodexData\AGENTS.md`
- existing files under `D:\CodexData\memories\rules`
- the candidate or proposed rule being considered

When a conflict appears:

1. Append a conflict entry to `D:\CodexData\memories\short_term_memory\rule-inbox.md`.
2. Do not auto-resolve, auto-overwrite, or silently choose a winner.
3. Ask the user to decide which rule should win or how the rules should be rewritten.

For task-specific memory, apply the same conflict rule inside the project-local `memories` tree only:

- project-local `memories\rules`
- project-local `memories\short_term_memory`
- the task-specific candidate or proposed rule being considered

When a task-specific conflict appears:

1. Append the conflict entry to the project-local `memories\short_term_memory\rule-inbox.md`.
2. Do not auto-resolve, auto-overwrite, or silently choose a winner.
3. Ask the user to decide which project-local rule should win or how the project-local rules should be rewritten.

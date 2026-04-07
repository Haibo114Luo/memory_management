---
name: memory-rule-curation
description: Use when Codex is handling corrections, failures, review feedback, memory or rule curation, task-specific project memory, or updates that keep `D:\CodexData\AGENTS.md` concise instead of turning it into a detailed memory log.
---

# Memory Rule Curation

## Overview

Use this skill to decide what should be remembered, how it should be remembered, and only then where it should live. Keep the existing global split between active rules, archived memory, and candidate rule intake. Add a task-specific layer only when the memory applies to the current project rather than to the machine-wide defaults. Keep `D:\CodexData\AGENTS.md` small, keep operational rules under `D:\CodexData\memories\rules`, and keep historical or low-frequency material out of the active instruction layer.

## Quick Start

1. Read `D:\CodexData\AGENTS.md`, especially the task-approach and memory sections.
2. Read `D:\CodexData\memories\rules\index.md` for the current operational rule entry points.
3. Read [references/memory-layout.md](references/memory-layout.md) to confirm the expected directory structure.
4. Read [references/destination-matrix.md](references/destination-matrix.md) after deciding what should be captured and whether the memory is global or task-specific. Use the minimal entry formats there instead of inventing ad-hoc note structures.
5. Make the smallest set of edits that keeps the structure consistent and the prompt layer clean.

## Core Rules

- Keep `D:\CodexData\AGENTS.md` as a compact rules entry layer. Put triggers, scope, and a small number of always-on constraints there. Do not turn it into a decision log or a rule archive.
- Treat `D:\CodexData\memories\short_term_memory` as an inbox for candidate rules and temporary memory-management notes. It is not a default authoritative source.
- Treat `D:\CodexData\memories\rules` as curated operational guidance that can be loaded for relevant tasks.
- Treat `D:\CodexData\memories\long_term_memory` as archival reference material that is only searched when the user explicitly asks to recall or search memory.
- When this skill writes to `short_term_memory`, `long_term_memory`, `rules`, or `AGENTS.md`, write the file content in English. If the source material is in another language, translate or paraphrase it into concise English instead of copying non-English prose into the write target.
- Preserve the existing global layers as-is. Only add a `task-specific` layer when the memory is local to the current Codex conversation's project and should not become machine-wide guidance.
- Do not use `D:\CodexData\rules\default.rules` for memory or instruction curation. That path is reserved for other runtime rule machinery.
- Default to explicit-signal triggering: use this skill when the user mentions memory, rules, inbox, curation, or when the task is clearly about memory/rule-system maintenance even without those exact words.
- When a write happens to short-term memory, long-term memory, or rules, add a one-line summary at the end of the user-facing reply in the form `Memory update triggered: X writes total, including X short-term memory / X long-term memory / X rules.`

## Workflow

### 1. Identify the source

- Label the intake source before deciding anything else: `user correction`, `failure case`, `review feedback`, or `other`.
- Preserve the source type in candidate entries so later promotion decisions still carry context.
- If the source is unclear, state the uncertainty instead of inventing one.

### 2. Decide what to remember and how to remember it

- Determine whether the new information is a candidate rule, a stable operational rule, an archival reference, or a structural change to the memory system itself.
- If the information is a one-off correction, a fresh observation, or still needs judgment, treat it as a candidate first.
- If the information is stable, repeatedly useful, and should guide future work, treat it as an operational rule.
- If the information is mainly historical context, investigation notes, or low-frequency background, treat it as archival memory.
- Write the memory as compact, execution-oriented guidance. Do not start from the filesystem layout and work backward into meaning.
- Use the smallest entry format that still preserves meaning. Do not create custom fields unless the current task clearly needs them.

### 3. Decide the scope

- Keep the existing global structure unchanged for machine-wide rules, machine environment constraints, and other reusable guidance that should survive across projects.
- Use `task-specific` only when the memory belongs to the current Codex conversation's project and would be misleading or noisy if promoted to the global memory layers.
- Default to the current working directory as the project root for task-specific memory only when it obviously looks like a project root, such as when it contains markers like `.git`, `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or another clear project manifest.
- If the current working directory does not obviously look like a project root, do not auto-create project-local `memories`. Ask the user to confirm the target project root before writing task-specific memory.
- Do not promote task-specific memory into the global layers just because it was recently observed in the current project.
- If the scope is ambiguous, default to candidate-first handling and note the ambiguity in the candidate entry.

### 4. Choose the destination

- For global-scope candidate rules, append uncurated entries to `D:\CodexData\memories\short_term_memory\rule-inbox.md`.
- For global-scope operational rules, write stable guidance into `D:\CodexData\memories\rules\index.md` or the relevant topic file under `D:\CodexData\memories\rules`.
- For global-scope archival references, write into `D:\CodexData\memories\long_term_memory`.
- For task-specific memory, create a `memories` folder at the current project root when it does not already exist, but only after confirming that the current working directory obviously is the project root or the user has confirmed the target root.
- Update `D:\CodexData\AGENTS.md` only when a small always-on trigger, scope rule, or entry-point rule genuinely belongs in the active prompt layer.
- Use topic directories inside long-term memory by default instead of dropping archival files directly at the root.

### 5. Keep `AGENTS.md` small

- If a rule needs more than a short trigger or scope statement, keep the detail in this skill or in `D:\CodexData\memories\rules`.
- Prefer a single clear invocation rule over repeating the same idea in multiple AGENTS sections.
- If an AGENTS change would duplicate existing memory-structure rules, rewrite or consolidate instead of stacking another near-duplicate instruction.

### 6. Maintain the rules set

- Use `D:\CodexData\memories\rules\index.md` as the default entry point.
- Default to candidate-first handling. Do not promote a new candidate from `rule-inbox.md` into curated rules until it has appeared in at least two independent triggers, unless it is a low-dispute machine/environment or structure rule that is clearly stable enough for direct promotion.
- For low-dispute machine/environment or structure rules, direct promotion is allowed. For style, workflow, abstraction, or governance rules, ask the user before promoting even if the recurrence threshold has been met.
- Add a new topic file only when the rule area is coherent, likely to grow, and no existing topic file is the better home. Prefer reusing an existing topic file when the new rule comfortably fits there.
- Keep topic files execution-oriented. Do not paste long debugging timelines or historical narratives into curated rule files.

### 7. Handle conflicts explicitly

- Treat conflicts across all three layers as blocking: `D:\CodexData\AGENTS.md`, existing files under `D:\CodexData\memories\rules`, and the candidate material being considered for promotion.
- If a candidate or proposed rule conflicts with any of those layers, do not auto-merge, auto-overwrite, or silently pick a winner.
- Before asking the user, append a conflict entry to `D:\CodexData\memories\short_term_memory\rule-inbox.md` so the issue is auditable.
- For task-specific memory, apply the same blocking rule inside the project-local `memories` tree only. Check the project-local curated rules, the project-local candidate material, and the task-specific candidate being considered. Do not treat global-layer disagreement as a task-specific conflict check.
- Before asking the user about a task-specific conflict, append the conflict entry to the project-local `memories\short_term_memory\rule-inbox.md` rather than to the global inbox.
- After logging the conflict, stop and ask the user to decide which rule should win or how the rules should be rewritten.

### 8. Verify after edits

- Confirm the directory layout still matches [references/memory-layout.md](references/memory-layout.md).
- Confirm `AGENTS.md` references the correct paths and does not absorb process detail that belongs in this skill.
- Confirm new or modified Markdown files remain UTF-8 without BOM.
- If task-specific memory was written, confirm the project-local `memories` structure exists at the current project root and does not leak into unrelated directories.
- If task-specific memory was written, confirm the chosen root either obviously looked like a project root or was explicitly confirmed by the user.
- If any memory write happened, confirm the user-facing reply ends with the lightweight write summary line.

## Examples

- If the user says "Whenever I correct a recurring environment mistake, capture it for later review," label the source as `user correction` and append a candidate entry to `D:\CodexData\memories\short_term_memory\rule-inbox.md` first.
- If the same environment constraint appears in two independent triggers and should guide future tasks across projects, move or rewrite it into the relevant curated rule file under `D:\CodexData\memories\rules`.
- If the user asks to preserve an investigation writeup for future recall, place it under `D:\CodexData\memories\long_term_memory` instead of the curated rules set.
- If a failure pattern applies only to the current project, create project-local `memories` at that project's root and store it in the task-specific layer rather than polluting the global memory tree.
- If a proposed curated rule conflicts with an existing AGENTS rule or another curated rule, log the conflict in `rule-inbox.md` and ask the user to decide before changing the long-term rule set.

## References

- Read [references/memory-layout.md](references/memory-layout.md) for the current filesystem layout and path semantics.
- Read [references/destination-matrix.md](references/destination-matrix.md) when classifying new material or deciding whether `AGENTS.md` should change.

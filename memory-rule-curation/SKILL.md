---
name: memory-rule-curation
description: Use when Codex is handling corrections, failures, review feedback, memory or rule curation, task-specific project memory, or updates that keep `D:\CodexData\AGENTS.md` concise instead of turning it into a detailed memory log.
---

# Memory Rule Curation

## Overview

Use this skill to decide what should be remembered, how it should be remembered, and only then where it should live. Keep the existing global split between active rules, archived memory, deferred TODO branches, and candidate rule intake. Add a task-specific layer only when the memory applies to the current project rather than to the machine-wide defaults. Keep `D:\CodexData\AGENTS.md` small, keep operational rules under `D:\CodexData\memory_manual\rules`, keep unfinished global work under `D:\CodexData\memory_manual\long_term_memory\TODO`, and keep historical or low-frequency material out of the active instruction layer.

## Quick Start

1. Read `D:\CodexData\AGENTS.md`, especially the task-approach and memory sections.
2. Read `D:\CodexData\memory_manual\rules\index.md` for the current operational rule entry points.
3. Read [references/memory-layout.md](references/memory-layout.md) to confirm the expected directory structure.
4. Read [references/destination-matrix.md](references/destination-matrix.md) after deciding what should be captured and whether the memory is global or task-specific. Follow its destination-specific format discipline: candidates and archival memories may use entry records, but curated rule files must be normative discipline documents, not event logs.
5. Make the smallest set of edits that keeps the structure consistent and the prompt layer clean.

## Core Rules

### Memory source boundary

- When the user explicitly asks to update memory, remember something, record long-term memory, or perform memory curation in this machine workflow, interpret the request as targeting the user-maintained file-based memory repository under `D:\CodexData\memory_manual` and this skill's related files, unless the user explicitly names Codex product Memories, app-level Memories, or another target.
- Treat Codex product Memories and other system-managed memory surfaces as outside this skill's writable manual memory repository. Do not write user-curated memory there.
- Distinguish memory surfaces before refusing or writing: (1) Codex product Memories or app-level automatic memory, (2) a current-session injected memory folder or memory summary, and (3) the user-maintained file-based memory repository under `D:\CodexData\memory_manual`.
- If a higher-priority instruction says "memories" without clearly naming `D:\CodexData\memory_manual`, do not automatically treat it as a ban on the user-maintained file-based memory repository. First scope which memory surface the instruction controls.
- If a higher-priority instruction explicitly forbids writes to `D:\CodexData\memory_manual` or to the exact target file, obey that instruction and report the block. Do not claim this skill can override system or developer instructions.
- Do not update Codex product Memories or system-managed memory from this skill. This skill manages only the user's file-based memory and rule-curation surfaces.
- Keep `D:\CodexData\AGENTS.md` as a compact rules entry layer. Put triggers, scope, and a small number of always-on constraints there. Do not turn it into a decision log or a rule archive.
- Treat `D:\CodexData\memory_manual\short_term_memory` as an inbox for candidate rules and temporary memory-management notes. It is not a default authoritative source.
- Treat `D:\CodexData\memory_manual\rules` as curated operational guidance that can be loaded for relevant tasks. Rule files should describe stable discipline to follow; they must not become one-correction-per-entry stories, debugging timelines, or behavior logs.
- Treat `D:\CodexData\memory_manual\long_term_memory` as archival reference material that is only searched when the user explicitly asks to recall or search memory.
- Treat `D:\CodexData\memory_manual\long_term_memory\TODO` as the reserved location for global unfinished branches, deferred maintenance items, and planned work the user wants resumed later. Keep only still-open work there.
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

- Determine whether the new information is a candidate rule, a stable operational rule, an archival reference, a deferred TODO branch, or a structural change to the memory system itself.
- If the information is a one-off correction, a fresh observation, or still needs judgment, treat it as a candidate first.
- If the information is stable, repeatedly useful, and should guide future work, treat it as an operational rule.
- If the information is mainly historical context, investigation notes, or low-frequency background, treat it as archival memory.
- If the information describes unfinished work that should be resumed later, treat it as TODO memory until the branch is completed.
- Write the memory as compact, execution-oriented guidance. Do not start from the filesystem layout and work backward into meaning.
- Use entry-style records only for candidate, TODO, and archival memory where provenance matters. For curated operational rules, summarize across evidence and write generalized discipline with clear scope, boundaries, and execution instructions. Do not preserve source events one by one inside rule topic files.

### 3. Decide the scope

- Keep the existing global structure unchanged for machine-wide rules, machine environment constraints, and other reusable guidance that should survive across projects.
- Use `task-specific` only when the memory belongs to the current Codex conversation's project and would be misleading or noisy if promoted to the global memory layers.
- Default to the current working directory as the project root for task-specific memory only when it obviously looks like a project root, such as when it contains markers like `.git`, `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, or another clear project manifest.
- If the current working directory does not obviously look like a project root, do not auto-create project-local `memories`. Ask the user to confirm the target project root before writing task-specific memory.
- Do not promote task-specific memory into the global layers just because it was recently observed in the current project.
- If the scope is ambiguous, default to candidate-first handling and note the ambiguity in the candidate entry.

### 4. Choose the destination

- For global-scope candidate rules, append uncurated entries to `D:\CodexData\memory_manual\short_term_memory\rule-inbox.md`.
- For global-scope operational rules, write stable guidance into `D:\CodexData\memory_manual\rules\index.md` or the relevant topic file under `D:\CodexData\memory_manual\rules`; rewrite the durable point as rule discipline rather than copying the intake history.
- For global-scope unfinished branches or deferred work, write into `D:\CodexData\memory_manual\long_term_memory\TODO`.
- For global-scope archival references, write into an appropriate root-level topic file under `D:\CodexData\memory_manual\long_term_memory`; use a subdirectory only for indexed multi-file collections or domains clearly expected to grow.
- For task-specific memory, create a `memories` folder at the current project root when it does not already exist, but only after confirming that the current working directory obviously is the project root or the user has confirmed the target root.
- Update `D:\CodexData\AGENTS.md` only when a small always-on trigger, scope rule, or entry-point rule genuinely belongs in the active prompt layer.
- Use root-level topic files inside long-term memory by default for singleton or low-frequency archival memory. Create subdirectories only for live queues, debug/incident logs, indexed multi-file collections, or domains clearly expected to grow. Update `D:\CodexData\memory_manual\long_term_memory\index.md` whenever adding, renaming, or removing root-level topic files or special subdirectories.

### 5. Keep `AGENTS.md` small

- If a rule needs more than a short trigger or scope statement, keep the detail in this skill or in `D:\CodexData\memory_manual\rules`.
- Prefer a single clear invocation rule over repeating the same idea in multiple AGENTS sections.
- If an AGENTS change would duplicate existing memory-structure rules, rewrite or consolidate instead of stacking another near-duplicate instruction.

### 6. Maintain the rules set

- Use `D:\CodexData\memory_manual\rules\index.md` as the default entry point.
- Default to candidate-first handling. Do not promote a new candidate from `rule-inbox.md` into curated rules until it has appeared in at least two independent triggers, unless it is a low-dispute machine/environment or structure rule that is clearly stable enough for direct promotion.
- For low-dispute machine/environment or structure rules, direct promotion is allowed. For style, workflow, abstraction, or governance rules, ask the user before promotion even if the recurrence threshold has been met.
- Add a new topic file only when the rule area is coherent, likely to grow, and no existing topic file is the better home. Prefer reusing an existing topic file when the new rule comfortably fits there.
- Keep topic files execution-oriented and low-burden for future agents. Do not paste long debugging timelines, historical narratives, or one-by-one user-correction records into curated rule files. A rule file should make the underlying discipline obvious without requiring the model to re-interpret the story that produced it.

### 7. Handle conflicts explicitly

- Treat conflicts across all three layers as blocking: `D:\CodexData\AGENTS.md`, existing files under `D:\CodexData\memory_manual\rules`, and the candidate material being considered for promotion.
- If a candidate or proposed rule conflicts with any of those layers, do not auto-merge, auto-overwrite, or silently pick a winner.
- Before asking the user, append a conflict entry to `D:\CodexData\memory_manual\short_term_memory\rule-inbox.md` so the issue is auditable.
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

- If the user says "Whenever I correct a recurring environment mistake, capture it for later review," label the source as `user correction` and append a candidate entry to `D:\CodexData\memory_manual\short_term_memory\rule-inbox.md` first.
- If the same environment constraint appears in two independent triggers and should guide future tasks across projects, move or rewrite it into the relevant curated rule file under `D:\CodexData\memory_manual\rules`.
- If the user asks to preserve an investigation writeup for future recall, place it in the appropriate root-level topic file in `D:\CodexData\memory_manual\long_term_memory`, or create a new root-level `*.md` topic file and update `long_term_memory\index.md`; use a subdirectory only if the topic is an indexed multi-file collection.
- If the user asks to preserve a deferred maintenance stream or unfinished branch for later continuation, place it under `D:\CodexData\memory_manual\long_term_memory\TODO` until the work is completed.
- If a failure pattern applies only to the current project, create project-local `memories` at that project's root and store it in the task-specific layer rather than polluting the global memory tree.
- If a proposed curated rule conflicts with an existing AGENTS rule or another curated rule, log the conflict in `rule-inbox.md` and ask the user to decide before changing the long-term rule set.

## References

- Read [references/memory-layout.md](references/memory-layout.md) for the current filesystem layout and path semantics.
- Read [references/destination-matrix.md](references/destination-matrix.md) when classifying new material or deciding whether `AGENTS.md` should change.
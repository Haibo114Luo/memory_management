# Memory Layout

This skill manages the memory and rules structure rooted at `D:\CodexData\memory_manual`.

Keep this global structure unchanged. Add a project-local `task-specific` memory layer only when the captured memory belongs to the current project rather than to the machine-wide defaults.

## Active Structure

```text
D:\CodexData\memory_manual\
  long_term_memory\
    index.md
    machine-environment.md
    codex-runtime-governance.md
    workflow-patterns.md
    TODO\
      index.md
    debug_history\
      index.md
      <one Markdown file per debugging incident>
  short_term_memory\
    rule-inbox.md
  rules\
    index.md
    project-start-workflows.md
    project-workspace-governance.md
    codex-harness-governance.md
    python-runtime.md
    windows-shell-encoding.md
```

## Task-Specific Structure

When a memory applies only to the current Codex conversation's project, create this structure at that project's root:

```text
<project-root>\
  memories\
    long_term_memory\
    short_term_memory\
      rule-inbox.md
    rules\
      index.md
```

Use the project-local structure only for task-specific material. Do not create it for machine-wide rules that should remain in `D:\CodexData\memory_manual`.
Default to the current working directory only when it obviously looks like the project root. Otherwise ask the user to confirm the target root before creating project-local `memories`.

## Path Semantics

- `D:\CodexData\memory_manual\long_term_memory` stores archival, low-frequency reference material that should only be searched when the user explicitly asks to recall or search memory.
- `D:\CodexData\memory_manual\long_term_memory\TODO` stores global unfinished branches, deferred maintenance streams, and planned work the user wants resumed later.
- Organize singleton or low-frequency archival material as root-level topic files under `D:\CodexData\memory_manual\long_term_memory`, and update `long_term_memory\index.md` whenever those topic files change. Use subdirectories only for live queues such as `TODO`, debug/incident logs such as `debug_history`, indexed multi-file collections, or domains clearly expected to grow.
- `D:\CodexData\memory_manual\short_term_memory` stores candidate rules and temporary memory-management notes that still require curation.
- `D:\CodexData\memory_manual\rules` stores curated operational guidance that may be read for relevant tasks.
- `D:\CodexData\memory_manual\rules\index.md` is the default entry point for the curated rules set.
- `<project-root>\memories` mirrors the same split for task-specific material and should be created only when task-specific memory is actually needed.

## Related Files

- `D:\CodexData\AGENTS.md` remains the active top-level instruction layer and should stay concise.
- `D:\CodexData\rules\default.rules` is unrelated to memory curation and must not be reused for this workflow.
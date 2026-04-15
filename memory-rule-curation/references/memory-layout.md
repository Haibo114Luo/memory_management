# Memory Layout

This skill manages the memory and rules structure rooted at `D:\CodexData\memories`.

Keep this global structure unchanged. Add a project-local `task-specific` memory layer only when the captured memory belongs to the current project rather than to the machine-wide defaults.

## Active Structure

```text
D:\CodexData\memories\
  long_term_memory\
    TODO\
      index.md
    debug_history\
      ai-readable-debug-log.md
  short_term_memory\
    rule-inbox.md
  rules\
    index.md
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

Use the project-local structure only for task-specific material. Do not create it for machine-wide rules that should remain in `D:\CodexData\memories`.
Default to the current working directory only when it obviously looks like the project root. Otherwise ask the user to confirm the target root before creating project-local `memories`.

## Path Semantics

- `D:\CodexData\memories\long_term_memory` stores archival, low-frequency reference material that should only be searched when the user explicitly asks to recall or search memory.
- `D:\CodexData\memories\long_term_memory\TODO` stores global unfinished branches, deferred maintenance streams, and planned work the user wants resumed later.
- Organize completed archival material inside topic directories under `D:\CodexData\memories\long_term_memory`, such as `debug_history`, instead of treating the root as the main filing surface.
- `D:\CodexData\memories\short_term_memory` stores candidate rules and temporary memory-management notes that still require curation.
- `D:\CodexData\memories\rules` stores curated operational guidance that may be read for relevant tasks.
- `D:\CodexData\memories\rules\index.md` is the default entry point for the curated rules set.
- `<project-root>\memories` mirrors the same split for task-specific material and should be created only when task-specific memory is actually needed.

## Related Files

- `D:\CodexData\AGENTS.md` remains the active top-level instruction layer and should stay concise.
- `D:\CodexData\rules\default.rules` is unrelated to memory curation and must not be reused for this workflow.
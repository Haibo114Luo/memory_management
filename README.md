# memory_management

## 中文说明

这个仓库目前主要用于托管 `memory-rule-curation` 这个 skill。

### 这个 SKILL 实现了什么

`memory-rule-curation` 用来帮助 Agent 处理“记忆 / 规则整理”这一类任务。它的核心目标不是机械记录，而是先判断什么内容值得记住、应该如何记忆，再决定它属于全局记忆还是当前项目的 `task-specific` 记忆。

这个 skill 当前实现了以下能力：

- 从 `user correction`、`failure case`、`review feedback` 等来源识别候选记忆或候选规则。
- 先判断“写什么、怎么记”，再判断 scope，最后才决定写入位置。
- 保留现有全局记忆层结构，同时补充 `task-specific` 项目级记忆层。
- 为短期记忆、长期记忆、规则提供最小条目格式，减少随意记录造成的漂移。
- 在发生实际记忆写入时，要求在回答结尾输出一条轻量摘要提示。
- 对全局记忆和项目内 `memories` 分别执行冲突检查，避免静默覆盖。

### 它是如何实现的

这个 skill 由以下几部分组成：

- [`memory-rule-curation/SKILL.md`](./memory-rule-curation/SKILL.md)
  负责定义 skill 的触发条件、总体原则、工作流和冲突处理规则。
- [`memory-rule-curation/references/destination-matrix.md`](./memory-rule-curation/references/destination-matrix.md)
  负责定义“什么内容该写到哪里”、最小条目格式，以及 task-specific 与全局层的分流规则。
- [`memory-rule-curation/references/memory-layout.md`](./memory-rule-curation/references/memory-layout.md)
  负责定义全局记忆目录和项目内 `memories/` 目录的结构语义。
- [`memory-rule-curation/agents/openai.yaml`](./memory-rule-curation/agents/openai.yaml)
  提供 UI 侧的显示名称、简介和默认 prompt。

### 冲突与修复说明

这个 skill 曾经暴露出一次与运行时发现机制有关的路径冲突：真实目录始终位于 `D:\CodexData\skills\memory-rule-curation`，但运行时一度把它暴露成了错误路径 `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation\SKILL.md`。

这里最重要的事实边界是：

- `memory-rule-curation` 不是一开始就创建在 `superpowers` 子目录里。
- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation` 这个 junction 是排障时后加进去的兼容补丁，不是冲突的根因。
- 根据调试记录，能确认的根因边界只是“运行时 / 会话层暴露了错误路径”，而不是某个已定位、可编辑的本地注册表项。

这个问题的完整前因后果、调查过程、根因判断边界、兼容修复和后续清理步骤已经单独写入：

- [`CONFLICT.md`](./CONFLICT.md)

当前公开文档采用匿名化路径示例，并以 `UserName` 代替真实用户名。

### 下载与使用说明

你可以直接下载整个仓库，或者只取 `memory-rule-curation/` 目录下的 skill 内容。

需要注意：这个 skill 当前包含面向我本机环境的硬编码路径，例如 `D:\CodexData`。如果你要在自己的 Agent app 中使用，安装时需要把这些路径替换成你自己的 Agent app 根目录。

例如：

- `D:\CodexData\AGENTS.md` 应替换为你的 Agent app 根目录下的 `AGENTS.md`
- `D:\CodexData\memories` 应替换为你的 Agent app 根目录下的 `memories`
- 其他以 `D:\CodexData` 开头的路径也应按同样方式替换

如果你的 Agent app 根目录不是 Windows 路径，或者目录结构不同，你还需要同步调整 references 中的目录示例和相关说明。

### 当前状态

- 这个 skill 已经适合我自己的环境中使用和继续迭代。
- 它已发布到 GitHub 以便版本管理和分享。
- 路径冲突问题已有文档化记录和修复说明，见 [`CONFLICT.md`](./CONFLICT.md)。
- 公开文档中的个人用户名已统一匿名化为 `UserName`。
- 它还没有被彻底泛化成“别人安装后即可直接使用”的通用版本。

## English

This repository currently hosts the `memory-rule-curation` skill.

### What this skill implements

`memory-rule-curation` helps an Agent handle memory and rule curation tasks. Its core goal is not blind logging. Instead, it first decides what is worth remembering and how it should be remembered, then decides whether the content belongs to the global memory layers or to a task-specific project memory layer.

The skill currently provides the following behavior:

- It identifies candidate memories or candidate rules from sources such as `user correction`, `failure case`, and `review feedback`.
- It decides what to write and how to remember it before deciding scope and destination.
- It preserves the existing global memory-layer structure while adding a `task-specific` project-level memory layer.
- It defines minimal entry formats for short-term memory, long-term memory, and rules to reduce drift from ad-hoc note taking.
- It requires a lightweight write-summary line at the end of the reply whenever an actual memory write happens.
- It applies conflict checks separately inside the global memory layers and inside project-local `memories`, preventing silent overwrites.

### How it is implemented

This skill is implemented through the following files:

- [`memory-rule-curation/SKILL.md`](./memory-rule-curation/SKILL.md)
  Defines the triggering conditions, core principles, workflow, and conflict-handling rules.
- [`memory-rule-curation/references/destination-matrix.md`](./memory-rule-curation/references/destination-matrix.md)
  Defines routing rules, minimal entry formats, and the split between task-specific and global memory.
- [`memory-rule-curation/references/memory-layout.md`](./memory-rule-curation/references/memory-layout.md)
  Defines the meaning of the global memory layout and the project-local `memories/` layout.
- [`memory-rule-curation/agents/openai.yaml`](./memory-rule-curation/agents/openai.yaml)
  Provides UI-facing metadata such as display name, short description, and default prompt.

### Conflict and remediation note

This skill previously exposed a runtime discovery-path conflict. The real skill directory always lived at `D:\CodexData\skills\memory-rule-curation`, but the runtime temporarily surfaced the wrong path `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation\SKILL.md`.

The important fact boundaries are:

- `memory-rule-curation` was not originally created inside the `superpowers` tree.
- The junction at `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation` was added later as a compatibility repair during debugging, not as the original cause.
- Based on the recorded investigation, the confirmed root-cause boundary is only that an opaque runtime / session discovery layer surfaced the wrong path. No editable local registration record was found and tied to that behavior.

The full timeline, investigation details, root-cause boundary, compatibility repair, and later cleanup are documented separately in:

- [`CONFLICT.md`](./CONFLICT.md)

Public documentation in this repository now uses anonymized path examples and replaces the real username with `UserName`.

### Download and adaptation

You can download the whole repository or just take the contents under `memory-rule-curation/`.

Important: this skill currently contains hardcoded paths for my local environment, such as `D:\CodexData`. During installation, you should replace those paths with the root directory of your own Agent app.

For example:

- Replace `D:\CodexData\AGENTS.md` with the `AGENTS.md` path under your own Agent app root.
- Replace `D:\CodexData\memories` with the `memories` directory under your own Agent app root.
- Replace any other path starting with `D:\CodexData` in the same way.

If your Agent app root is not a Windows path, or if your directory layout differs, you should also adjust the directory examples and related reference text accordingly.

### Current status

- This skill is ready for use and further iteration in my own environment.
- It is published to GitHub for versioning and sharing.
- The path-conflict issue now has a written incident record and remediation note in [`CONFLICT.md`](./CONFLICT.md).
- Public-facing documentation now anonymizes the personal username as `UserName`.
- It has not yet been fully generalized into a plug-and-play skill for other users.
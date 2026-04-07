# Skill Discovery Conflict

## 中文说明

这份文档记录 `memory-rule-curation` 在 Codex skill discovery 过程中出现的一次路径冲突，以及这次冲突是如何被定位、临时修复并最终清理的。

### 现象

`memory-rule-curation` 的真实目录始终位于：

- `D:\CodexData\skills\memory-rule-curation`

但在运行时暴露出来的 skill 路径一度变成了：

- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation\SKILL.md`

这会造成一个误导性的表象：看起来像是这个 skill 在创建时就被注册进了 `superpowers` 仓库的子目录里。

### 已确认事实

以下事实已经由调试记录确认：

1. `memory-rule-curation` 的真实实体目录创建在 `D:\CodexData\skills\memory-rule-curation`。
2. 当时技能列表对外暴露了错误路径 `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation\SKILL.md`。
3. 排障过程中没有在以下位置找到一个明确可编辑、可直接归因的本地注册记录：
   - `D:\CodexData\config.toml`
   - `D:\CodexData\state_5.sqlite`
   - `D:\CodexData\logs_1.sqlite`
   - packaged-app LocalState / LocalCache 的文本层
   - `C:\Users\UserName\.codex` 下可见的 metadata
4. 因此，这次问题更像是一个陈旧或不透明的 runtime / session discovery layer 暴露了错误路径，而不是 skill 本体损坏。
5. `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation` 这个 junction 是调试时后来创建的兼容补丁，用来立刻恢复路径解析；它不是这次冲突的起点。

### 根因判断边界

这次事件最重要的点，不是“已经完全找到唯一根因”，而是“明确知道哪些说法不能乱写”。

可以确认的说法是：

- 运行时 / 会话层把 skill 暴露成了错误路径。
- 这个错误路径并不反映 skill 的真实创建位置。
- 可见配置和本地状态检查未能找到一个可直接编辑的注册来源。

不能当成既定事实的说法是：

- “这个 skill 本来就创建在 `superpowers` 子目录下。”
- “根因就是它被挂进了 `superpowers` namespace。”
- “`superpowers` 下的 junction 就是冲突来源。”

后两种说法都把排障时后加进去的兼容补丁误写成了根因，这是错误的。

### 修复过程

修复分成两个阶段。

#### 第一阶段：兼容补丁

为了先恢复可用性，调试时创建了一个 compatibility junction：

- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation -> D:\CodexData\skills\memory-rule-curation`

这一步的目的不是解释根因，而是让已经对外暴露的错误路径重新能解析到真实 skill 目录，从而立即恢复使用。

#### 第二阶段：命名空间清理

后续又做了一步更干净的整理：

- 创建顶层 personal skill 入口：
  - `C:\Users\UserName\.agents\skills\memory-rule-curation -> D:\CodexData\skills\memory-rule-curation`
- 删除 `superpowers` 目录下的嵌套兼容 junction：
  - `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation`

这一步的作用是减少命名空间误导，让这个 skill 不再表现得像 `superpowers` 自带子项。

### 当前状态

当前文件系统层面的状态是：

- `D:\CodexData\skills\memory-rule-curation` 仍然是单一真实来源。
- `C:\Users\UserName\.agents\skills\memory-rule-curation` 是当前的顶层 personal skill 发现入口。
- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation` 已被移除，不再作为兼容挂载保留。

### 剩余注意事项

如果某个已启动的 Codex 会话仍然显示旧路径，最可能的原因是启动期 skill catalog 缓存还没有刷新。在这种情况下，重启 Codex 或开启新会话通常才会完全反映新的 discovery 结构。

## English

This document records a skill-discovery path conflict involving `memory-rule-curation` and explains how it was investigated, patched for compatibility, and later cleaned up.

### Symptom

The real `memory-rule-curation` directory always lived at:

- `D:\CodexData\skills\memory-rule-curation`

But the runtime temporarily surfaced the skill through the wrong path:

- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation\SKILL.md`

That created a misleading impression that the skill had originally been registered inside the `superpowers` repository tree.

### Confirmed facts

The debug record confirms the following:

1. The real skill directory was created at `D:\CodexData\skills\memory-rule-curation`.
2. The exposed skill list showed the wrong path `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation\SKILL.md`.
3. During investigation, no clear editable local registration record was found in:
   - `D:\CodexData\config.toml`
   - `D:\CodexData\state_5.sqlite`
   - `D:\CodexData\logs_1.sqlite`
   - packaged-app LocalState / LocalCache text layers
   - visible metadata under `C:\Users\UserName\.codex`
4. The issue therefore behaved like a stale or opaque runtime / session discovery layer surfacing the wrong path, rather than a broken skill body.
5. The junction at `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation` was created later as a compatibility repair to restore path resolution. It was not the starting point of the conflict.

### Root-cause boundary

The key point in this incident is not “we know the one true root cause in full detail.” The key point is “we know which claims must not be overstated.”

What can be stated confidently:

- the runtime / session layer surfaced the wrong path;
- that surfaced path did not reflect the real creation location of the skill;
- inspection of visible config and local state did not reveal a directly editable registration source tied to the behavior.

What should not be written as settled fact:

- “the skill was originally created inside the `superpowers` tree”;
- “the root cause was that it had been mounted into the `superpowers` namespace”;
- “the `superpowers` junction itself caused the conflict.”

Those claims incorrectly turn a later compatibility patch into the original cause.

### Repair sequence

The repair happened in two stages.

#### Stage 1: compatibility patch

To restore immediate usability, a compatibility junction was created:

- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation -> D:\CodexData\skills\memory-rule-curation`

The purpose of this step was not to explain the root cause. It was to make the externally surfaced wrong path resolve back to the real skill directory as quickly as possible.

#### Stage 2: namespace cleanup

A later cleanup step made the discovery shape less misleading:

- create a top-level personal-skill entry:
  - `C:\Users\UserName\.agents\skills\memory-rule-curation -> D:\CodexData\skills\memory-rule-curation`
- remove the nested compatibility junction under `superpowers`:
  - `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation`

This step reduced namespace confusion so the skill no longer appeared to be a built-in child of `superpowers`.

### Current state

At the filesystem layer, the current state is:

- `D:\CodexData\skills\memory-rule-curation` remains the single real source.
- `C:\Users\UserName\.agents\skills\memory-rule-curation` is the current top-level personal-skill discovery entry.
- `C:\Users\UserName\.codex\superpowers\skills\memory-rule-curation` has been removed and is no longer kept as a compatibility mount.

### Remaining caveat

If an already-running Codex session still shows the old path, the most likely reason is that the startup-time skill catalog has not been refreshed yet. In practice, restarting Codex or opening a fresh session is usually required for the updated discovery structure to appear end-to-end.
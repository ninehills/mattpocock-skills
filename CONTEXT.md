# Matt Pocock 技能集

一组由 Claude Code 加载的 agent 技能（斜杠命令和行为）。技能按分组组织，由 `/setup-matt-pocock-skills` 生成的按仓库配置来消费。

## 术语

**Issue 追踪器**：
托管仓库 issue 的工具——GitHub Issues、Linear、本地 `.scratch/` markdown 约定或类似工具。像 `to-issues`、`to-prd`、`triage` 和 `qa` 这样的技能从中读取和写入。
_避免使用_：backlog manager、backlog backend、issue host

**Issue**：
**Issue 追踪器**中单个可追踪的工作单元——由 `to-issues` 产出的 bug、任务、PRD 或切片。
_避免使用_：ticket（仅在引用将其称为 ticket 的外部系统时使用）

**分诊角色**：
在分诊期间应用于 **Issue** 的规范状态机标签（例如 `needs-triage`、`ready-for-afk`）。每个角色通过 `docs/agents/triage-labels.md` 映射到 **Issue 追踪器**中的实际标签字符串。

## 关系

- 一个 **Issue 追踪器** 包含多个 **Issue**
- 一个 **Issue** 同时携带一个 **分诊角色**

## 已标记的歧义

- "backlog" 之前被用来同时表示托管 issue 的*工具*和其中的*工作体*——已解决：工具是 **Issue 追踪器**；"backlog" 不再作为领域术语使用。
- "backlog backend" / "backlog manager" ——已解决：合并为 **Issue 追踪器**。

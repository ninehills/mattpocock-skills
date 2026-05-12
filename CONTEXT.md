# Matt Pocock 技能 (Matt Pocock Skills)

一组由 Claude Code 加载的 agent 技能（斜杠命令和行为）。技能按类别组织，由 `/setup-matt-pocock-skills` 发出的每个仓库配置消费。

> A collection of agent skills (slash commands and behaviors) loaded by Claude Code. Skills are organized into buckets and consumed by per-repo configuration emitted by `/setup-matt-pocock-skills`.

## 领域语言 (Language)

**Issue tracker**（问题追踪器）：
托管仓库 issue 的工具——GitHub Issues、Linear、本地 `.scratch/` markdown 约定或类似工具。`to-issues`、`to-prd`、`triage` 和 `qa` 等技能从中读取和写入。
_避免使用_：backlog manager、backlog backend、issue host

> **Issue tracker**:
> The tool that hosts a repo's issues — GitHub Issues, Linear, a local `.scratch/` markdown convention, or similar. Skills like `to-issues`, `to-prd`, `triage`, and `qa` read from and write to it.
> _Avoid_: backlog manager, backlog backend, issue host

**Issue**（问题）：
**Issue tracker** 中的单个跟踪工作单元——由 `to-issues` 产生的 bug、task、PRD 或切片。
_避免使用_：ticket（仅在引用称其为 ticket 的外部系统时使用）

> **Issue**:
> A single tracked unit of work inside an **Issue tracker** — a bug, task, PRD, or slice produced by `to-issues`.
> _Avoid_: ticket (use only when quoting external systems that call them tickets)

**Triage role**（分类角色）：
在分类期间应用于 **Issue** 的规范状态机标签（例如 `needs-triage`、`ready-for-afk`）。每个角色通过 `docs/agents/triage-labels.md` 映射到 **Issue tracker** 中的实际标签字符串。

> **Triage role**:
> A canonical state-machine label applied to an **Issue** during triage (e.g. `needs-triage`, `ready-for-afk`). Each role maps to a real label string in the **Issue tracker** via `docs/agents/triage-labels.md`.

## 关系 (Relationships)

- 一个 **Issue tracker** 包含多个 **Issue**
  > An **Issue tracker** holds many **Issues**
- 一个 **Issue** 同一时间携带一个 **Triage role**
  > An **Issue** carries one **Triage role** at a time

## 已标记的歧义 (Flagged ambiguities)

- "backlog" 之前既用于表示托管 issue 的*工具*，也用于表示其中的*工作集*——已解决：工具是 **Issue tracker**；"backlog" 不再作为领域术语使用。
  > "backlog" was previously used to mean both the *tool* hosting issues and the *body of work* inside it — resolved: the tool is the **Issue tracker**; "backlog" is no longer used as a domain term.
- "backlog backend" / "backlog manager"——已解决：合并为 **Issue tracker**。
  > "backlog backend" / "backlog manager" — resolved: collapsed into **Issue tracker**.

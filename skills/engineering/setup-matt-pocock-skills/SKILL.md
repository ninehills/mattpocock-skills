---
name: setup-matt-pocock-skills
description: 在 AGENTS.md/CLAUDE.md 和 docs/agents/ 中设置一个 `## Agent skills` 块，以便工程技能了解此仓库的问题跟踪器（GitHub 或本地 markdown）、分诊标签词汇和领域文档布局。在首次使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前运行——或当这些技能似乎缺少问题跟踪器、分诊标签或领域文档的上下文时运行。
disable-model-invocation: true
---

# 设置 Matt Pocock 的技能

搭建工程技能所假设的每个仓库配置：

- **问题跟踪器** — Issue 存放的位置（默认 GitHub；也开箱支持本地 markdown）
- **分诊标签** — 五个规范分诊角色使用的字符串
- **领域文档** — `CONTEXT.md` 和 ADR 存放的位置，以及读取它们的消费者规则

这是一个提示驱动的技能，不是确定性脚本。探索、展示发现、与用户确认，然后写入。

## 流程

### 1. 探索

查看当前仓库以了解其初始状态。读取存在的任何内容；不要假设：

- `git remote -v` 和 `.git/config` — 这是 GitHub 仓库吗？哪个？
- 仓库根目录的 `AGENTS.md` 和 `CLAUDE.md` — 是否存在？是否已有 `## Agent skills` 部分？
- 仓库根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录
- `docs/agents/` — 此技能的先前输出是否已存在？
- `.scratch/` — 本地 markdown 问题跟踪器约定已在使用的标志

### 2. 展示发现并询问

总结存在和缺失的内容。然后**一次一个**引导用户完成三个决策——展示一个部分，获得用户的答案，然后转到下一个。不要一次全部倾倒。

假设用户不知道这些术语的含义。每个部分以简短的解释开始（它是什么，为什么这些技能需要它，如果他们选择不同会有什么变化）。然后展示选项和默认值。

**部分 A — 问题跟踪器。**

> 解释：问题跟踪器是此仓库 Issue 存放的地方。像 `to-issues`、`triage`、`to-prd` 和 `qa` 这样的技能会从中读取和写入——它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写 markdown 文件，还是遵循你描述的其他工作流。选择你实际为此仓库跟踪工作的地方。

默认姿态：这些技能是为 GitHub 设计的。如果 `git remote` 指向 GitHub，建议那个。如果 `git remote` 指向 GitLab（`gitlab.com` 或自托管主机），建议 GitLab。否则（或如果用户偏好），提供：

- **GitHub** — Issue 存在于仓库的 GitHub Issues 中（使用 `gh` CLI）
- **GitLab** — Issue 存在于仓库的 GitLab Issues 中（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown** — Issue 作为此仓库 `.scratch/<feature>/` 下的文件存在（适合个人项目或无远程的仓库）
- **其他**（Jira、Linear 等）— 要求用户用一段话描述工作流；技能将记录为自由文本

**部分 B — 分诊标签词汇。**

> 解释：当 `triage` 技能处理传入的 Issue 时，它通过状态机移动它——需要评估、等待报告者、准备好由离线代理领取、需要人工、或不修复。为此，它需要应用匹配你*实际配置*的字符串的标签（或问题跟踪器中的等效物）。如果你的仓库已经使用不同的标签名称（例如 `bug:triage` 而不是 `needs-triage`），在此映射，以便技能应用正确的标签而不是创建重复。

五个规范角色：

- `needs-triage` — 维护者需要评估
- `needs-info` — 等待报告者
- `ready-for-agent` — 完全指定，离线准备就绪（代理可以在没有人工上下文的情况下领取）
- `ready-for-human` — 需要人工实现
- `wontfix` — 不会被处理

默认：每个角色的字符串等于其名称。询问用户是否要覆盖任何。如果他们的问题跟踪器没有现有标签，使用默认值即可。

**部分 C — 领域文档。**

> 解释：一些技能（`improve-codebase-architecture`、`diagnose`、`tdd`）读取 `CONTEXT.md` 文件来学习项目的领域语言，以及 `docs/adr/` 了解过去的架构决策。它们需要知道仓库有一个全局上下文还是多个（例如具有独立前端/后端上下文的 monorepo），以便它们在正确的位置查找。

确认布局：

- **单上下文** — 仓库根目录一个 `CONTEXT.md` + `docs/adr/`。大多数仓库是这种。
- **多上下文** — 根目录的 `CONTEXT-MAP.md` 指向每个上下文的 `CONTEXT.md` 文件（通常是 monorepo）。

### 3. 确认并编辑

向用户展示以下内容的草稿：

- 要添加到正在编辑的 `CLAUDE.md` / `AGENTS.md` 中的 `## Agent skills` 块（选择规则见步骤 4）
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容

让他们在写入前编辑。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果都不存在，询问用户创建哪一个——不要替他们选择。

当 `CLAUDE.md` 已存在时永远不要创建 `AGENTS.md`（反之亦然）——始终编辑已存在的那个。

如果选定的文件中已存在 `## Agent skills` 块，就地更新其内容而不是追加重复。不要覆盖周围部分的用户编辑。

该块：

```markdown
## Agent skills

### Issue tracker

[一行摘要说明 Issue 跟踪的位置]。见 `docs/agents/issue-tracker.md`。

### Triage labels

[一行摘要说明标签词汇]。见 `docs/agents/triage-labels.md`。

### Domain docs

[一行摘要说明布局——"单上下文"或"多上下文"]。见 `docs/agents/domain.md`。
```

然后使用此技能文件夹中的种子模板作为起点写入三个文档文件：

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub 问题跟踪器
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) — GitLab 问题跟踪器
- [issue-tracker-local.md](./issue-tracker-local.md) — 本地 markdown 问题跟踪器
- [triage-labels.md](./triage-labels.md) — 标签映射
- [domain.md](./domain.md) — 领域文档消费者规则 + 布局

对于"其他"问题跟踪器，根据用户的描述从头编写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户设置已完成，以及哪些工程技能现在将从这些文件读取。提及他们以后可以直接编辑 `docs/agents/*.md`——仅当他们想要切换问题跟踪器或从头重新开始时才需要重新运行此技能。

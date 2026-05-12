---
name: setup-matt-pocock-skills
description: 在 AGENTS.md/CLAUDE.md 中设置 `## Agent skills` 块和 `docs/agents/`，以便工程技能了解此仓库的 issue 跟踪器（GitHub 或本地 markdown）、分诊标签词汇和领域文档布局。在首次使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前运行——或当这些技能似乎缺少关于 issue 跟踪器、分诊标签或领域文档的上下文时运行。 (Sets up an `## Agent skills` block in AGENTS.md/CLAUDE.md and `docs/agents/` so the engineering skills know this repo's issue tracker (GitHub or local markdown), triage label vocabulary, and domain doc layout. Run before first use of `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out` — or if those skills appear to be missing context about the issue tracker, triage labels, or domain docs.)
disable-model-invocation: true
---

# 设置 Matt Pocock 技能 (Setup Matt Pocock's Skills)

搭建工程技能所假设的每仓库配置：

> Scaffold the per-repo configuration that the engineering skills assume:

- **Issue 跟踪器 (Issue tracker)** — issue 存放在哪里（默认 GitHub；也开箱支持本地 markdown）
  > Where issues live (GitHub by default; local markdown is also supported out of the box)
- **分诊标签 (Triage labels)** — 用于五个规范分诊角色的字符串
  > The strings used for the five canonical triage roles
- **领域文档 (Domain docs)** — `CONTEXT.md` 和 ADR 存放在哪里，以及读取它们的消费规则
  > Where `CONTEXT.md` and ADRs live, and the consumer rules for reading them

这是一个提示驱动的技能，不是确定性脚本。探索、呈现发现、与用户确认，然后写入。

> This is a prompt-driven skill, not a deterministic script. Explore, present what you found, confirm with the user, then write.

## 流程 (Process)

### 1. 探索 (Explore)

查看当前仓库以了解其初始状态。读取已存在的内容；不要假设：

> Look at the current repo to understand its starting state. Read whatever exists; don't assume:

- `git remote -v` 和 `.git/config` — 这是 GitHub 仓库吗？哪一个？
  > `git remote -v` and `.git/config` — is this a GitHub repo? Which one?
- 仓库根目录的 `AGENTS.md` 和 `CLAUDE.md` — 是否存在？其中是否已有 `## Agent skills` 部分？
  > `AGENTS.md` and `CLAUDE.md` at the repo root — does either exist? Is there already an `## Agent skills` section in either?
- 仓库根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
  > `CONTEXT.md` and `CONTEXT-MAP.md` at the repo root
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录
  > `docs/adr/` and any `src/*/docs/adr/` directories
- `docs/agents/` — 此技能的先前输出是否已存在？
  > `docs/agents/` — does this skill's prior output already exist?
- `.scratch/` — 本地 markdown issue 跟踪器约定已在使用的标志
  > `.scratch/` — sign that a local-markdown issue tracker convention is already in use

### 2. 呈现发现并询问 (Present findings and ask)

总结什么存在和什么缺失。然后**一次一个**引导用户通过三个决策——呈现一个部分，得到用户的答案，然后进入下一个。不要一次倾倒所有三个。

> Summarise what's present and what's missing. Then walk the user through the three decisions **one at a time** — present a section, get the user's answer, then move to the next. Don't dump all three at once.

假设用户不知道这些术语的含义。每个部分以简短解释开始（它是什么，为什么这些技能需要它，如果选择不同会有什么变化）。然后展示选择和默认值。

> Assume the user does not know what these terms mean. Each section starts with a short explainer (what it is, why these skills need it, what changes if they pick differently). Then show the choices and the default.

**部分 A — Issue 跟踪器 (Section A — Issue tracker)**。

> Explainer: "Issue 跟踪器"是此仓库存放 issue 的地方。像 `to-issues`、`triage`、`to-prd` 和 `qa` 这样的技能从中读取和写入——它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写 markdown 文件，还是遵循你描述的其他工作流。选择你实际为此仓库跟踪工作的地方。
> The "issue tracker" is where issues live for this repo. Skills like `to-issues`, `triage`, `to-prd`, and `qa` read from and write to it — they need to know whether to call `gh issue create`, write a markdown file under `.scratch/`, or follow some other workflow you describe. Pick the place you actually track work for this repo.

默认姿态：这些技能为 GitHub 设计。如果 `git remote` 指向 GitHub，提议那个。如果 `git remote` 指向 GitLab（`gitlab.com` 或自托管），提议 GitLab。否则（或如果用户偏好），提供：

> Default posture: these skills were designed for GitHub. If a `git remote` points at GitHub, propose that. If a `git remote` points at GitLab (`gitlab.com` or a self-hosted host), propose GitLab. Otherwise (or if the user prefers), offer:

- **GitHub** — issue 存在仓库的 GitHub Issues 中（使用 `gh` CLI）
  > Issues live in the repo's GitHub Issues (uses the `gh` CLI)
- **GitLab** — issue 存在仓库的 GitLab Issues 中（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
  > Issues live in the repo's GitLab Issues (uses the [`glab`](https://gitlab.com/gitlab-org/cli) CLI)
- **本地 markdown (Local markdown)** — issue 作为文件存放在此仓库的 `.scratch/<feature>/` 下（适合个人项目或没有远程的仓库）
  > Issues live as files under `.scratch/<feature>/` in this repo (good for solo projects or repos without a remote)
- **其他 (Other)**（Jira、Linear 等）— 让用户用一段话描述工作流；技能会将其记录为自由文本
  > Ask the user to describe the workflow in one paragraph; the skill will record it as freeform prose

**部分 B — 分诊标签词汇 (Section B — Triage label vocabulary)**。

> Explainer: 当 `triage` 技能处理传入的 issue 时，它通过状态机移动它——需要评估、等待报告者、准备就绪可由 AFK agent 认领、准备就绪需人工处理、或不修复。为此，它需要应用标签（或你的 issue 跟踪器中的等效物）来匹配你*实际配置*的*字符串*。如果你的仓库已经使用不同的标签名称（例如 `bug:triage` 而不是 `needs-triage`），在这里映射它们以便技能应用正确的标签而不是创建重复的。
> When the `triage` skill processes an incoming issue, it moves it through a state machine — needs evaluation, waiting on reporter, ready for an AFK agent to pick up, ready for a human, or won't fix. To do that, it needs to apply labels (or the equivalent in your issue tracker) that match strings *you've actually configured*. If your repo already uses different label names (e.g. `bug:triage` instead of `needs-triage`), map them here so the skill applies the right ones instead of creating duplicates.

五个规范角色：

> The five canonical roles:

- `needs-triage` — 维护者需要评估 (maintainer needs to evaluate)
- `needs-info` — 等待报告者提供更多信息 (waiting on reporter)
- `ready-for-agent` — 完全指定，AFK 就绪（agent 可以无需人工上下文即可认领）(fully specified, AFK-ready (an agent can pick it up with no human context))
- `ready-for-human` — 需要人工实现 (needs human implementation)
- `wontfix` — 不会被处理 (will not be actioned)

默认值：每个角色的字符串等于其名称。询问用户是否要覆盖。如果他们的 issue 跟踪器没有现有标签，默认值就可以了。

> Default: each role's string equals its name. Ask the user if they want to override any. If their issue tracker has no existing labels, the defaults are fine.

**部分 C — 领域文档 (Section C — Domain docs)**。

> Explainer: 一些技能（`improve-codebase-architecture`、`diagnose`、`tdd`）读取 `CONTEXT.md` 文件来学习项目的领域语言，以及 `docs/adr/` 来了解过去的架构决策。它们需要知道仓库是有一个全局上下文还是多个（例如有独立前端/后端上下文的 monorepo），以便它们在正确的地方查找。
> Some skills (`improve-codebase-architecture`, `diagnose`, `tdd`) read a `CONTEXT.md` file to learn the project's domain language, and `docs/adr/` for past architectural decisions. They need to know whether the repo has one global context or multiple (e.g. a monorepo with separate frontend/backend contexts) so they look in the right place.

确认布局：

> Confirm the layout:

- **单上下文 (Single context)** — 仓库根目录一个 `CONTEXT.md` + `docs/adr/`。大多数仓库是这种。
  > One `CONTEXT.md` + `docs/adr/` at the repo root. Most repos are this.
- **多上下文 (Multi-context)** — 根目录 `CONTEXT-MAP.md` 指向每个上下文的 `CONTEXT.md` 文件（通常是 monorepo）。
  > `CONTEXT-MAP.md` at the root pointing to per-context `CONTEXT.md` files (typically a monorepo).

### 3. 确认并编辑 (Confirm and edit)

向用户展示以下内容的草稿：

> Show the user a draft of:

- 要添加到正在编辑的 `CLAUDE.md` / `AGENTS.md` 的 `## Agent skills` 块（选择规则见步骤 4）
  > The `## Agent skills` block to add to whichever of `CLAUDE.md` / `AGENTS.md` is being edited (see step 4 for selection rules)
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容
  > The contents of `docs/agents/issue-tracker.md`, `docs/agents/triage-labels.md`, `docs/agents/domain.md`

让用户在写入前编辑。

> Let them edit before writing.

### 4. 写入 (Write)

**选择要编辑的文件：**

> **Pick the file to edit:**

- 如果 `CLAUDE.md` 存在，编辑它。
  > If `CLAUDE.md` exists, edit it.
- 否则如果 `AGENTS.md` 存在，编辑它。
  > Else if `AGENTS.md` exists, edit it.
- 如果都不存在，询问用户要创建哪一个——不要替他们选择。
  > If neither exists, ask the user which one to create — don't pick for them.

当 `CLAUDE.md` 已存在时永远不要创建 `AGENTS.md`（反之亦然）——始终编辑已存在的那个。

> Never create `AGENTS.md` when `CLAUDE.md` already exists (or vice versa) — always edit the one that's already there.

如果所选文件中已有 `## Agent skills` 块，原地更新其内容而不是追加重复的。不要覆盖周围部分的用户编辑。

> If an `## Agent skills` block already exists in the chosen file, update its contents in-place rather than appending a duplicate. Don't overwrite user edits to the surrounding sections.

该块：

> The block:

```markdown
## Agent skills

### Issue tracker

[issue 跟踪位置的一行摘要]。见 `docs/agents/issue-tracker.md`。
[one-line summary of where issues are tracked]. See `docs/agents/issue-tracker.md`.

### Triage labels

[标签词汇的一行摘要]。见 `docs/agents/triage-labels.md`。
[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[布局的一行摘要——"单上下文"或"多上下文"]。见 `docs/agents/domain.md`。
[one-line summary of layout — "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

然后使用此技能文件夹中的种子模板作为起点编写三个文档文件：

> Then write the three docs files using the seed templates in this skill folder as a starting point:

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub issue 跟踪器
  > GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) — GitLab issue 跟踪器
  > GitLab issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md) — 本地 markdown issue 跟踪器
  > Local-markdown issue tracker
- [triage-labels.md](./triage-labels.md) — 标签映射
  > Label mapping
- [domain.md](./domain.md) — 领域文档消费规则 + 布局
  > Domain doc consumer rules + layout

对于"其他"issue 跟踪器，使用用户的描述从头编写 `docs/agents/issue-tracker.md`。

> For "other" issue trackers, write `docs/agents/issue-tracker.md` from scratch using the user's description.

### 5. 完成 (Done)

告诉用户设置已完成，以及哪些工程技能现在会从这些文件读取。提及其后可以直接编辑 `docs/agents/*.md`——只有在他们想切换 issue 跟踪器或从头开始时才需要重新运行此技能。

> Tell the user the setup is complete and which engineering skills will now read from these files. Mention they can edit `docs/agents/*.md` directly later — re-running this skill is only necessary if they want to switch issue trackers or restart from scratch.

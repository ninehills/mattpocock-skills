---
name: review
description: 从一个固定点（提交、分支、标签或 merge-base）沿两个轴审查变更 —— 标准（代码是否遵循本仓库记录的编码规范？）和规格（代码是否符合原始 issue/PRD 的要求？）。以并行子 agent 运行两个审查并并排报告。当用户想要审查分支、PR、进行中的工作变更或要求"review since X"时使用。 (Review the changes since a fixed point (commit, branch, tag, or merge-base) along two axes — Standards (does the code follow this repo's documented coding standards?) and Spec (does the code match what the originating issue/PRD asked for?). Runs both reviews in parallel sub-agents and reports them side by side. Use when the user wants to review a branch, a PR, work-in-progress changes, or asks to "review since X".)
---

# 审查 (Review)

对 `HEAD` 与用户提供的固定点之间的 diff 进行双轴审查：
> Two-axis review of the diff between `HEAD` and a fixed point the user supplies:

- **标准** — 代码是否符合本仓库记录的编码规范？
  > **Standards** — does the code conform to this repo's documented coding standards?
- **规格** — 代码是否忠实地实现了原始 issue / PRD / 规格？
  > **Spec** — does the code faithfully implement the originating issue / PRD / spec?

两个轴作为**并行子 agent** 运行，这样它们不会相互污染上下文，然后本技能汇总它们的发现。

> Both axes run as **parallel sub-agents** so they don't pollute each other's context, then this skill aggregates their findings.

issue tracker 应该已经提供给你了 —— 如果 `docs/agents/issue-tracker.md` 缺失，运行 `/setup-matt-pocock-skills`。

> The issue tracker should have been provided to you — run `/setup-matt-pocock-skills` if `docs/agents/issue-tracker.md` is missing.

## 流程 (Process)

### 1. 固定固定点

用户说的任何内容作为固定点 —— 提交 SHA、分支名、标签、`main`、`HEAD~5` 等。不要有主见；直接传递。如果用户没有指定，询问："以什么为基准审查 —— 分支、提交还是 `main`？"在得到之前不要继续。

> Whatever the user said is the fixed point — a commit SHA, branch name, tag, `main`, `HEAD~5`, etc. Don't be opinionated; pass it through. If they didn't specify one, ask: "Review against what — a branch, a commit, or `main`?" Don't proceed until you have it.

捕获 diff 命令一次：`git diff <fixed-point>...HEAD`（三点，所以比较是针对 merge-base 的）。同时通过 `git log <fixed-point>..HEAD --oneline` 记录提交列表。

> Capture the diff command once: `git diff <fixed-point>...HEAD` (three-dot, so the comparison is against the merge-base). Also note the list of commits via `git log <fixed-point>..HEAD --oneline`.

### 2. 确定规格来源

按以下顺序查找原始规格：
> Look for the originating spec, in this order:

1. 提交消息中的 issue 引用（`#123`、`Closes #45`、GitLab `!67` 等）—— 通过 `docs/agents/issue-tracker.md` 中的工作流获取。
   > Issue references in the commit messages (`#123`, `Closes #45`, GitLab `!67`, etc.) — fetch via the workflow in `docs/agents/issue-tracker.md`.
2. 用户作为参数传递的路径。
   > A path the user passed as an argument.
3. `docs/`、`specs/` 或 `.scratch/` 下与分支名或功能匹配的 PRD/规格文件。
   > A PRD/spec file under `docs/`, `specs/`, or `.scratch/` matching the branch name or feature.
4. 如果什么都没找到，询问用户规格在哪里。如果用户说没有，**规格**子 agent 将跳过并报告"no spec available"。
   > If nothing is found, ask the user where the spec is. If they say there isn't one, the **Spec** sub-agent will skip and report "no spec available".

### 3. 确定标准来源

仓库中记录代码编写方式的任何内容。常见位置：
> Anything in the repo that documents how code should be written. Common locations:

- `CLAUDE.md`、`AGENTS.md`
- `CONTRIBUTING.md`
- `CONTEXT.md`、`CONTEXT-MAP.md`、每个上下文的 `CONTEXT.md` 文件
- `docs/adr/`（架构决策也是标准）
  > `docs/adr/` (architectural decisions are standards)
- `.editorconfig`、`eslint.config.*`、`biome.json`、`prettier.config.*`、`tsconfig.json`（机器强制的标准 —— 记录它们但不重新检查工具已经检查的内容）
  > `.editorconfig`, `eslint.config.*`, `biome.json`, `prettier.config.*`, `tsconfig.json` (machine-enforced standards — note them but don't re-check what tooling already checks)
- 仓库根目录或 `docs/` 下的任何 `STYLE.md`、`STANDARDS.md`、`STYLEGUIDE.md` 或类似文件
  > Any `STYLE.md`, `STANDARDS.md`, `STYLEGUIDE.md`, or similar at the repo root or under `docs/`

收集文件列表。**标准**子 agent 将读取它们。

> Collect the list of files. The **Standards** sub-agent will read them.

### 4. 并行启动两个子 agent

发送一条包含两个 `Agent` 工具调用的消息。对两者都使用 `general-purpose` 子 agent。

> Send a single message with two `Agent` tool calls. Use the `general-purpose` subagent for both.

**标准子 agent 提示** —— 包括：
> **Standards sub-agent prompt** — include:

- 完整的 diff 命令和提交列表。
  > The full diff command and commit list.
- 在步骤3中找到的标准源文件列表。
  > The list of standards-source files you found in step 3.
- 指令："读取标准文档。然后读取 diff。报告 —— 按文件/块相关地 —— diff 违反记录标准的每个地方。引用标准（文件 + 规则）。区分硬违规和判断性跳过。跳过工具强制检查的内容。400字以内。"
  > The brief: "Read the standards docs. Then read the diff. Report — per file/hunk where relevant — every place the diff violates a documented standard. Cite the standard (file + the rule). Distinguish hard violations from judgement calls. Skip anything tooling enforces. Under 400 words."

**规格子 agent 提示** —— 包括：
> **Spec sub-agent prompt** — include:

- diff 命令和提交列表。
  > The diff command and commit list.
- 规格的路径或获取的内容。
  > The path or fetched contents of the spec.
- 指令："读取规格。然后读取 diff。报告：(a) 规格要求的但缺失或部分实现的需求；(b) diff 中未被要求的行为（范围蔓延）；(c) 看起来已实现但实现看起来有问题的需求。对每个发现引用规格行。400字以内。"
  > The brief: "Read the spec. Then read the diff. Report: (a) requirements the spec asked for that are missing or partial; (b) behaviour in the diff that wasn't asked for (scope creep); (c) requirements that look implemented but where the implementation looks wrong. Quote the spec line for each finding. Under 400 words."

如果规格缺失，跳过规格子 agent 并在最终报告中注明。

> If the spec is missing, skip the Spec sub-agent and note this in the final report.

### 5. 汇总

在 `## Standards` 和 `## Spec` 标题下逐字或轻度清理地呈现两份报告。**不要**合并或重新排序发现 —— 两个轴是刻意分开的，这样用户可以独立查看。

> Present the two reports under `## Standards` and `## Spec` headings, verbatim or lightly cleaned. Do **not** merge or rerank findings — the two axes are deliberately separate so the user can see them independently.

以一行摘要结束：每个轴的总发现数，以及标记的最严重单个问题（如果有）。

> End with a one-line summary: total findings per axis, and the worst single issue (if any) flagged.

## 为什么两个轴 (Why two axes)

一个变更可能通过一个轴但不通过另一个：
> A change can pass one axis and fail the other:

- 代码遵循每个标准但实现了错误的东西 → **标准通过，规格失败。**
  > Code that follows every standard but implements the wrong thing → **Standards pass, Spec fail.**
- 代码完全按 issue 要求实现但违反了项目约定 → **规格通过，标准失败。**
  > Code that does exactly what the issue asked but breaks the project's conventions → **Spec pass, Standards fail.**

分开报告可以防止一个轴掩盖另一个。

> Reporting them separately stops one axis from masking the other.

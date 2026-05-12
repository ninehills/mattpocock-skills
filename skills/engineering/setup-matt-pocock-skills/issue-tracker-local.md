# Issue 跟踪器：本地 Markdown (Issue tracker: Local Markdown)

此仓库的 issue 和 PRD 作为 markdown 文件存放在 `.scratch/` 中。

> Issues and PRDs for this repo live as markdown files in `.scratch/`.

## 约定 (Conventions)

- 每个功能一个目录：`.scratch/<feature-slug>/`
  > One feature per directory: `.scratch/<feature-slug>/`
- PRD 是 `.scratch/<feature-slug>/PRD.md`
  > The PRD is `.scratch/<feature-slug>/PRD.md`
- 实现 issue 是 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始编号
  > Implementation issues are `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`
- 分诊状态记录在每个 issue 文件顶部附近的 `Status:` 行（角色字符串见 `triage-labels.md`）
  > Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- 评论和对话历史追加在文件底部的 `## Comments` 标题下
  > Comments and conversation history append to the bottom of the file under a `## Comments` heading

## 当技能说"发布到 issue 跟踪器"时 (When a skill says "publish to the issue tracker")

在 `.scratch/<feature-slug>/` 下创建新文件（如需要则创建目录）。

> Create a new file under `.scratch/<feature-slug>/` (creating the directory if needed).

## 当技能说"获取相关工单"时 (When a skill says "fetch the relevant ticket")

读取引用路径处的文件。用户通常会直接传递路径或 issue 编号。

> Read the file at the referenced path. The user will normally pass the path or the issue number directly.

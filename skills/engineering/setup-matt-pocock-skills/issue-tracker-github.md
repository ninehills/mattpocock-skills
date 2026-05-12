# Issue 跟踪器：GitHub (Issue tracker: GitHub)

此仓库的 issue 和 PRD 作为 GitHub issue 存放。使用 `gh` CLI 进行所有操作。

> Issues and PRDs for this repo live as GitHub issues. Use the `gh` CLI for all operations.

## 约定 (Conventions)

- **创建 issue (Create an issue)**：`gh issue create --title "..." --body "..."`。多行正文使用 heredoc。
  > Use a heredoc for multi-line bodies.
- **读取 issue (Read an issue)**：`gh issue view <number> --comments`，用 `jq` 过滤评论并同时获取标签。
  > Filtering comments by `jq` and also fetching labels.
- **列出 issue (List issues)**：`gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'`，配合适当的 `--label` 和 `--state` 过滤器。
  > With appropriate `--label` and `--state` filters.
- **评论 issue (Comment on an issue)**：`gh issue comment <number> --body "..."`
  > `gh issue comment <number> --body "..."`
- **应用/移除标签 (Apply / remove labels)**：`gh issue edit <number> --add-label "..."` / `--remove-label "..."`
  > `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **关闭 (Close)**：`gh issue close <number> --comment "..."`
  > `gh issue close <number> --comment "..."`

从 `git remote -v` 推断仓库——`gh` 在克隆内运行时会自动这样做。

> Infer the repo from `git remote -v` — `gh` does this automatically when run inside a clone.

## 当技能说"发布到 issue 跟踪器"时 (When a skill says "publish to the issue tracker")

创建 GitHub issue。

> Create a GitHub issue.

## 当技能说"获取相关工单"时 (When a skill says "fetch the relevant ticket")

运行 `gh issue view <number> --comments`。

> Run `gh issue view <number> --comments`.

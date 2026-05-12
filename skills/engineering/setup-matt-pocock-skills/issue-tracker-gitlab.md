# 问题跟踪器：GitLab

此仓库的 Issue 和 PRD 存放为 GitLab Issue。使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI 进行所有操作。

## 约定

- **创建 Issue**：`glab issue create --title "..." --description "..."`。多行描述使用 heredoc。传递 `--description -` 以打开编辑器。
- **读取 Issue**：`glab issue view <number> --comments`。使用 `-F json` 获取机器可读输出。
- **列出 Issue**：`glab issue list -F json`，配合适当的 `--label` 过滤器。
- **评论 Issue**：`glab issue note <number> --message "..."`。GitLab 将评论称为"notes"。
- **添加/移除标签**：`glab issue update <number> --label "..."` / `--unlabel "..."`。多个标签可以用逗号分隔或重复使用标志。
- **关闭**：`glab issue close <number>`。`glab issue close` 不接受关闭评论，所以先用 `glab issue note <number> --message "..."` 发布解释，然后关闭。
- **合并请求**：GitLab 将 PR 称为"merge requests"。使用 `glab mr create`、`glab mr view`、`glab mr note` 等——与 `gh pr ...` 形状相同，用 `mr` 替换 `pr`，用 `note`/`--message` 替换 `comment`/`--body`。

从 `git remote -v` 推断仓库——`glab` 在克隆内运行时会自动这样做。

## 当技能说"发布到问题跟踪器"

创建一个 GitLab Issue。

## 当技能说"获取相关工单"

运行 `glab issue view <number> --comments`。

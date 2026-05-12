# Issue 跟踪器：GitLab (Issue tracker: GitLab)

此仓库的 issue 和 PRD 作为 GitLab issue 存放。使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI 进行所有操作。

> Issues and PRDs for this repo live as GitLab issues. Use the [`glab`](https://gitlab.com/gitlab-org/cli) CLI for all operations.

## 约定 (Conventions)

- **创建 issue (Create an issue)**：`glab issue create --title "..." --description "..."`。多行描述使用 heredoc。传入 `--description -` 可打开编辑器。
  > Use a heredoc for multi-line descriptions. Pass `--description -` to open an editor.
- **读取 issue (Read an issue)**：`glab issue view <number> --comments`。使用 `-F json` 获取机器可读输出。
  > Use `-F json` for machine-readable output.
- **列出 issue (List issues)**：`glab issue list -F json`，配合适当的 `--label` 过滤器。
  > With appropriate `--label` filters.
- **评论 issue (Comment on an issue)**：`glab issue note <number> --message "..."`。GitLab 将评论称为"notes"。
  > GitLab calls comments "notes".
- **应用/移除标签 (Apply / remove labels)**：`glab issue update <number> --label "..."` / `--unlabel "..."`。多个标签可用逗号分隔或重复标志。
  > Multiple labels can be comma-separated or by repeating the flag.
- **关闭 (Close)**：`glab issue close <number>`。`glab issue close` 不接受关闭评论，所以先用 `glab issue note <number> --message "..."` 发布解释，然后再关闭。
  > `glab issue close` does not accept a closing comment, so post the explanation first with `glab issue note <number> --message "..."`, then close.
- **合并请求 (Merge requests)**：GitLab 将 PR 称为"merge requests"。使用 `glab mr create`、`glab mr view`、`glab mr note` 等——与 `gh pr ...` 形状相同，用 `mr` 替代 `pr`，用 `note`/`--message` 替代 `comment`/`--body`。
  > GitLab calls PRs "merge requests". Use `glab mr create`, `glab mr view`, `glab mr note`, etc. — the same shape as `gh pr ...` with `mr` in place of `pr` and `note`/`--message` in place of `comment`/`--body`.

从 `git remote -v` 推断仓库——`glab` 在克隆内运行时会自动这样做。

> Infer the repo from `git remote -v` — `glab` does this automatically when run inside a clone.

## 当技能说"发布到 issue 跟踪器"时 (When a skill says "publish to the issue tracker")

创建 GitLab issue。

> Create a GitLab issue.

## 当技能说"获取相关工单"时 (When a skill says "fetch the relevant ticket")

运行 `glab issue view <number> --comments`。

> Run `glab issue view <number> --comments`.

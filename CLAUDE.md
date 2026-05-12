技能按类别文件夹组织在 `skills/` 下：

> Skills are organized into bucket folders under `skills/`:

- `engineering/` — 日常代码工作 (daily code work)
- `productivity/` — 日常非代码工作流工具 (daily non-code workflow tools)
- `misc/` — 保留但很少使用 (kept around but rarely used)
- `personal/` — 与我自己的设置相关，不推广 (tied to my own setup, not promoted)
- `in-progress/` — 尚未准备好发布的草稿 (drafts not yet ready to ship)
- `deprecated/` — 不再使用 (no longer used)

`engineering/`、`productivity/` 或 `misc/` 中的每个技能必须在顶层 `README.md` 中有引用，并在 `.claude-plugin/plugin.json` 中有条目。`personal/`、`in-progress/` 和 `deprecated/` 中的技能不得出现在这两个地方。

> Every skill in `engineering/`, `productivity/`, or `misc/` must have a reference in the top-level `README.md` and an entry in `.claude-plugin/plugin.json`. Skills in `personal/`, `in-progress/`, and `deprecated/` must not appear in either.

顶层 `README.md` 中的每个技能条目必须将技能名称链接到其 `SKILL.md`。

> Each skill entry in the top-level `README.md` must link the skill name to its `SKILL.md`.

每个类别文件夹都有一个 `README.md`，列出该类别中的每个技能及其单行描述，技能名称链接到其 `SKILL.md`。

> Each bucket folder has a `README.md` that lists every skill in the bucket with a one-line description, with the skill name linked to its `SKILL.md`.

技能按照分组文件夹组织在 `skills/` 目录下：

- `engineering/` — 日常代码工作
- `productivity/` — 日常非代码工作流工具
- `misc/` — 保留但很少使用
- `personal/` — 与我自己的设置相关，不对外推广
- `in-progress/` — 尚未准备好发布的草稿
- `deprecated/` — 不再使用

`engineering/`、`productivity/` 或 `misc/` 中的每个技能都必须在顶层 `README.md` 中有引用，并在 `.claude-plugin/plugin.json` 中有条目。`personal/`、`in-progress/` 和 `deprecated/` 中的技能不得出现在两者中。

顶层 `README.md` 中的每个技能条目必须将技能名称链接到其 `SKILL.md`。

每个分组文件夹都有一个 `README.md`，列出该分组中的每个技能及其简要描述，技能名称链接到其 `SKILL.md`。

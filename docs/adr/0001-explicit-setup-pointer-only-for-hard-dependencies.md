# 显式 `/setup-matt-pocock-skills` 指针仅用于硬依赖 (Explicit `/setup-matt-pocock-skills` pointer only for hard dependencies)

工程技能依赖于由 `/setup-matt-pocock-skills` 种子化的每个仓库配置（issue tracker、分类标签词汇、领域文档布局）。某些技能没有该配置就无法有意义地运行——它们必须发布到特定的 issue tracker 或应用特定的标签字符串。其他技能仅使用它来优化输出（词汇、ADR 感知），没有它也能优雅降级。

> Engineering skills depend on per-repo config (issue tracker, triage label vocabulary, domain doc layout) seeded by `/setup-matt-pocock-skills`. Some skills cannot meaningfully function without that config — they have to publish to a specific issue tracker or apply a specific label string. Others only use it to sharpen output (vocabulary, ADR awareness) and degrade gracefully without it.

我们将这些分为**硬依赖**和**软依赖**技能：

> We split these into **hard-dependency** and **soft-dependency** skills:

- **硬依赖 (Hard dependency)**（`to-issues`、`to-prd`、`triage`）——包含一个显式的单行说明：_"……应该已经提供给你了——如果没有，请运行 `/setup-matt-pocock-skills`。"_ 没有映射，输出是错误的，而不仅仅是模糊的。
  > **Hard dependency** (`to-issues`, `to-prd`, `triage`) — include an explicit one-liner: _"… should have been provided to you — run `/setup-matt-pocock-skills` if not."_ Without the mapping, output is wrong, not just fuzzy.
- **软依赖 (Soft dependency)**（`diagnose`、`tdd`、`improve-codebase-architecture`、`zoom-out`）——仅在模糊的文字中引用"项目的领域词汇表"和"你正在修改的区域中的 ADR"。如果文档不存在，技能仍然工作；输出只是不那么精准。
  > **Soft dependency** (`diagnose`, `tdd`, `improve-codebase-architecture`, `zoom-out`) — reference "the project's domain glossary" and "ADRs in the area you're touching" in vague prose only. If the docs aren't there, the skill still works; output is just less sharp.

这种拆分保持了软依赖技能的 token 轻量，并避免了在不承载实际作用的地方盲目复制设置指针。

> The split keeps soft-dependency skills token-light and avoids cargo-culting the setup pointer into places where it isn't load-bearing.

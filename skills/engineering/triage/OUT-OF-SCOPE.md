# 超出范围知识库 (Out-of-Scope Knowledge Base)

仓库中的 `.out-of-scope/` 目录存储被拒绝功能请求的持久记录。它有两个目的：

> The `.out-of-scope/` directory in a repo stores persistent records of rejected feature requests. It serves two purposes:

1. **机构记忆 (Institutional memory)** — 功能被拒绝的原因，这样当 issue 关闭时推理不会丢失
   > Why a feature was rejected, so the reasoning isn't lost when the issue is closed
2. **去重 (Deduplication)** — 当新 issue 进来匹配先前的拒绝记录时，skill 可以呈现之前的决定而不是重新争论
   > When a new issue comes in that matches a prior rejection, the skill can surface the previous decision instead of re-litigating it

## 目录结构 (Directory structure)

```
.out-of-scope/
├── dark-mode.md
├── plugin-system.md
└── graphql-api.md
```

每个**概念**一个文件，而不是每个 issue。多个请求相同内容的 issue 归入一个文件下。

> One file per **concept**, not per issue. Multiple issues requesting the same thing are grouped under one file.

## 文件格式 (File format)

文件应以轻松、可读的风格编写——更像简短的设计文档而不是数据库条目。使用段落、代码示例和例子使推理对首次遇到它的人来说清晰且有用。

> The file should be written in a relaxed, readable style — more like a short design document than a database entry. Use paragraphs, code samples, and examples to make the reasoning clear and useful to someone encountering it for the first time.

```markdown
# Dark Mode

This project does not support dark mode or user-facing theming.

## Why this is out of scope

The rendering pipeline assumes a single color palette defined in
`ThemeConfig`. Supporting multiple themes would require:

- A theme context provider wrapping the entire component tree
- Per-component theme-aware style resolution
- A persistence layer for user theme preferences

This is a significant architectural change that doesn't align with the
project's focus on content authoring. Theming is a concern for downstream
consumers who embed or redistribute the output.

```ts
// The current ThemeConfig interface is not designed for runtime switching:
interface ThemeConfig {
  colors: ColorPalette; // single palette, resolved at build time
  fonts: FontStack;
}
```

## Prior requests

- #42 — "Add dark mode support"
- #87 — "Night theme for accessibility"
- #134 — "Dark theme option"
```

### 文件命名 (Naming the file)

使用简短、描述性的 kebab-case 名称表示概念：`dark-mode.md`、`plugin-system.md`、`graphql-api.md`。名称应足够可识别，让浏览目录的人无需打开文件即可了解被拒绝的内容。

> Use a short, descriptive kebab-case name for the concept: `dark-mode.md`, `plugin-system.md`, `graphql-api.md`. The name should be recognizable enough that someone browsing the directory understands what was rejected without opening the file.

### 编写原因 (Writing the reason)

原因应有实质内容——不是"我们不想要这个"而是为什么。好的原因引用：

> The reason should be substantive — not "we don't want this" but why. Good reasons reference:

- 项目范围或理念（"此项目专注于 X；主题化是下游关注的事"）
  > Project scope or philosophy ("This project focuses on X; theming is a downstream concern")
- 技术约束（"支持此功能需要 Y，与我们的 Z 架构冲突"）
  > Technical constraints ("Supporting this would require Y, which conflicts with our Z architecture")
- 战略决策（"我们选择使用 A 而不是 B，因为……"）
  > Strategic decisions ("We chose to use A instead of B because...")

原因应具有持久性。避免引用临时情况（"我们现在太忙了"）——那些不是真正的拒绝，而是延迟。

> The reason should be durable. Avoid referencing temporary circumstances ("we're too busy right now") — those aren't real rejections, they're deferrals.

## 何时检查 `.out-of-scope/` (When to check `.out-of-scope/`)

在分诊期间（步骤 1：收集上下文），读取 `.out-of-scope/` 中的所有文件。评估新 issue 时：

> During triage (Step 1: Gather context), read all files in `.out-of-scope/`. When evaluating a new issue:

- 检查请求是否匹配现有的超出范围概念
  > Check if the request matches an existing out-of-scope concept
- 按概念相似性匹配，而非关键词——"night theme" 匹配 `dark-mode.md`
  > Matching is by concept similarity, not keyword — "night theme" matches `dark-mode.md`
- 如果匹配，向维护者呈现："这与 `.out-of-scope/dark-mode.md` 相似——我们之前拒绝了因为 [原因]。你还持相同看法吗？"
  > If there's a match, surface it to the maintainer: "This is similar to `.out-of-scope/dark-mode.md` — we rejected this before because [reason]. Do you still feel the same way?"

维护者可能：

> The maintainer may:

- **确认 (Confirm)** — 新 issue 被添加到现有文件的"先前请求"列表中，然后关闭
  > The new issue gets added to the existing file's "Prior requests" list, then closed
- **重新考虑 (Reconsider)** — 超出范围文件被删除或更新，issue 通过正常分诊流程继续
  > The out-of-scope file gets deleted or updated, and the issue proceeds through normal triage
- **不同意 (Disagree)** — issue 相关但不同，通过正常分诊继续
  > The issues are related but distinct, proceed with normal triage

## 何时写入 `.out-of-scope/` (When to write to `.out-of-scope/`)

仅当**增强功能**（不是 bug）被拒绝为 `wontfix` 时。流程：

> Only when an **enhancement** (not a bug) is rejected as `wontfix`. The flow:

1. 维护者决定功能请求超出范围
   > Maintainer decides a feature request is out of scope
2. 检查匹配的 `.out-of-scope/` 文件是否已存在
   > Check if a matching `.out-of-scope/` file already exists
3. 如果是：将新 issue 追加到"先前请求"列表
   > If yes: append the new issue to the "Prior requests" list
4. 如果否：创建新文件，包含概念名称、决定、原因和第一个先前请求
   > If no: create a new file with the concept name, decision, reason, and first prior request
5. 在 issue 上发布评论解释决定并提及 `.out-of-scope/` 文件
   > Post a comment on the issue explaining the decision and mentioning the `.out-of-scope/` file
6. 使用 `wontfix` 标签关闭 issue
   > Close the issue with the `wontfix` label

## 更新或删除超出范围文件 (Updating or removing out-of-scope files)

如果维护者改变对先前被拒绝概念的看法：

> If the maintainer changes their mind about a previously rejected concept:

- 删除 `.out-of-scope/` 文件
  > Delete the `.out-of-scope/` file
- skill 不需要重新打开旧 issue——它们是历史记录
  > The skill does not need to reopen old issues — they're historical records
- 触发重新考虑的新 issue 通过正常分诊继续
  > The new issue that triggered the reconsideration proceeds through normal triage

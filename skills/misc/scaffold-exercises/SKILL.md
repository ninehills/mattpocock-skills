---
name: scaffold-exercises
description: 创建能通过 lint 的练习目录结构，包含章节、问题、解答和讲解。当用户想要搭建练习骨架、创建练习存根或设置新课程章节时使用。 (Create exercise directory structures with sections, problems, solutions, and explainers that pass linting. Use when user wants to scaffold exercises, create exercise stubs, or set up a new course section.)
---

# 搭建练习 (Scaffold Exercises)

创建能通过 `pnpm ai-hero-cli internal lint` 的练习目录结构，然后用 `git commit` 提交。

> Create exercise directory structures that pass `pnpm ai-hero-cli internal lint`, then commit with `git commit`.

## 目录命名 (Directory naming)

- **章节（Sections）**：`exercises/` 内的 `XX-section-name/`（例如 `01-retrieval-skill-building`）
  > **Sections**: `XX-section-name/` inside `exercises/` (e.g., `01-retrieval-skill-building`)
- **练习（Exercises）**：章节内的 `XX.YY-exercise-name/`（例如 `01.03-retrieval-with-bm25`）
  > **Exercises**: `XX.YY-exercise-name/` inside a section (e.g., `01.03-retrieval-with-bm25`)
- 章节编号 = `XX`，练习编号 = `XX.YY`
  > Section number = `XX`, exercise number = `XX.YY`
- 名称使用短横线命名法（小写、连字符）
  > Names are dash-case (lowercase, hyphens)

## 练习变体 (Exercise variants)

每个练习至少需要以下子文件夹之一：
> Each exercise needs at least one of these subfolders:

- `problem/` - 学生工作区，包含 TODO
  > `problem/` - student workspace with TODOs
- `solution/` - 参考实现
  > `solution/` - reference implementation
- `explainer/` - 概念材料，无 TODO
  > `explainer/` - conceptual material, no TODOs

创建存根时，默认使用 `explainer/`，除非计划另有说明。
> When stubbing, default to `explainer/` unless the plan specifies otherwise.

## 必需文件 (Required files)

每个子文件夹（`problem/`、`solution/`、`explainer/`）都需要一个 `readme.md`，它：
> Each subfolder (`problem/`, `solution/`, `explainer/`) needs a `readme.md` that:

- **不能为空**（必须有实际内容，哪怕只有一行标题也可以）
  > Is **not empty** (must have real content, even a single title line works)
- 没有断开的链接
  > Has no broken links

创建存根时，创建一个带有标题和描述的最小 readme：
> When stubbing, create a minimal readme with a title and a description:

```md
# Exercise Title

Description here
```

如果子文件夹有代码，还需要一个 `main.ts`（超过1行）。但对于存根来说，只有 readme 的练习也可以。
> If the subfolder has code, it also needs a `main.ts` (>1 line). But for stubs, a readme-only exercise is fine.

## 工作流 (Workflow)

1. **解析计划** - 提取章节名称、练习名称和变体类型
   > **Parse the plan** - extract section names, exercise names, and variant types
2. **创建目录** - 对每个路径执行 `mkdir -p`
   > **Create directories** - `mkdir -p` for each path
3. **创建存根 readme** - 每个变体文件夹一个带有标题的 `readme.md`
   > **Create stub readmes** - one `readme.md` per variant folder with a title
4. **运行 lint** - `pnpm ai-hero-cli internal lint` 进行验证
   > **Run lint** - `pnpm ai-hero-cli internal lint` to validate
5. **修复错误** - 迭代直到 lint 通过
   > **Fix any errors** - iterate until lint passes

## Lint 规则摘要 (Lint rules summary)

lint 工具（`pnpm ai-hero-cli internal lint`）检查：
> The linter (`pnpm ai-hero-cli internal lint`) checks:

- 每个练习都有子文件夹（`problem/`、`solution/`、`explainer/`）
  > Each exercise has subfolders (`problem/`, `solution/`, `explainer/`)
- 至少存在 `problem/`、`explainer/` 或 `explainer.1/` 之一
  > At least one of `problem/`, `explainer/`, or `explainer.1/` exists
- 主要子文件夹中存在且非空的 `readme.md`
  > `readme.md` exists and is non-empty in the primary subfolder
- 没有 `.gitkeep` 文件
  > No `.gitkeep` files
- 没有 `speaker-notes.md` 文件
  > No `speaker-notes.md` files
- readme 中没有断开的链接
  > No broken links in readmes
- readme 中没有 `pnpm run exercise` 命令
  > No `pnpm run exercise` commands in readmes
- 每个子文件夹都需要 `main.ts`，除非只有 readme
  > `main.ts` required per subfolder unless it's readme-only

## 移动/重命名练习 (Moving/renaming exercises)

重新编号或移动练习时：
> When renumbering or moving exercises:

1. 使用 `git mv`（而非 `mv`）重命名目录 - 保留 git 历史
   > Use `git mv` (not `mv`) to rename directories - preserves git history
2. 更新数字前缀以保持顺序
   > Update the numeric prefix to maintain order
3. 移动后重新运行 lint
   > Re-run lint after moves

示例：
> Example:

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## 示例：从计划创建存根 (Example: stubbing from a plan)

给定如下计划：
> Given a plan like:

```
Section 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

创建：
> Create:

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

然后创建 readme 存根：
> Then create readme stubs:

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```

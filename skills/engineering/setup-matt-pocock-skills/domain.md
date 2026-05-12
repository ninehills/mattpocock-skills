# 领域文档 (Domain Docs)

工程技能在探索代码库时应如何消费此仓库的领域文档。

> How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## 探索前先阅读这些 (Before exploring, read these)

- **`CONTEXT.md`** — 在仓库根目录，或
  > At the repo root, or
- **`CONTEXT-MAP.md`** — 在仓库根目录（如果存在）——它指向每个上下文的一个 `CONTEXT.md`。阅读与主题相关的每个。
  > At the repo root if it exists — it points at one `CONTEXT.md` per context. Read each one relevant to the topic.
- **`docs/adr/`** — 阅读涉及你即将工作区域的 ADR。在多上下文仓库中，也检查 `src/<context>/docs/adr/` 中的上下文范围决策。
  > Read ADRs that touch the area you're about to work in. In multi-context repos, also check `src/<context>/docs/adr/` for context-scoped decisions.

如果这些文件中任何不存在，**静默继续**。不要标记它们的缺失；不要建议预先创建它们。生产者技能（`/grill-with-docs`）会在术语或决策真正被确定时懒创建它们。

> If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The producer skill (`/grill-with-docs`) creates them lazily when terms or decisions actually get resolved.

## 文件结构 (File structure)

单上下文仓库（大多数仓库）：

> Single-context repo (most repos):

```
/
├── CONTEXT.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

多上下文仓库（根目录存在 `CONTEXT-MAP.md`）：

> Multi-context repo (presence of `CONTEXT-MAP.md` at the root):

```
/
├── CONTEXT-MAP.md
├── docs/adr/                          ← 系统级决策 (system-wide decisions)
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                  ← 上下文特定决策 (context-specific decisions)
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```

## 使用词汇表的术语 (Use the glossary's vocabulary)

当你的输出命名领域概念时（在 issue 标题、重构提案、假设、测试名称中），使用 `CONTEXT.md` 中定义的术语。不要漂移到词汇表明确避免的同义词。

> When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

如果你需要的概念还不在词汇表中，这是一个信号——要么你在发明项目不使用的语言（重新考虑），要么存在真正的缺口（为 `/grill-with-docs` 记录它）。

> If the concept you need isn't in the glossary yet, that's a signal — either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/grill-with-docs`).

## 标记 ADR 冲突 (Flag ADR conflicts)

如果你的输出与现有 ADR 矛盾，明确揭示而不是静默覆盖：

> If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _与 ADR-0007（事件溯源订单）矛盾——但值得重新开启因为……_
> _Contradicts ADR-0007 (event-sourced orders) — but worth reopening because…_

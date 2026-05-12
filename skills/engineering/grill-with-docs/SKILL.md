---
name: grill-with-docs
description: 盘问式会话，对照现有领域模型挑战你的计划，精炼术语，并在决策成型时同步更新文档（CONTEXT.md、ADR）。当用户想根据项目语言和已记录决策来压力测试计划时使用。 (Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates documentation (CONTEXT.md, ADRs) inline as decisions crystallise. Use when user wants to stress-test a plan against their project's language and documented decisions.)
---

<what-to-do>

就这个计划的每个方面不停地盘问我，直到我们达成共识。沿着设计树的每个分支，逐一解决决策之间的依赖关系。对每个问题，提供你的推荐答案。

> Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

一次问一个问题，等待每个问题的反馈后再继续。

> Ask the questions one at a time, waiting for feedback on each question before continuing.

如果一个问题可以通过探索代码库来回答，就去探索代码库。

> If a question can be answered by exploring the codebase, explore the codebase instead.

</what-to-do>

<supporting-info>

## 领域感知 (Domain awareness)

在代码库探索过程中，同时寻找现有文档：

> During codebase exploration, also look for existing documentation:

### 文件结构 (File structure)

大多数仓库有单一上下文：

> Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

如果根目录存在 `CONTEXT-MAP.md`，说明仓库有多个上下文。该映射指向每个上下文的位置：

> If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. The map points to where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← 系统级决策 (system-wide decisions)
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← 上下文特定决策 (context-specific decisions)
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

懒创建文件——只在有内容可写时才创建。如果 `CONTEXT.md` 不存在，在第一个术语被确定时创建。如果 `docs/adr/` 不存在，在第一个 ADR 被需要时创建。

> Create files lazily — only when you have something to write. If no `CONTEXT.md` exists, create one when the first term is resolved. If no `docs/adr/` exists, create it when the first ADR is needed.

## 会话过程中 (During the session)

### 对照词汇表质疑 (Challenge against the glossary)

当用户使用的术语与 `CONTEXT.md` 中现有语言冲突时，立即指出。"你的词汇表将'取消'定义为 X，但你似乎指的是 Y——到底是哪个？"

> When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"

### 精炼模糊语言 (Sharpen fuzzy language)

当用户使用模糊或过载的术语时，提出精确的规范术语。"你说的是'账户'——你指的是客户还是用户？它们是不同的东西。"

> When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."

### 讨论具体场景 (Discuss concrete scenarios)

当讨论领域关系时，用具体场景压力测试它们。设计能探测边界情况的场景，迫使用户精确说明概念之间的边界。

> When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### 与代码交叉引用 (Cross-reference with code)

当用户陈述某事如何运作时，检查代码是否一致。如果发现矛盾，揭示它："你的代码取消整个订单，但你刚说可以部分取消——哪个是对的？"

> When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible — which is right?"

### 同步更新 CONTEXT.md (Update CONTEXT.md inline)

当一个术语被确定时，立即更新 `CONTEXT.md`。不要批量处理——在发生时捕获。使用 [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md) 中的格式。

> When a term is resolved, update `CONTEXT.md` right there. Don't batch these up — capture them as they happen. Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

不要将 `CONTEXT.md` 与实现细节耦合。只包含对领域专家有意义的术语。

> Don't couple `CONTEXT.md` to implementation details. Only include terms that are meaningful to domain experts.

### 谨慎提供 ADR (Offer ADRs sparingly)

只有在以下三个条件都满足时才提议创建 ADR：

> Only offer to create an ADR when all three are true:

1. **难以逆转 (Hard to reverse)** — 后来改变主意的成本是实质性的
   > The cost of changing your mind later is meaningful
2. **没有上下文会令人惊讶 (Surprising without context)** — 未来的读者会想"他们为什么这样做？"
   > A future reader will wonder "why did they do it this way?"
3. **真实权衡的结果 (The result of a real trade-off)** — 有真正的替代方案，你基于特定原因选了一个
   > There were genuine alternatives and you picked one for specific reasons

如果三个条件中缺少任何一个，跳过 ADR。使用 [ADR-FORMAT.md](./ADR-FORMAT.md) 中的格式。

> If any of the three is missing, skip the ADR. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).

</supporting-info>

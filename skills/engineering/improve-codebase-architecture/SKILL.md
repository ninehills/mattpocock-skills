---
name: improve-codebase-architecture
description: 在代码库中发现深化机会，依据 CONTEXT.md 中的领域语言和 docs/adr/ 中的决策。当用户想改进架构、发现重构机会、整合紧耦合模块、或使代码库更可测试和 AI 可导航时使用。 (Find deepening opportunities in a codebase, informed by the domain language in CONTEXT.md and the decisions in docs/adr/. Use when the user wants to improve architecture, find refactoring opportunities, consolidate tightly-coupled modules, or make a codebase more testable and AI-navigable.)
---

# 改进代码库架构 (Improve Codebase Architecture)

揭示架构摩擦并提出**深化机会**——将浅层模块转化为深层模块的重构。目标是可测试性和 AI 可导航性。

> Surface architectural friction and propose **deepening opportunities** — refactors that turn shallow modules into deep ones. The aim is testability and AI-navigability.

## 术语表 (Glossary)

在每个建议中精确使用这些术语。一致的语言是关键——不要漂移到"组件"、"服务"、"API"或"边界"。完整定义见 [LANGUAGE.md](LANGUAGE.md)。

> Use these terms exactly in every suggestion. Consistent language is the point — don't drift into "component," "service," "API," or "boundary." Full definitions in [LANGUAGE.md](LANGUAGE.md).

- **模块 (Module)** — 任何有接口和实现的东西（函数、类、包、切片）。
  > Anything with an interface and an implementation (function, class, package, slice).
- **接口 (Interface)** — 调用者使用模块必须知道的一切：类型、不变量、错误模式、排序、配置。不仅仅是类型签名。
  > Everything a caller must know to use the module: types, invariants, error modes, ordering, config. Not just the type signature.
- **实现 (Implementation)** — 内部的代码。
  > The code inside.
- **深度 (Depth)** — 接口处的杠杆率：小接口背后有大量行为。**深** = 高杠杆。**浅** = 接口几乎和实现一样复杂。
  > Leverage at the interface: a lot of behaviour behind a small interface. **Deep** = high leverage. **Shallow** = interface nearly as complex as the implementation.
- **接缝 (Seam)** — 接口所在的位置；一个可以在不原地编辑的情况下改变行为的地方。（用这个，不用"边界"。）
  > Where an interface lives; a place behaviour can be altered without editing in place. (Use this, not "boundary.")
- **适配器 (Adapter)** — 在接缝处满足接口的具体事物。
  > A concrete thing satisfying an interface at a seam.
- **杠杆 (Leverage)** — 调用者从深度中获得的东西。
  > What callers get from depth.
- **局部性 (Locality)** — 维护者从深度中获得的东西：变更、bug、知识集中在一个地方。
  > What maintainers get from depth: change, bugs, knowledge concentrated in one place.

关键原则（完整列表见 [LANGUAGE.md](LANGUAGE.md)）：

> Key principles (see [LANGUAGE.md](LANGUAGE.md) for the full list):

- **删除测试**：想象删除模块。如果复杂性消失了，它是一个透传。如果复杂性在 N 个调用者中重新出现，它是在发挥作用。
  > **Deletion test**: imagine deleting the module. If complexity vanishes, it was a pass-through. If complexity reappears across N callers, it was earning its keep.
- **接口就是测试面。**
  > **The interface is the test surface.**
- **一个适配器 = 假设的接缝。两个适配器 = 真正的接缝。**
  > **One adapter = hypothetical seam. Two adapters = real seam.**

此技能*受*项目领域模型启发。领域语言为好的接缝命名；ADR 记录技能不应重新争论的决策。

> This skill is _informed_ by the project's domain model. The domain language gives names to good seams; ADRs record decisions the skill should not re-litigate.

## 流程 (Process)

### 1. 探索 (Explore)

先阅读项目的领域词汇表和你正在修改区域的任何 ADR。

> Read the project's domain glossary and any ADRs in the area you're touching first.

然后使用 Agent 工具和 `subagent_type=Explore` 遍历代码库。不要遵循僵化的启发式——有机地探索，注意你在哪里体验到摩擦：

> Then use the Agent tool with `subagent_type=Explore` to walk the codebase. Don't follow rigid heuristics — explore organically and note where you experience friction:

- 理解一个概念需要在多少个小模块之间跳转？
  > Where does understanding one concept require bouncing between many small modules?
- 哪些模块是**浅层的**——接口几乎和实现一样复杂？
  > Where are modules **shallow** — interface nearly as complex as the implementation?
- 哪里纯粹函数仅为可测试性而被提取，但真正的 bug 隐藏在它们的调用方式中（没有**局部性**）？
  > Where have pure functions been extracted just for testability, but the real bugs hide in how they're called (no **locality**)?
- 哪里紧耦合的模块在接缝处泄漏？
  > Where do tightly-coupled modules leak across their seams?
- 代码库的哪些部分没有测试，或通过当前接口难以测试？
  > Which parts of the codebase are untested, or hard to test through their current interface?

对任何你怀疑是浅层的模块应用**删除测试**：删除它会集中复杂性，还是只是移动它？"是的，集中了"是你想要的信号。

> Apply the **deletion test** to anything you suspect is shallow: would deleting it concentrate complexity, or just move it? A "yes, concentrates" is the signal you want.

### 2. 呈现候选 (Present candidates)

呈现深化机会的编号列表。每个候选包含：

> Present a numbered list of deepening opportunities. For each candidate:

- **文件 (Files)** — 涉及哪些文件/模块
  > Which files/modules are involved
- **问题 (Problem)** — 当前架构为什么造成摩擦
  > Why the current architecture is causing friction
- **方案 (Solution)** — 用简单英语描述什么会改变
  > Plain English description of what would change
- **收益 (Benefits)** — 用局部性和杠杆来解释，以及测试将如何改善
  > Explained in terms of locality and leverage, and also in how tests would improve

**使用 CONTEXT.md 词汇表描述领域，使用 [LANGUAGE.md](LANGUAGE.md) 词汇表描述架构。** 如果 `CONTEXT.md` 定义了"订单"，就说"订单接收模块"——而不是"FooBarHandler"，也不是"订单服务"。

> **Use CONTEXT.md vocabulary for the domain, and [LANGUAGE.md](LANGUAGE.md) vocabulary for the architecture.** If `CONTEXT.md` defines "Order," talk about "the Order intake module" — not "the FooBarHandler," and not "the Order service."

**ADR 冲突**：如果候选与现有 ADR 矛盾，只在摩擦足够大以至于值得重新审视 ADR 时才指出。清晰标记（例如 _"与 ADR-0007 矛盾——但值得重新开启因为……"_）。不要列出 ADR 禁止的所有理论重构。

> **ADR conflicts**: if a candidate contradicts an existing ADR, only surface it when the friction is real enough to warrant revisiting the ADR. Mark it clearly (e.g. _"contradicts ADR-0007 — but worth reopening because…"_). Don't list every theoretical refactor an ADR forbids.

不要提出接口。问用户："你想探索哪些？"

> Do NOT propose interfaces yet. Ask the user: "Which of these would you like to explore?"

### 3. 盘问循环 (Grilling loop)

一旦用户选择了一个候选，进入盘问对话。和他们一起走设计树——约束、依赖、深化模块的形状、接缝后面是什么、哪些测试能存活。

> Once the user picks a candidate, drop into a grilling conversation. Walk the design tree with them — constraints, dependencies, the shape of the deepened module, what sits behind the seam, what tests survive.

决策成型时副作用同步发生：

> Side effects happen inline as decisions crystallize:

- **用 `CONTEXT.md` 中没有的概念命名深化模块？** 将术语添加到 `CONTEXT.md`——与 `/grill-with-docs` 相同的纪律（见 [CONTEXT-FORMAT.md](../grill-with-docs/CONTEXT-FORMAT.md)）。如果文件不存在则懒创建。
  > **Naming a deepened module after a concept not in `CONTEXT.md`?** Add the term to `CONTEXT.md` — same discipline as `/grill-with-docs` (see [CONTEXT-FORMAT.md](../grill-with-docs/CONTEXT-FORMAT.md)). Create the file lazily if it doesn't exist.
- **在对话中精炼模糊术语？** 立即更新 `CONTEXT.md`。
  > **Sharpening a fuzzy term during the conversation?** Update `CONTEXT.md` right there.
- **用户以有分量的理由拒绝候选？** 提供 ADR，措辞为：_"想让我把这个记录为 ADR，以便未来的架构审查不会重新提出它吗？"_ 只在理由确实会被未来探索者需要以避免重新提出同样的东西时才提供——跳过临时性的理由（"现在不值得"）和不言自明的理由。见 [ADR-FORMAT.md](../grill-with-docs/ADR-FORMAT.md)。
  > **User rejects the candidate with a load-bearing reason?** Offer an ADR, framed as: _"Want me to record this as an ADR so future architecture reviews don't re-suggest it?"_ Only offer when the reason would actually be needed by a future explorer to avoid re-suggesting the same thing — skip ephemeral reasons ("not worth it right now") and self-evident ones. See [ADR-FORMAT.md](../grill-with-docs/ADR-FORMAT.md).
- **想为深化模块探索替代接口？** 见 [INTERFACE-DESIGN.md](INTERFACE-DESIGN.md)。
  > **Want to explore alternative interfaces for the deepened module?** See [INTERFACE-DESIGN.md](INTERFACE-DESIGN.md).

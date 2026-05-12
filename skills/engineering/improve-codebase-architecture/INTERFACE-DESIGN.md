# 接口设计 (Interface Design)

当用户想为选定的深化候选探索替代接口时，使用此并行子 agent 模式。基于"设计两次"（Ousterhout）——你的第一个想法不太可能是最好的。

> When the user wants to explore alternative interfaces for a chosen deepening candidate, use this parallel sub-agent pattern. Based on "Design It Twice" (Ousterhout) — your first idea is unlikely to be the best.

使用 [LANGUAGE.md](LANGUAGE.md) 中的词汇——**模块**、**接口**、**接缝**、**适配器**、**杠杆**。

> Uses the vocabulary in [LANGUAGE.md](LANGUAGE.md) — **module**, **interface**, **seam**, **adapter**, **leverage**.

## 流程 (Process)

### 1. 框定问题空间 (Frame the problem space)

在生成子 agent 之前，为选定候选编写面向用户的问题空间解释：

> Before spawning sub-agents, write a user-facing explanation of the problem space for the chosen candidate:

- 任何新接口需要满足的约束
  > The constraints any new interface would need to satisfy
- 它将依赖的依赖，以及它们属于哪个类别（见 [DEEPENING.md](DEEPENING.md)）
  > The dependencies it would rely on, and which category they fall into (see [DEEPENING.md](DEEPENING.md))
- 一个粗略的示例代码草图来具象化约束——不是提案，只是让约束具体化的方式
  > A rough illustrative code sketch to ground the constraints — not a proposal, just a way to make the constraints concrete

展示给用户，然后立即进入步骤 2。用户在子 agent 并行工作时阅读和思考。

> Show this to the user, then immediately proceed to Step 2. The user reads and thinks while the sub-agents work in parallel.

### 2. 生成子 agent (Spawn sub-agents)

使用 Agent 工具并行生成 3+ 个子 agent。每个必须为深化模块产生**截然不同**的接口。

> Spawn 3+ sub-agents in parallel using the Agent tool. Each must produce a **radically different** interface for the deepened module.

用单独的技术简报提示每个子 agent（文件路径、耦合细节、[DEEPENING.md](DEEPENING.md) 中的依赖类别、接缝后面是什么）。简报独立于步骤 1 中面向用户的问题空间解释。给每个 agent 不同的设计约束：

> Prompt each sub-agent with a separate technical brief (file paths, coupling details, dependency category from [DEEPENING.md](DEEPENING.md), what sits behind the seam). The brief is independent of the user-facing problem-space explanation in Step 1. Give each agent a different design constraint:

- Agent 1: "最小化接口——目标最多 1-3 个入口点。最大化每个入口点的杠杆。"
  > "Minimize the interface — aim for 1–3 entry points max. Maximise leverage per entry point."
- Agent 2: "最大化灵活性——支持多种用例和扩展。"
  > "Maximise flexibility — support many use cases and extension."
- Agent 3: "为最常见调用者优化——使默认情况变得简单。"
  > "Optimise for the most common caller — make the default case trivial."
- Agent 4（如适用）: "围绕端口与适配器设计跨接缝依赖。"
  > "Design around ports & adapters for cross-seam dependencies."

在简报中包含 [LANGUAGE.md](LANGUAGE.md) 词汇和 CONTEXT.md 词汇，使每个子 agent 的命名与架构语言和项目领域语言一致。

> Include both [LANGUAGE.md](LANGUAGE.md) vocabulary and CONTEXT.md vocabulary in the brief so each sub-agent names things consistently with the architecture language and the project's domain language.

每个子 agent 输出：

> Each sub-agent outputs:

1. 接口（类型、方法、参数——加上不变量、排序、错误模式）
   > Interface (types, methods, params — plus invariants, ordering, error modes)
2. 展示调用者如何使用的用法示例
   > Usage example showing how callers use it
3. 实现在接缝后面隐藏了什么
   > What the implementation hides behind the seam
4. 依赖策略和适配器（见 [DEEPENING.md](DEEPENING.md)）
   > Dependency strategy and adapters (see [DEEPENING.md](DEEPENING.md))
5. 权衡——杠杆在哪里高，在哪里薄
   > Trade-offs — where leverage is high, where it's thin

### 3. 呈现和比较 (Present and compare)

按顺序呈现设计以便用户理解每个，然后用文字进行对比。按**深度**（接口处的杠杆）、**局部性**（变更集中在哪里）和**接缝位置**进行对比。

> Present designs sequentially so the user can absorb each one, then compare them in prose. Contrast by **depth** (leverage at the interface), **locality** (where change concentrates), and **seam placement**.

比较后，给出你自己的推荐：你认为哪个设计最强以及为什么。如果不同设计的元素可以很好地组合，提出混合方案。要有主见——用户想要有力的解读，而不是菜单。

> After comparing, give your own recommendation: which design you think is strongest and why. If elements from different designs would combine well, propose a hybrid. Be opinionated — the user wants a strong read, not a menu.

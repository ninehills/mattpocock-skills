---
name: design-an-interface
description: 使用并行子 agent 为模块生成多个截然不同的接口设计。当用户想要设计 API、探索接口选项、比较模块形态或提到"design it twice"时使用。 (Generate multiple radically different interface designs for a module using parallel sub-agents. Use when user wants to design an API, explore interface options, compare module shapes, or mentions "design it twice".)
---

# 设计接口 (Design an Interface)

基于《软件设计哲学》中的"设计两次"：你的第一个想法不太可能是最好的。生成多个截然不同的设计，然后进行比较。

> Based on "Design It Twice" from "A Philosophy of Software Design": your first idea is unlikely to be the best. Generate multiple radically different designs, then compare.

## 工作流 (Workflow)

### 1. 收集需求

设计之前，理解：
> Before designing, understand:

- [ ] 该模块解决什么问题？
  > What problem does this module solve?
- [ ] 调用者是谁？（其他模块、外部用户、测试）
  > Who are the callers? (other modules, external users, tests)
- [ ] 关键操作是什么？
  > What are the key operations?
- [ ] 有什么约束？（性能、兼容性、现有模式）
  > Any constraints? (performance, compatibility, existing patterns)
- [ ] 什么应该隐藏在内部 vs 暴露在外？
  > What should be hidden inside vs exposed?

询问："这个模块需要做什么？谁会使用它？"

> Ask: "What does this module need to do? Who will use it?"

### 2. 生成设计（并行子 Agent）

同时启动 3+ 个子 agent。每个必须产生一个**截然不同的**方法。

> Spawn 3+ sub-agents simultaneously using Task tool. Each must produce a **radically different** approach.

```
Prompt template for each sub-agent:

Design an interface for: [module description]

Requirements: [gathered requirements]

Constraints for this design: [assign a different constraint to each agent]
- Agent 1: "Minimize method count - aim for 1-3 methods max"
- Agent 2: "Maximize flexibility - support many use cases"
- Agent 3: "Optimize for the most common case"
- Agent 4: "Take inspiration from [specific paradigm/library]"

Output format:
1. Interface signature (types/methods)
2. Usage example (how caller uses it)
3. What this design hides internally
4. Trade-offs of this approach
```

### 3. 展示设计

展示每个设计，包括：
> Show each design with:

1. **接口签名** - 类型、方法、参数
   > **Interface signature** - types, methods, params
2. **使用示例** - 调用者在实际中如何使用
   > **Usage examples** - how callers actually use it in practice
3. **它隐藏了什么** - 保持在内部的复杂性
   > **What it hides** - complexity kept internal

按顺序展示设计，让用户在比较之前能理解每个方法。

> Present designs sequentially so user can absorb each approach before comparison.

### 4. 比较设计

展示所有设计后，从以下方面进行比较：
> After showing all designs, compare them on:

- **接口简洁性**：更少的方法、更简单的参数
  > **Interface simplicity**: fewer methods, simpler params
- **通用 vs 专用**：灵活性 vs 专注度
  > **General-purpose vs specialized**: flexibility vs focus
- **实现效率**：形态是否允许高效的内部实现？
  > **Implementation efficiency**: does shape allow efficient internals?
- **深度**：小接口隐藏重大复杂性（好）vs 大接口实现浅薄（差）
  > **Depth**: small interface hiding significant complexity (good) vs large interface with thin implementation (bad)
- **易于正确使用** vs **易于误用**
  > **Ease of correct use** vs **ease of misuse**

用文字讨论权衡，不用表格。突出设计差异最大的地方。

> Discuss trade-offs in prose, not tables. Highlight where designs diverge most.

### 5. 综合

通常最好的设计结合了多个选项的见解。询问：
> Often the best design combines insights from multiple options. Ask:

- "哪个设计最适合你的主要用例？"
  > "Which design best fits your primary use case?"
- "其他设计中有什么值得融入的元素吗？"
  > "Any elements from other designs worth incorporating?"

## 评估标准 (Evaluation Criteria)

来自《软件设计哲学》：
> From "A Philosophy of Software Design":

**接口简洁性**：更少的方法、更简单的参数 = 更容易学习和正确使用。
> **Interface simplicity**: Fewer methods, simpler params = easier to learn and use correctly.

**通用性**：可以在不更改的情况下处理未来的用例。但要警惕过度泛化。
> **General-purpose**: Can handle future use cases without changes. But beware over-generalization.

**实现效率**：接口形态是否允许高效实现？还是迫使内部实现变得笨拙？
> **Implementation efficiency**: Does interface shape allow efficient implementation? Or force awkward internals?

**深度**：小接口隐藏重大复杂性 = 深模块（好）。大接口实现浅薄 = 浅模块（避免）。
> **Depth**: Small interface hiding significant complexity = deep module (good). Large interface with thin implementation = shallow module (avoid).

## 反模式 (Anti-Patterns)

- 不要让子 agent 产生相似的设计 - 强制要求截然不同
  > Don't let sub-agents produce similar designs - enforce radical difference
- 不要跳过比较 - 价值在于对比
  > Don't skip comparison - the value is in contrast
- 不要实现 - 这纯粹是关于接口形态
  > Don't implement - this is purely about interface shape
- 不要基于实现工作量来评估
  > Don't evaluate based on implementation effort

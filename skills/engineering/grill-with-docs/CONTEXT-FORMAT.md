# CONTEXT.md 格式 (CONTEXT.md Format)

## 结构 (Structure)

```md
# {上下文名称 (Context Name)}

{一两句话描述这个上下文是什么以及为什么存在。}
{One or two sentence description of what this context is and why it exists.}

## 语言 (Language)

**订单 (Order)**:
{对该术语的简明描述}
{A concise description of the term}
_避免 (Avoid)_: 采购、交易 (Purchase, transaction)

**发票 (Invoice)**:
交付后发送给客户的付款请求。
_Avoid_: Bill, payment request

**客户 (Customer)**:
下订单的个人或组织。
_Avoid_: Client, buyer, account

## 关系 (Relationships)

- 一个**订单**产生一张或多张**发票**
- 一张**发票**属于恰好一个**客户**
- An **Order** produces one or more **Invoices**
- An **Invoice** belongs to exactly one **Customer**

## 示例对话 (Example dialogue)

> **开发者 (Dev):** "当**客户**下一个**订单**时，我们会立即创建**发票**吗？"
> **领域专家 (Domain expert):** "不会——**发票**只有在**履行**确认后才生成。"

## 已标记的模糊性 (Flagged ambiguities)

- "账户"曾同时用来指**客户**和**用户**——已解决：它们是不同的概念。
- "account" was used to mean both **Customer** and **User** — resolved: these are distinct concepts.
```

## 规则 (Rules)

- **要有主见 (Be opinionated)** — 当同一概念有多个词时，选最好的一个，将其他的列为要避免的别名。
  > When multiple words exist for the same concept, pick the best one and list the others as aliases to avoid.
- **明确标记冲突 (Flag conflicts explicitly)** — 如果一个术语被模糊使用，在"已标记的模糊性"中指出并给出清晰的解决方案。
  > If a term is used ambiguously, call it out in "Flagged ambiguities" with a clear resolution.
- **保持定义精炼 (Keep definitions tight)** — 最多一句话。定义它*是*什么，而不是它做什么。
  > One sentence max. Define what it IS, not what it does.
- **展示关系 (Show relationships)** — 使用粗体术语名称，在明显处表达基数。
  > Use bold term names and express cardinality where obvious.
- **只包含此项目上下文特有的术语 (Only include terms specific to this project's context)** — 通用编程概念（超时、错误类型、工具模式）即使项目大量使用也不属于这里。添加术语前先问：这是此上下文独有的概念还是通用编程概念？只有前者才属于这里。
  > General programming concepts (timeouts, error types, utility patterns) don't belong even if the project uses them extensively. Before adding a term, ask: is this a concept unique to this context, or a general programming concept? Only the former belongs.
- **在自然聚类出现时将术语分组到子标题下 (Group terms under subheadings)** — 如果所有术语属于一个单一内聚领域，扁平列表即可。
  > When natural clusters emerge. If all terms belong to a single cohesive area, a flat list is fine.
- **写一段示例对话 (Write an example dialogue)** — 开发者和领域专家之间的对话，展示术语如何自然交互并澄清相关概念之间的边界。
  > A conversation between a dev and a domain expert that demonstrates how the terms interact naturally and clarifies boundaries between related concepts.

## 单上下文 vs 多上下文仓库 (Single vs multi-context repos)

**单上下文（大多数仓库）(Single context (most repos))：** 仓库根目录一个 `CONTEXT.md`。

> One `CONTEXT.md` at the repo root.

**多上下文 (Multiple contexts)：** 仓库根目录一个 `CONTEXT-MAP.md` 列出上下文、它们的位置及相互关系：

> A `CONTEXT-MAP.md` at the repo root lists the contexts, where they live, and how they relate to each other:

```md
# 上下文映射 (Context Map)

## 上下文 (Contexts)

- [订单](./src/ordering/CONTEXT.md) — 接收和跟踪客户订单 (receives and tracks customer orders)
- [账单](./src/billing/CONTEXT.md) — 生成发票和处理付款 (generates invoices and processes payments)
- [履行](./src/fulfillment/CONTEXT.md) — 管理仓库拣货和发货 (manages warehouse picking and shipping)

## 关系 (Relationships)

- **订单 → 履行**: 订单发出 `OrderPlaced` 事件；履行消费它们开始拣货
- **履行 → 账单**: 履行发出 `ShipmentDispatched` 事件；账单消费它们生成发票
- **订单 ↔ 账单**: 共享 `CustomerId` 和 `Money` 类型
- **Ordering → Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment → Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

技能会推断适用哪种结构：

> The skill infers which structure applies:

- 如果 `CONTEXT-MAP.md` 存在，读取它来找到上下文
  > If `CONTEXT-MAP.md` exists, read it to find contexts
- 如果只有根目录 `CONTEXT.md` 存在，单上下文
  > If only a root `CONTEXT.md` exists, single context
- 如果都不存在，在第一个术语被确定时懒创建根目录 `CONTEXT.md`
  > If neither exists, create a root `CONTEXT.md` lazily when the first term is resolved

当存在多个上下文时，推断当前主题关联哪个。如果不明确，询问。

> When multiple contexts exist, infer which one the current topic relates to. If unclear, ask.

# ADR 格式 (ADR Format)

ADR 存放在 `docs/adr/` 中，使用顺序编号：`0001-slug.md`、`0002-slug.md` 等。

> ADRs live in `docs/adr/` and use sequential numbering: `0001-slug.md`, `0002-slug.md`, etc.

懒创建 `docs/adr/` 目录——只在第一个 ADR 被需要时创建。

> Create the `docs/adr/` directory lazily — only when the first ADR is needed.

## 模板 (Template)

```md
# {决策的简短标题 (Short title of the decision)}

{1-3 句话：上下文是什么，我们决定了什么，以及为什么。}
{1-3 sentences: what's the context, what did we decide, and why.}
```

就这样。一个 ADR 可以是一个段落。价值在于记录*做了*一个决策以及*为什么*——而不是填写各个部分。

> That's it. An ADR can be a single paragraph. The value is in recording *that* a decision was made and *why* — not in filling out sections.

## 可选部分 (Optional sections)

只在它们带来真正价值时才包含。大多数 ADR 不需要这些。

> Only include these when they add genuine value. Most ADRs won't need them.

- **Status 前置元数据 (Status frontmatter)** (`proposed | accepted | deprecated | superseded by ADR-NNNN`) — 当决策被重新审视时有用
  > Useful when decisions are revisited
- **考虑的选项 (Considered Options)** — 仅当被拒绝的替代方案值得记忆时
  > Only when the rejected alternatives are worth remembering
- **后果 (Consequences)** — 仅当需要指出非显而易见的下游影响时
  > Only when non-obvious downstream effects need to be called out

## 编号 (Numbering)

扫描 `docs/adr/` 找到最高已存在编号并加一。

> Scan `docs/adr/` for the highest existing number and increment by one.

## 何时提供 ADR (When to offer an ADR)

以下三个条件必须全部满足：

> All three of these must be true:

1. **难以逆转 (Hard to reverse)** — 后来改变主意的成本是实质性的
   > The cost of changing your mind later is meaningful
2. **没有上下文会令人惊讶 (Surprising without context)** — 未来的读者会看代码想"他们到底为什么这样做？"
   > A future reader will look at the code and wonder "why on earth did they do it this way?"
3. **真实权衡的结果 (The result of a real trade-off)** — 有真正的替代方案，你基于特定原因选了一个
   > There were genuine alternatives and you picked one for specific reasons

如果决策容易逆转，跳过它——你反正会逆转。如果它不令人惊讶，没人会好奇为什么。如果没有真正的替代方案，除了"我们做了显而易见的事"之外没什么可记录的。

> If a decision is easy to reverse, skip it — you'll just reverse it. If it's not surprising, nobody will wonder why. If there was no real alternative, there's nothing to record beyond "we did the obvious thing."

### 什么情况符合条件 (What qualifies)

- **架构形态 (Architectural shape)** — "我们使用 monorepo。" "写模型是事件溯源的，读模型投影到 Postgres。"
  > "We're using a monorepo." "The write model is event-sourced, the read model is projected into Postgres."
- **上下文间的集成模式 (Integration patterns between contexts)** — "订单和账单通过领域事件通信，而不是同步 HTTP。"
  > "Ordering and Billing communicate via domain events, not synchronous HTTP."
- **带来锁定的技术选择 (Technology choices that carry lock-in)** — 数据库、消息总线、认证提供者、部署目标。不是每个库——只是那些需要一个季度才能替换的。
  > Database, message bus, auth provider, deployment target. Not every library — just the ones that would take a quarter to swap out.
- **边界和范围决策 (Boundary and scope decisions)** — "客户数据由客户上下文拥有；其他上下文仅通过 ID 引用。" 明确的"不"和"是"一样有价值。
  > "Customer data is owned by the Customer context; other contexts reference it by ID only." The explicit no-s are as valuable as the yes-s.
- **刻意偏离显而易见的路径 (Deliberate deviations from the obvious path)** — "我们使用手动 SQL 而不是 ORM 因为 X。" 任何合理读者会假设相反做法的地方。这些能阻止下一个工程师"修复"一个刻意的设计。
  > "We're using manual SQL instead of an ORM because X." Anything where a reasonable reader would assume the opposite. These stop the next engineer from "fixing" something that was deliberate.
- **代码中不可见的约束 (Constraints not visible in the code)** — "我们不能使用 AWS 因为合规要求。" "响应时间必须在 200ms 以内因为合作伙伴 API 契约。"
  > "We can't use AWS because of compliance requirements." "Response times must be under 200ms because of the partner API contract."
- **被拒绝的替代方案中拒绝原因非显而易见的 (Rejected alternatives when the rejection is non-obvious)** — 如果你考虑过 GraphQL 但因微妙原因选了 REST，记录它——否则六个月后会有人再次建议 GraphQL。
  > If you considered GraphQL and picked REST for subtle reasons, record it — otherwise someone will suggest GraphQL again in six months.

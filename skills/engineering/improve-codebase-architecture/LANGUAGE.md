# 语言 (Language)

此技能每个建议的共享词汇。精确使用这些术语——不要替换"组件"、"服务"、"API"或"边界"。一致的语言是全部意义所在。

> Shared vocabulary for every suggestion this skill makes. Use these terms exactly — don't substitute "component," "service," "API," or "boundary." Consistent language is the whole point.

## 术语 (Terms)

**模块 (Module)**
任何有接口和实现的东西。刻意与规模无关——同样适用于函数、类、包或跨层切片。
_避免 (Avoid)_: 单元、组件、服务 (unit, component, service)。

> Anything with an interface and an implementation. Deliberately scale-agnostic — applies equally to a function, class, package, or tier-spanning slice.

**接口 (Interface)**
调用者正确使用模块必须知道的一切。包括类型签名，但也包括不变量、排序约束、错误模式、所需配置和性能特征。
_避免 (Avoid)_: API、签名（太窄——仅指类型层面的表面）。

> Everything a caller must know to use the module correctly. Includes the type signature, but also invariants, ordering constraints, error modes, required configuration, and performance characteristics.

**实现 (Implementation)**
模块内部的东西——它的代码体。与**适配器**不同：一个东西可以是小适配器但有大实现（Postgres 仓库），或大适配器但有小实现（内存 fake）。当接缝是话题时用"适配器"；否则用"实现"。

> What's inside a module — its body of code. Distinct from **Adapter**: a thing can be a small adapter with a large implementation (a Postgres repo) or a large adapter with a small implementation (an in-memory fake). Reach for "adapter" when the seam is the topic; "implementation" otherwise.

**深度 (Depth)**
接口处的杠杆——调用者（或测试）每单位需要学习的接口可以操作的行为量。当大量行为位于小接口背后时，模块是**深的**。当接口几乎和实现一样复杂时，模块是**浅的**。

> Leverage at the interface — the amount of behaviour a caller (or test) can exercise per unit of interface they have to learn. A module is **deep** when a large amount of behaviour sits behind a small interface. A module is **shallow** when the interface is nearly as complex as the implementation.

**接缝 (Seam)** _(来自 Michael Feathers)_
一个你可以在不原地编辑的情况下改变行为的地方。模块接口所在的*位置*。选择在哪里放置接缝是一个独立的设计决策，与接缝后面放什么无关。
_避免 (Avoid)_: 边界（与 DDD 的限界上下文过载）。

> A place where you can alter behaviour without editing in that place. The *location* at which a module's interface lives. Choosing where to put the seam is its own design decision, distinct from what goes behind it.

**适配器 (Adapter)**
在接缝处满足接口的具体事物。描述*角色*（它填充什么槽位），而不是实质（里面是什么）。

> A concrete thing that satisfies an interface at a seam. Describes *role* (what slot it fills), not substance (what's inside).

**杠杆 (Leverage)**
调用者从深度中获得的东西。每单位需要学习的接口获得更多能力。一个实现在 N 个调用点和 M 个测试中得到回报。

> What callers get from depth. More capability per unit of interface they have to learn. One implementation pays back across N call sites and M tests.

**局部性 (Locality)**
维护者从深度中获得的东西。变更、bug、知识和验证集中在一个地方，而不是分散在调用者中。修复一次，到处修复。

> What maintainers get from depth. Change, bugs, knowledge, and verification concentrate at one place rather than spreading across callers. Fix once, fixed everywhere.

## 原则 (Principles)

- **深度是接口的属性，不是实现的属性。** 一个深模块可以在内部由小的、可 mock 的、可替换的部件组成——它们只是不是接口的一部分。模块可以有**内部接缝**（对其实现私有，由其自己的测试使用）以及接口处的**外部接缝**。
  > **Depth is a property of the interface, not the implementation.** A deep module can be internally composed of small, mockable, swappable parts — they just aren't part of the interface. A module can have **internal seams** (private to its implementation, used by its own tests) as well as the **external seam** at its interface.
- **删除测试。** 想象删除模块。如果复杂性消失了，模块没有隐藏任何东西（它是透传）。如果复杂性在 N 个调用者中重新出现，模块是在发挥作用。
  > **The deletion test.** Imagine deleting the module. If complexity vanishes, the module wasn't hiding anything (it was a pass-through). If complexity reappears across N callers, the module was earning its keep.
- **接口就是测试面。** 调用者和测试穿越同一个接缝。如果你想测试*绕过*接口，模块可能是错误的形状。
  > **The interface is the test surface.** Callers and tests cross the same seam. If you want to test *past* the interface, the module is probably the wrong shape.
- **一个适配器意味着假设的接缝。两个适配器意味着真正的接缝。** 除非有什么东西真正在接缝处变化，否则不要引入接缝。
  > **One adapter means a hypothetical seam. Two adapters means a real one.** Don't introduce a seam unless something actually varies across it.

## 关系 (Relationships)

- **模块**恰好有一个**接口**（它呈现给调用者和测试的表面）。
  > A **Module** has exactly one **Interface** (the surface it presents to callers and tests).
- **深度**是**模块**的属性，相对于其**接口**测量。
  > **Depth** is a property of a **Module**, measured against its **Interface**.
- **接缝**是**模块**的**接口**所在的地方。
  > A **Seam** is where a **Module**'s **Interface** lives.
- **适配器**位于**接缝**并满足**接口**。
  > An **Adapter** sits at a **Seam** and satisfies the **Interface**.
- **深度**为调用者产生**杠杆**，为维护者产生**局部性**。
  > **Depth** produces **Leverage** for callers and **Locality** for maintainers.

## 被拒绝的框架 (Rejected framings)

- **深度即实现行数与接口行数的比率**（Ousterhout）：奖励填充实现。我们改用深度即杠杆。
  > **Depth as ratio of implementation-lines to interface-lines** (Ousterhout): rewards padding the implementation. We use depth-as-leverage instead.
- **"接口"即 TypeScript `interface` 关键字或类的公共方法**：太窄——这里的接口包括调用者必须知道的每个事实。
  > **"Interface" as the TypeScript `interface` keyword or a class's public methods**: too narrow — interface here includes every fact a caller must know.
- **"边界"**：与 DDD 的限界上下文过载。说**接缝**或**接口**。
  > **"Boundary"**: overloaded with DDD's bounded context. Say **seam** or **interface**.

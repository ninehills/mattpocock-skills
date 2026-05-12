# 何时使用 Mock (When to Mock)

仅在**系统边界**处使用 Mock：

> Mock at **system boundaries** only:

- 外部 API（支付、邮件等）
  > External APIs (payment, email, etc.)
- 数据库（有时——优先使用测试数据库）
  > Databases (sometimes - prefer test DB)
- 时间/随机性
  > Time/randomness
- 文件系统（有时）
  > File system (sometimes)

不要 Mock：

> Don't mock:

- 你自己的类/模块
  > Your own classes/modules
- 内部协作者
  > Internal collaborators
- 任何你控制的东西
  > Anything you control

## 为可 Mock 性设计 (Designing for Mockability)

在系统边界处，设计易于 Mock 的接口：

> At system boundaries, design interfaces that are easy to mock:

**1. 使用依赖注入 (Use dependency injection)**

传入外部依赖而不是内部创建：

> Pass external dependencies in rather than creating them internally:

```typescript
// 易于 Mock (Easy to mock)
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// 难以 Mock (Hard to mock)
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}
```

**2. 优先使用 SDK 风格接口而非通用 fetcher (Prefer SDK-style interfaces over generic fetchers)**

为每个外部操作创建特定函数，而不是一个带有条件逻辑的通用函数：

> Create specific functions for each external operation instead of one generic function with conditional logic:

```typescript
// 好 (GOOD): 每个函数独立可 Mock
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// 坏 (BAD): Mock 需要内部条件逻辑
const api = {
  fetch: (endpoint, options) => fetch(endpoint, options),
};
```

SDK 方法意味着：

> The SDK approach means:

- 每个 mock 返回一个特定形状
  > Each mock returns one specific shape
- 测试设置中没有条件逻辑
  > No conditional logic in test setup
- 更容易看到测试操作了哪些端点
  > Easier to see which endpoints a test exercises
- 每端点类型安全
  > Type safety per endpoint

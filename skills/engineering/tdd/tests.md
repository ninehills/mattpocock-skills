# 好测试和坏测试 (Good and Bad Tests)

## 好测试 (Good Tests)

**集成风格 (Integration-style)**：通过真实接口测试，而不是内部部件的 mock。

> Test through real interfaces, not mocks of internal parts.

```typescript
// 好 (GOOD): 测试可观察行为
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特征 (Characteristics)：

- 测试用户/调用者关心的行为
  > Tests behavior users/callers care about
- 仅使用公共 API
  > Uses public API only
- 在内部重构中存活
  > Survives internal refactors
- 描述"什么"，不是"如何"
  > Describes WHAT, not HOW
- 每个测试一个逻辑断言
  > One logical assertion per test

## 坏测试 (Bad Tests)

**实现细节测试 (Implementation-detail tests)**：与内部结构耦合。

> Coupled to internal structure.

```typescript
// 坏 (BAD): 测试实现细节
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

红旗 (Red flags)：

- Mock 内部协作者
  > Mocking internal collaborators
- 测试私有方法
  > Testing private methods
- 断言调用次数/顺序
  > Asserting on call counts/order
- 在无行为变更的重构时测试失败
  > Test breaks when refactoring without behavior change
- 测试名称描述"如何"而不是"什么"
  > Test name describes HOW not WHAT
- 通过外部方式而不是接口验证
  > Verifying through external means instead of interface

```typescript
// 坏 (BAD): 绕过接口验证
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// 好 (GOOD): 通过接口验证
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```

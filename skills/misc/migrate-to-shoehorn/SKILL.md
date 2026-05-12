---
name: migrate-to-shoehorn
description: 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。当用户提到 shoehorn、想要替换测试中的 `as` 或需要部分测试数据时使用。 (Migrate test files from `as` type assertions to @total-typescript/shoehorn. Use when user mentions shoehorn, wants to replace `as` in tests, or needs partial test data.)
---

# 迁移到 Shoehorn (Migrate to Shoehorn)

## 为什么用 shoehorn？

`shoehorn` 让你在测试中传递部分数据，同时保持 TypeScript 类型安全。它用类型安全的替代方案替换 `as` 断言。

> `shoehorn` lets you pass partial data in tests while keeping TypeScript happy. It replaces `as` assertions with type-safe alternatives.

**仅限测试代码。** 永远不要在生产代码中使用 shoehorn。

> **Test code only.** Never use shoehorn in production code.

测试中使用 `as` 的问题：
> Problems with `as` in tests:

- 被训练成不要使用它
  > Trained not to use it
- 必须手动指定目标类型
  > Must manually specify target type
- 对于故意错误的数据需要双重 as（`as unknown as Type`）
  > Double-as (`as unknown as Type`) for intentionally wrong data

## 安装 (Install)

```bash
npm i @total-typescript/shoehorn
```

## 迁移模式 (Migration patterns)

### 属性多但只需少量的大对象

之前：
> Before:

```ts
type Request = {
  body: { id: string };
  headers: Record<string, string>;
  cookies: Record<string, string>;
  // ...20 more properties
};

it("gets user by id", () => {
  // Only care about body.id but must fake entire Request
  getUser({
    body: { id: "123" },
    headers: {},
    cookies: {},
    // ...fake all 20 properties
  });
});
```

之后：
> After:

```ts
import { fromPartial } from "@total-typescript/shoehorn";

it("gets user by id", () => {
  getUser(
    fromPartial({
      body: { id: "123" },
    }),
  );
});
```

### `as Type` → `fromPartial()`

之前：
> Before:

```ts
getUser({ body: { id: "123" } } as Request);
```

之后：
> After:

```ts
import { fromPartial } from "@total-typescript/shoehorn";

getUser(fromPartial({ body: { id: "123" } }));
```

### `as unknown as Type` → `fromAny()`

之前：
> Before:

```ts
getUser({ body: { id: 123 } } as unknown as Request); // wrong type on purpose
```

之后：
> After:

```ts
import { fromAny } from "@total-typescript/shoehorn";

getUser(fromAny({ body: { id: 123 } }));
```

## 各函数使用场景 (When to use each)

| 函数 | 使用场景 |
| --------------- | -------------------------------------------------- |
| `fromPartial()` | 传递仍能通过类型检查的部分数据 |
| `fromAny()` | 传递故意错误的数据（保留自动补全） |
| `fromExact()` | 强制完整对象（稍后可换成 fromPartial） |

| Function        | Use case                                           |
| --------------- | -------------------------------------------------- |
| `fromPartial()` | Pass partial data that still type-checks           |
| `fromAny()`     | Pass intentionally wrong data (keeps autocomplete) |
| `fromExact()`   | Force full object (swap with fromPartial later)    |

## 工作流 (Workflow)

1. **收集需求** - 询问用户：
   > **Gather requirements** - ask user:
   - 哪些测试文件有造成问题的 `as` 断言？
     > What test files have `as` assertions causing problems?
   - 是否在处理只有部分属性重要的大对象？
     > Are they dealing with large objects where only some properties matter?
   - 是否需要传递故意错误的数据用于错误测试？
     > Do they need to pass intentionally wrong data for error testing?

2. **安装并迁移**：
   > **Install and migrate**:
   - [ ] 安装：`npm i @total-typescript/shoehorn`
     > Install: `npm i @total-typescript/shoehorn`
   - [ ] 查找有 `as` 断言的测试文件：`grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"`
     > Find test files with `as` assertions: `grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"`
   - [ ] 将 `as Type` 替换为 `fromPartial()`
     > Replace `as Type` with `fromPartial()`
   - [ ] 将 `as unknown as Type` 替换为 `fromAny()`
     > Replace `as unknown as Type` with `fromAny()`
   - [ ] 从 `@total-typescript/shoehorn` 添加导入
     > Add imports from `@total-typescript/shoehorn`
   - [ ] 运行类型检查以验证
     > Run type check to verify

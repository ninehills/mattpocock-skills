---
name: tdd
description: 红-绿-重构循环的测试驱动开发。当用户想使用 TDD 构建功能或修复 bug、提到"红-绿-重构"、想要集成测试、或要求测试先行开发时使用。 (Test-driven development with red-green-refactor loop. Use when user wants to build features or fix bugs using TDD, mentions "red-green-refactor", wants integration tests, or asks for test-first development.)
---

# 测试驱动开发 (Test-Driven Development)

## 哲学 (Philosophy)

**核心原则 (Core principle)**：测试应该通过公共接口验证行为，而不是实现细节。代码可以完全改变；测试不应该。

> Tests should verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't.

**好测试 (Good tests)** 是集成风格的：它们通过公共 API 操作真正的代码路径。它们描述系统*做什么*，而不是*怎么做*。一个好测试读起来像一个规格说明——"用户可以用有效的购物车结账"告诉你确切存在什么能力。这些测试在重构中存活，因为它们不关心内部结构。

> **Good tests** are integration-style: they exercise real code paths through public APIs. They describe _what_ the system does, not _how_ it does it. A good test reads like a specification - "user can checkout with valid cart" tells you exactly what capability exists. These tests survive refactors because they don't care about internal structure.

**坏测试 (Bad tests)** 与实现耦合。它们 mock 内部协作者、测试私有方法、或通过外部方式验证（比如直接查询数据库而不是使用接口）。警告信号：你的测试在重构时失败，但行为没有改变。如果你重命名一个内部函数而测试失败，那些测试是在测试实现，而不是行为。

> **Bad tests** are coupled to implementation. They mock internal collaborators, test private methods, or verify through external means (like querying a database directly instead of using the interface). The warning sign: your test breaks when you refactor, but behavior hasn't changed. If you rename an internal function and tests fail, those tests were testing implementation, not behavior.

见 [tests.md](tests.md) 了解示例，[mocking.md](mocking.md) 了解 mock 指南。

> See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## 反模式：水平切片 (Anti-Pattern: Horizontal Slices)

**不要先写所有测试，再写所有实现。** 这是"水平切片"——将 RED 视为"写所有测试"，GREEN 视为"写所有代码"。

> **DO NOT write all tests first, then all implementation.** This is "horizontal slicing" - treating RED as "write all tests" and GREEN as "write all code."

这会产生**糟糕的测试**：

> This produces **crap tests**:

- 批量编写的测试测试的是*想象的*行为，而不是*实际的*行为
  > Tests written in bulk test _imagined_ behavior, not _actual_ behavior
- 你最终测试的是*形状*（数据结构、函数签名）而不是面向用户的行为
  > You end up testing the _shape_ of things (data structures, function signatures) rather than user-facing behavior
- 测试变得对真正的变化不敏感——行为破坏时它们通过，行为正常时它们失败
  > Tests become insensitive to real changes - they pass when behavior breaks, fail when behavior is fine
- 你超出了前灯的范围，在理解实现之前就承诺了测试结构
  > You outrun your headlights, committing to test structure before understanding the implementation

**正确方法 (Correct approach)**：通过曳光弹实现垂直切片。一个测试 → 一个实现 → 重复。每个测试都响应你从上一个周期学到的东西。因为你刚写了代码，你确切知道什么行为重要以及如何验证它。

> Vertical slices via tracer bullets. One test → one implementation → repeat. Each test responds to what you learned from the previous cycle. Because you just wrote the code, you know exactly what behavior matters and how to verify it.

```
错误 (WRONG) (水平 horizontal):
  RED:   test1, test2, test3, test4, test5
  GREEN: impl1, impl2, impl3, impl4, impl5

正确 (RIGHT) (垂直 vertical):
  RED→GREEN: test1→impl1
  RED→GREEN: test2→impl2
  RED→GREEN: test3→impl3
  ...
```

## 工作流 (Workflow)

### 1. 规划 (Planning)

探索代码库时，使用项目的领域词汇表以便测试名称和接口词汇匹配项目的语言，并尊重你正在修改区域的 ADR。

> When exploring the codebase, use the project's domain glossary so that test names and interface vocabulary match the project's language, and respect ADRs in the area you're touching.

在写任何代码之前：

> Before writing any code:

- [ ] 与用户确认需要哪些接口变更
  > Confirm with user what interface changes are needed
- [ ] 与用户确认要测试哪些行为（优先排序）
  > Confirm with user which behaviors to test (prioritize)
- [ ] 识别[深层模块](deep-modules.md)的机会（小接口，深实现）
  > Identify opportunities for [deep modules](deep-modules.md) (small interface, deep implementation)
- [ ] 为[可测试性](interface-design.md)设计接口
  > Design interfaces for [testability](interface-design.md)
- [ ] 列出要测试的行为（不是实现步骤）
  > List the behaviors to test (not implementation steps)
- [ ] 获取用户对计划的批准
  > Get user approval on the plan

问："公共接口应该是什么样子？哪些行为最重要需要测试？"

> Ask: "What should the public interface look like? Which behaviors are most important to test?"

**你不能测试一切。** 与用户确认哪些行为最重要。将测试精力集中在关键路径和复杂逻辑上，而不是每个可能的边界情况。

> **You can't test everything.** Confirm with the user exactly which behaviors matter most. Focus testing effort on critical paths and complex logic, not every possible edge case.

### 2. 曳光弹 (Tracer Bullet)

写一个测试来确认系统的一件事：

> Write ONE test that confirms ONE thing about the system:

```
RED:   为第一个行为写测试 → 测试失败
GREEN: 写最少代码使其通过 → 测试通过
RED:   Write test for first behavior → test fails
GREEN: Write minimal code to pass → test passes
```

这是你的曳光弹——证明路径端到端可行。

> This is your tracer bullet - proves the path works end-to-end.

### 3. 增量循环 (Incremental Loop)

对每个剩余行为：

> For each remaining behavior:

```
RED:   写下一个测试 → 失败
GREEN: 最少代码使其通过 → 通过
RED:   Write next test → fails
GREEN: Minimal code to pass → passes
```

规则：

> Rules:

- 一次一个测试
  > One test at a time
- 只写当前测试通过所需的最少代码
  > Only enough code to pass current test
- 不要预判未来的测试
  > Don't anticipate future tests
- 保持测试专注于可观察行为
  > Keep tests focused on observable behavior

### 4. 重构 (Refactor)

所有测试通过后，寻找[重构候选](refactoring.md)：

> After all tests pass, look for [refactor candidates](refactoring.md):

- [ ] 提取重复 (Extract duplication)
- [ ] 深化模块（将复杂性移到简单接口后面）
  > Deepen modules (move complexity behind simple interfaces)
- [ ] 在自然处应用 SOLID 原则
  > Apply SOLID principles where natural
- [ ] 考虑新代码揭示了现有代码的什么问题
  > Consider what new code reveals about existing code
- [ ] 每个重构步骤后运行测试
  > Run tests after each refactor step

**永远不要在 RED 时重构。** 先达到 GREEN。

> **Never refactor while RED.** Get to GREEN first.

## 每周期检查清单 (Checklist Per Cycle)

```
[ ] 测试描述行为，不是实现
[ ] Test describes behavior, not implementation
[ ] 测试仅使用公共接口
[ ] Test uses public interface only
[ ] 测试会在内部重构中存活
[ ] Test would survive internal refactor
[ ] 代码对此测试是最少的
[ ] Code is minimal for this test
[ ] 未添加投机性功能
[ ] No speculative features added
```

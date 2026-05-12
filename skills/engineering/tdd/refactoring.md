# 重构候选 (Refactor Candidates)

TDD 周期后，寻找：

> After TDD cycle, look for:

- **重复 (Duplication)** → 提取函数/类 (Extract function/class)
- **长方法 (Long methods)** → 拆分为私有辅助方法（保持测试在公共接口上）
  > Break into private helpers (keep tests on public interface)
- **浅层模块 (Shallow modules)** → 合并或深化
  > Combine or deepen
- **特性依恋 (Feature envy)** → 将逻辑移到数据所在处
  > Move logic to where data lives
- **基本类型偏执 (Primitive obsession)** → 引入值对象
  > Introduce value objects
- **新代码揭示的有问题的现有代码 (Existing code)** — 新代码揭示的有问题的现有代码
  > The new code reveals as problematic

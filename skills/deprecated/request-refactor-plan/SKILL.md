---
name: request-refactor-plan
description: 通过用户访谈创建包含小提交的详细重构计划，然后将其作为 GitHub issue 提交。当用户想要规划重构、创建重构 RFC 或将重构分解为安全的增量步骤时使用。 (Create a detailed refactor plan with tiny commits via user interview, then file it as a GitHub issue. Use when user wants to plan a refactor, create a refactoring RFC, or break a refactor into safe incremental steps.)
---

当用户想要创建重构请求时，将调用此技能。你应该按以下步骤进行。如果认为不必要，可以跳过某些步骤。

> This skill will be invoked when the user wants to create a refactor request. You should go through the steps below. You may skip steps if you don't consider them necessary.

1. 向用户询问他们想解决的问题的详细描述以及任何潜在的解决方案想法。

> Ask the user for a long, detailed description of the problem they want to solve and any potential ideas for solutions.

2. 探索仓库以验证他们的断言并了解代码库的当前状态。

> Explore the repo to verify their assertions and understand the current state of the codebase.

3. 询问他们是否考虑过其他选项，并向他们展示其他选项。

> Ask whether they have considered other options, and present other options to them.

4. 就实现细节访谈用户。要极其详细和彻底。

> Interview the user about the implementation. Be extremely detailed and thorough.

5. 敲定实现的确切范围。确定你计划更改和不更改的内容。

> Hammer out the exact scope of the implementation. Work out what you plan to change and what you plan not to change.

6. 在代码库中检查该区域的测试覆盖率。如果测试覆盖率不足，询问用户的测试计划。

> Look in the codebase to check for test coverage of this area of the codebase. If there is insufficient test coverage, ask the user what their plans for testing are.

7. 将实现分解为小提交计划。记住 Martin Fowler 的建议："让每个重构步骤尽可能小，这样你始终可以看到程序在工作。"

> Break the implementation into a plan of tiny commits. Remember Martin Fowler's advice to "make each refactoring step as small as possible, so that you can always see the program working."

8. 使用以下模板创建带有重构计划的 GitHub issue：

> Create a GitHub issue with the refactor plan. Use the following template for the issue description:

<refactor-plan-template>

## 问题陈述 (Problem Statement)

开发者面临的问题，从开发者的角度描述。

> The problem that the developer is facing, from the developer's perspective.

## 解决方案 (Solution)

问题的解决方案，从开发者的角度描述。

> The solution to the problem, from the developer's perspective.

## 提交计划 (Commits)

一个详细的实现计划。用通俗易懂的语言编写，将实现分解为尽可能小的提交。每个提交都应使代码库保持可工作状态。

> A LONG, detailed implementation plan. Write the plan in plain English, breaking down the implementation into the tiniest commits possible. Each commit should leave the codebase in a working state.

## 决策文档 (Decision Document)

已做出的实现决策列表。可以包括：
> A list of implementation decisions that were made. This can include:

- 将要构建/修改的模块
  > The modules that will be built/modified
- 将要修改的模块接口
  > The interfaces of those modules that will be modified
- 来自开发者的技术澄清
  > Technical clarifications from the developer
- 架构决策
  > Architectural decisions
- Schema 变更
  > Schema changes
- API 契约
  > API contracts
- 具体交互
  > Specific interactions

不要包含具体的文件路径或代码片段。它们可能很快就会过时。

> Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## 测试决策 (Testing Decisions)

已做出的测试决策列表。包括：
> A list of testing decisions that were made. Include:

- 什么是好测试的描述（只测试外部行为，不测试实现细节）
  > A description of what makes a good test (only test external behavior, not implementation details)
- 将要测试哪些模块
  > Which modules will be tested
- 测试的先例（即代码库中类似类型的测试）
  > Prior art for the tests (i.e. similar types of tests in the codebase)

## 范围之外 (Out of Scope)

不属于本次重构范围的描述。
> A description of the things that are out of scope for this refactor.

## 补充说明（可选）(Further Notes (optional))

关于重构的任何补充说明。
> Any further notes about the refactor.

</refactor-plan-template>

---
name: to-prd
description: 将当前对话上下文转化为 PRD 并发布到项目 issue 跟踪器。当用户想从当前上下文创建 PRD 时使用。 (Turn the current conversation context into a PRD and publish it to the project issue tracker. Use when user wants to create a PRD from the current context.)
---

此技能获取当前对话上下文和代码库理解并生成 PRD。不要采访用户——只需综合你已知的内容。

> This skill takes the current conversation context and codebase understanding and produces a PRD. Do NOT interview the user — just synthesize what you already know.

Issue 跟踪器和分诊标签词汇应该已经提供给你——如果没有，请运行 `/setup-matt-pocock-skills`。

> The issue tracker and triage label vocabulary should have been provided to you — run `/setup-matt-pocock-skills` if not.

## 流程 (Process)

1. 如果你尚未探索仓库，探索以了解代码库的当前状态。在整个 PRD 中使用项目的领域词汇表术语，并尊重你正在修改区域的任何 ADR。

   > Explore the repo to understand the current state of the codebase, if you haven't already. Use the project's domain glossary vocabulary throughout the PRD, and respect any ADRs in the area you're touching.

2. 概述你需要构建或修改的主要模块以完成实现。积极寻找可以提取为可独立测试的深层模块的机会。

   > Sketch out the major modules you will need to build or modify to complete the implementation. Actively look for opportunities to extract deep modules that can be tested in isolated.

   深层模块（与浅层模块相对）是将大量功能封装在一个简单、可测试且很少改变的接口中的模块。

   > A deep module (as opposed to a shallow module) is one which encapsulates a lot of functionality in a simple, testable interface which rarely changes.

   与用户确认这些模块符合他们的预期。与用户确认他们希望为哪些模块编写测试。

   > Check with the user that these modules match their expectations. Check with the user which modules they want tests written for.

3. 使用下面的模板编写 PRD，然后发布到项目 issue 跟踪器。应用 `ready-for-agent` 分诊标签——不需要额外分诊。

   > Write the PRD using the template below, then publish it to the project issue tracker. Apply the `ready-for-agent` triage label - no need for additional triage.

<prd-template>

## 问题陈述 (Problem Statement)

用户面临的问题，从用户的角度。

> The problem that the user is facing, from the user's perspective.

## 解决方案 (Solution)

问题的解决方案，从用户的角度。

> The solution to the problem, from the user's perspective.

## 用户故事 (User Stories)

一个长的、编号的用户故事列表。每个用户故事的格式为：

> A LONG, numbered list of user stories. Each user story should be in the format of:

1. 作为 <角色>，我想要 <功能>，以便 <收益>
   > As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. 作为手机银行客户，我想查看我的账户余额，以便我能更好地了解消费情况做出更明智的决策。
   > As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

这个用户故事列表应该非常详尽，覆盖功能的所有方面。

> This list of user stories should be extremely extensive and cover all aspects of the feature.

## 实现决策 (Implementation Decisions)

做出的实现决策列表。这可以包括：

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
- 特定交互
  > Specific interactions

不要包含特定文件路径或代码片段。它们可能很快就会过时。

> Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

例外：如果原型产生了一个比文字更精确编码决策的片段（状态机、reducer、schema、类型形状），在相关决策内联并简要注明它来自原型。裁剪到决策丰富的部分——不是可运行的演示，只是重要的部分。

> Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## 测试决策 (Testing Decisions)

做出的测试决策列表。包括：

> A list of testing decisions that were made. Include:

- 什么构成好测试的描述（只测试外部行为，不测试实现细节）
  > A description of what makes a good test (only test external behavior, not implementation details)
- 将要测试哪些模块
  > Which modules will be tested
- 测试的先例（即代码库中类似类型的测试）
  > Prior art for the tests (i.e. similar types of tests in the codebase)

## 范围外 (Out of Scope)

此 PRD 范围外的事物描述。

> A description of the things that are out of scope for this PRD.

## 进一步说明 (Further Notes)

关于该功能的任何进一步说明。

> Any further notes about the feature.

</prd-template>

---
name: to-issues
description: 使用曳光弹垂直切片将计划、规格或 PRD 拆分为项目 issue 跟踪器上可独立认领的 issue。当用户想将计划转换为 issue、创建实现工单、或将工作拆分为 issue 时使用。 (Break a plan, spec, or PRD into independently-grabbable issues on the project issue tracker using tracer-bullet vertical slices. Use when user wants to convert a plan into issues, create implementation tickets, or break down work into issues.)
---

# 转为 Issue (To Issues)

使用垂直切片（曳光弹）将计划拆分为可独立认领的 issue。

> Break a plan into independently-grabbable issues using vertical slices (tracer bullets).

Issue 跟踪器和分诊标签词汇应该已经提供给你——如果没有，请运行 `/setup-matt-pocock-skills`。

> The issue tracker and triage label vocabulary should have been provided to you — run `/setup-matt-pocock-skills` if not.

## 流程 (Process)

### 1. 收集上下文 (Gather context)

从对话上下文中已有的内容开始工作。如果用户传递 issue 引用（issue 编号、URL 或路径）作为参数，从 issue 跟踪器获取它并读取其完整正文和评论。

> Work from whatever is already in the conversation context. If the user passes an issue reference (issue number, URL, or path) as an argument, fetch it from the issue tracker and read its full body and comments.

### 2. 探索代码库（可选）(Explore the codebase (optional))

如果你尚未探索代码库，进行探索以了解代码的当前状态。Issue 标题和描述应使用项目的领域词汇表术语，并尊重你正在修改区域的 ADR。

> If you have not already explored the codebase, do so to understand the current state of the code. Issue titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

### 3. 起草垂直切片 (Draft vertical slices)

将计划拆分为**曳光弹** issue。每个 issue 是一个贯穿所有集成层的薄垂直切片，而不是某一层的水平切片。

> Break the plan into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

切片可以是"HITL"或"AFK"。HITL 切片需要人工交互，例如架构决策或设计审查。AFK 切片可以在无人工交互的情况下实现和合并。尽可能优先选择 AFK 而非 HITL。

> Slices may be 'HITL' or 'AFK'. HITL slices require human interaction, such as an architectural decision or a design review. AFK slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- 每个切片交付贯穿每一层（schema、API、UI、测试）的狭窄但完整的路径
  > Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- 完成的切片可以独立演示或验证
  > A completed slice is demoable or verifiable on its own
- 优先多个薄切片而非少量厚切片
  > Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. 向用户提问 (Quiz the user)

以编号列表呈现提议的拆分。对每个切片，展示：

> Present the proposed breakdown as a numbered list. For each slice, show:

- **标题 (Title)**：简短描述性名称
  > Short descriptive name
- **类型 (Type)**：HITL / AFK
- **被阻塞 (Blocked by)**：哪些其他切片（如有）必须先完成
  > Which other slices (if any) must complete first
- **覆盖的用户故事 (User stories covered)**：这解决了哪些用户故事（如果源材料有的话）
  > Which user stories this addresses (if the source material has them)

问用户：

> Ask the user:

- 粒度感觉对吗？（太粗/太细）
  > Does the granularity feel right? (too coarse / too fine)
- 依赖关系正确吗？
  > Are the dependency relationships correct?
- 任何切片应该合并或进一步拆分吗？
  > Should any slices be merged or split further?
- 正确的切片标记为 HITL 和 AFK 了吗？
  > Are the correct slices marked as HITL and AFK?

迭代直到用户批准拆分。

> Iterate until the user approves the breakdown.

### 5. 将 issue 发布到 issue 跟踪器 (Publish the issues to the issue tracker)

对每个已批准的切片，向 issue 跟踪器发布新 issue。使用下面的 issue 正文模板。这些 issue 被认为已准备好供 AFK agent 使用，因此除非另有指示，否则使用正确的分诊标签发布。

> For each approved slice, publish a new issue to the issue tracker. Use the issue body template below. These issues are considered ready for AFK agents, so publish them with the correct triage label unless instructed otherwise.

按依赖顺序发布 issue（先发布阻塞项），以便你可以在"被阻塞"字段中引用真正的 issue 标识符。

> Publish issues in dependency order (blockers first) so you can reference real issue identifiers in the "Blocked by" field.

<issue-template>
## 父级 (Parent)

对 issue 跟踪器上父级 issue 的引用（如果源是现有 issue，否则省略此部分）。

> A reference to the parent issue on the issue tracker (if the source was an existing issue, otherwise omit this section).

## 构建什么 (What to build)

此垂直切片的简明描述。描述端到端行为，而不是逐层实现。

> A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

避免特定文件路径或代码片段——它们很快就会过时。例外：如果原型产生了一个比文字更精确编码决策的片段（状态机、reducer、schema、类型形状），在此内联并简要注明它来自原型。裁剪到决策丰富的部分——不是可运行的演示，只是重要的部分。

> Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it here and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## 验收标准 (Acceptance criteria)

- [ ] 标准 1 (Criterion 1)
- [ ] 标准 2 (Criterion 2)
- [ ] 标准 3 (Criterion 3)

## 被阻塞 (Blocked by)

- 对阻塞工单的引用（如有）
  > A reference to the blocking ticket (if any)

或"无——可以立即开始"如果没有阻塞项。

> Or "None - can start immediately" if no blockers.

</issue-template>

不要关闭或修改任何父级 issue。

> Do NOT close or modify any parent issue.

---
name: qa
description: 交互式 QA 会话，用户以对话方式报告 bug 或问题，agent 提交 GitHub issue。在后台探索代码库以获取上下文和领域语言。当用户想要报告 bug、做 QA、以对话方式提交 issue 或提到"QA session"时使用。 (Interactive QA session where user reports bugs or issues conversationally, and the agent files GitHub issues. Explores the codebase in the background for context and domain language. Use when user wants to report bugs, do QA, file issues conversationally, or mentions "QA session".)
---

# QA 会话 (QA Session)

运行交互式 QA 会话。用户描述他们遇到的问题。你进行澄清，探索代码库获取上下文，并提交持久的、以用户为中心的、使用项目领域语言的 GitHub issue。

> Run an interactive QA session. The user describes problems they're encountering. You clarify, explore the codebase for context, and file GitHub issues that are durable, user-focused, and use the project's domain language.

## 对于用户提出的每个问题 (For each issue the user raises)

### 1. 听取并轻度澄清

让用户用自己的话描述问题。最多问 **2-3个简短的澄清问题**，重点关注：
> Let the user describe the problem in their own words. Ask **at most 2-3 short clarifying questions** focused on:

- 他们期望的 vs 实际发生的
  > What they expected vs what actually happened
- 复现步骤（如果不太明显的话）
  > Steps to reproduce (if not obvious)
- 是否一致出现还是间歇性的
  > Whether it's consistent or intermittent

不要过度访谈。如果描述足够清晰可以提交，就继续。
> Do NOT over-interview. If the description is clear enough to file, move on.

### 2. 在后台探索代码库

在与用户交谈的同时，启动一个 Agent（subagent_type=Explore）在后台了解相关区域。目标不是找到修复方案 —— 而是：
> While talking to the user, kick off an Agent (subagent_type=Explore) in the background to understand the relevant area. The goal is NOT to find a fix — it's to:

- 了解该区域使用的领域语言（查看 UBIQUITOUS_LANGUAGE.md）
  > Learn the domain language used in that area (check UBIQUITOUS_LANGUAGE.md)
- 理解该功能应该做什么
  > Understand what the feature is supposed to do
- 确定面向用户的行为边界
  > Identify the user-facing behavior boundary

这些上下文帮助你写出更好的 issue —— 但 issue 本身不应引用具体的文件、行号或内部实现细节。

> This context helps you write a better issue — but the issue itself should NOT reference specific files, line numbers, or internal implementation details.

### 3. 评估范围：单个问题还是拆分？

在提交之前，确定这是一个**单个问题**还是需要**拆分**为多个问题。

> Before filing, decide whether this is a **single issue** or needs to be **broken down** into multiple issues.

拆分的情况：
> Break down when:

- 修复涉及多个独立区域（例如"表单验证有误且成功消息缺失且重定向有问题"）
  > The fix spans multiple independent areas (e.g. "the form validation is wrong AND the success message is missing AND the redirect is broken")
- 有明显可分离的关注点，不同的人可以并行处理
  > There are clearly separable concerns that different people could work on in parallel
- 用户描述了具有多种不同失败模式或症状的问题
  > The user describes something that has multiple distinct failure modes or symptoms

保持为单个问题的情况：
> Keep as a single issue when:

- 是一个地方的一个行为有问题
  > It's one behavior that's wrong in one place
- 所有症状都由同一个根本行为引起
  > The symptoms are all caused by the same root behavior

### 4. 提交 GitHub issue

使用 `gh issue create` 创建 issue。不要请求用户先审查 —— 直接提交并分享 URL。

> Create issues with `gh issue create`. Do NOT ask the user to review first — just file and share URLs.

Issue 必须是**持久的** —— 即使在大规模重构后也应该有意义。从用户的角度编写。

> Issues must be **durable** — they should still make sense after major refactors. Write from the user's perspective.

#### 单个问题模板

使用此模板：
> Use this template:

```
## What happened

[Describe the actual behavior the user experienced, in plain language]

## What I expected

[Describe the expected behavior]

## Steps to reproduce

1. [Concrete, numbered steps a developer can follow]
2. [Use domain terms from the codebase, not internal module names]
3. [Include relevant inputs, flags, or configuration]

## Additional context

[Any extra observations from the user or from codebase exploration that help frame the issue — e.g. "this only happens when using the Docker layer, not the filesystem layer" — use domain language but don't cite files]
```

#### 拆分情况（多个问题）

按依赖顺序创建 issue（先创建阻塞项），这样你就可以引用真实的 issue 编号。

> Create issues in dependency order (blockers first) so you can reference real issue numbers.

对每个子问题使用此模板：
> Use this template for each sub-issue:

```
## Parent issue

#<parent-issue-number> (if you created a tracking issue) or "Reported during QA session"

## What's wrong

[Describe this specific behavior problem — just this slice, not the whole report]

## What I expected

[Expected behavior for this specific slice]

## Steps to reproduce

1. [Steps specific to THIS issue]

## Blocked by

- #<issue-number> (if this issue can't be fixed until another is resolved)

Or "None — can start immediately" if no blockers.

## Additional context

[Any extra observations relevant to this slice]
```

创建拆分时：
> When creating a breakdown:

- **优先多个薄 issue 而非少数厚 issue** —— 每个都应可独立修复和验证
  > **Prefer many thin issues over few thick ones** — each should be independently fixable and verifiable
- **诚实地标记阻塞关系** —— 如果 issue B 在 issue A 修复之前确实无法测试，就说明。如果它们是独立的，都标记为"None — can start immediately"
  > **Mark blocking relationships honestly** — if issue B genuinely can't be tested until issue A is fixed, say so. If they're independent, mark both as "None — can start immediately"
- **按依赖顺序创建 issue**，这样你可以在"Blocked by"中引用真实的 issue 编号
  > **Create issues in dependency order** so you can reference real issue numbers in "Blocked by"
- **最大化并行性** —— 目标是多人（或 agent）可以同时处理不同的 issue
  > **Maximize parallelism** — the goal is that multiple people (or agents) can grab different issues simultaneously

#### 所有 issue 正文的规则 (Rules for all issue bodies)

- **不要包含文件路径或行号** —— 这些会过时
  > **No file paths or line numbers** — these go stale
- **使用项目的领域语言**（如果存在 UBIQUITOUS_LANGUAGE.md 则查看）
  > **Use the project's domain language** (check UBIQUITOUS_LANGUAGE.md if it exists)
- **描述行为，不描述代码** —— "sync service 无法应用补丁"而非"applyPatch() 在第42行抛出异常"
  > **Describe behaviors, not code** — "the sync service fails to apply the patch" not "applyPatch() throws on line 42"
- **复现步骤是必须的** —— 如果无法确定，询问用户
  > **Reproduction steps are mandatory** — if you can't determine them, ask the user
- **保持简洁** —— 开发者应该能在30秒内读完 issue
  > **Keep it concise** — a developer should be able to read the issue in 30 seconds

提交后，打印所有 issue URL（附带阻塞关系摘要）并询问："下一个问题，还是结束了？"

> After filing, print all issue URLs (with blocking relationships summarized) and ask: "Next issue, or are we done?"

### 5. 继续会话

持续进行直到用户说结束。每个 issue 是独立的 —— 不要批量处理。

> Keep going until the user says they're done. Each issue is independent — don't batch them.

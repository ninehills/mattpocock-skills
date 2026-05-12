# 编写 Agent Brief (Writing Agent Briefs)

Agent brief 是当 issue 移到 `ready-for-agent` 时发布在 GitHub issue 上的结构化评论。它是 AFK agent 将依据工作的权威规范。原始 issue 正文和讨论是上下文——agent brief 是契约。

> An agent brief is a structured comment posted on a GitHub issue when it moves to `ready-for-agent`. It is the authoritative specification that an AFK agent will work from. The original issue body and discussion are context — the agent brief is the contract.

## 原则 (Principles)

### 持久性优先于精确性 (Durability over precision)

issue 可能在 `ready-for-agent` 状态停留数天或数周。代码库在此期间会发生变化。编写 brief 时应使其即使在文件被重命名、移动或重构时也能保持有用。

> The issue may sit in `ready-for-agent` for days or weeks. The codebase will change in the meantime. Write the brief so it stays useful even as files are renamed, moved, or refactored.

- **应该**描述接口、类型和行为契约
  > **Do** describe interfaces, types, and behavioral contracts
- **应该**指定 agent 应查找或修改的具体类型、函数签名或配置形状
  > **Do** name specific types, function signatures, or config shapes that the agent should look for or modify
- **不应该**引用文件路径——它们会过时
  > **Don't** reference file paths — they go stale
- **不应该**引用行号
  > **Don't** reference line numbers
- **不应该**假设当前实现结构将保持不变
  > **Don't** assume the current implementation structure will remain the same

### 行为性，而非过程性 (Behavioral, not procedural)

描述系统**应该做什么**，而不是**如何**实现它。agent 将从头探索代码库并做出自己的实现决策。

> Describe **what** the system should do, not **how** to implement it. The agent will explore the codebase fresh and make its own implementation decisions.

- **好的：** "`SkillConfig` 类型应接受一个可选的 `schedule` 字段，类型为 `CronExpression`"
  > **Good:** "The `SkillConfig` type should accept an optional `schedule` field of type `CronExpression`"
- **坏的：** "打开 src/types/skill.ts 并在第 42 行添加 schedule 字段"
  > **Bad:** "Open src/types/skill.ts and add a schedule field on line 42"
- **好的：** "当用户运行 `/triage` 且无参数时，应看到需要关注的 issue 摘要"
  > **Good:** "When a user runs `/triage` with no arguments, they should see a summary of issues needing attention"
- **坏的：** "在主处理函数中添加 switch 语句"
  > **Bad:** "Add a switch statement in the main handler function"

### 完整的验收标准 (Complete acceptance criteria)

Agent 需要知道何时完成。每个 agent brief 必须有具体的、可测试的验收标准。每个标准应可独立验证。

> The agent needs to know when it's done. Every agent brief must have concrete, testable acceptance criteria. Each criterion should be independently verifiable.

- **好的：** "运行 `gh issue list --label needs-triage` 返回已通过初始分类的 issue"
  > **Good:** "Running `gh issue list --label needs-triage` returns issues that have been through initial classification"
- **坏的：** "分诊应该正常工作"
  > **Bad:** "Triage should work correctly"

### 明确的范围边界 (Explicit scope boundaries)

说明什么是超出范围的。这可以防止 agent 过度修饰或对相邻功能做出假设。

> State what is out of scope. This prevents the agent from gold-plating or making assumptions about adjacent features.

## 模板 (Template)

```markdown
## Agent Brief

**Category:** bug / enhancement
**Summary:** one-line description of what needs to happen

**Current behavior:**
Describe what happens now. For bugs, this is the broken behavior.
For enhancements, this is the status quo the feature builds on.

**Desired behavior:**
Describe what should happen after the agent's work is complete.
Be specific about edge cases and error conditions.

**Key interfaces:**
- `TypeName` — what needs to change and why
- `functionName()` return type — what it currently returns vs what it should return
- Config shape — any new configuration options needed

**Acceptance criteria:**
- [ ] Specific, testable criterion 1
- [ ] Specific, testable criterion 2
- [ ] Specific, testable criterion 3

**Out of scope:**
- Thing that should NOT be changed or addressed in this issue
- Adjacent feature that might seem related but is separate
```

## 示例 (Examples)

### 好的 agent brief（bug）(Good agent brief (bug))

```markdown
## Agent Brief

**Category:** bug
**Summary:** Skill description truncation drops mid-word, producing broken output

**Current behavior:**
When a skill description exceeds 1024 characters, it is truncated at exactly
1024 characters regardless of word boundaries. This produces descriptions
that end mid-word (e.g. "Use when the user wants to confi").

**Desired behavior:**
Truncation should break at the last word boundary before 1024 characters
and append "..." to indicate truncation.

**Key interfaces:**
- The `SkillMetadata` type's `description` field — no type change needed,
  but the validation/processing logic that populates it needs to respect
  word boundaries
- Any function that reads SKILL.md frontmatter and extracts the description

**Acceptance criteria:**
- [ ] Descriptions under 1024 chars are unchanged
- [ ] Descriptions over 1024 chars are truncated at the last word boundary
      before 1024 chars
- [ ] Truncated descriptions end with "..."
- [ ] The total length including "..." does not exceed 1024 chars

**Out of scope:**
- Changing the 1024 char limit itself
- Multi-line description support
```

### 好的 agent brief（enhancement）(Good agent brief (enhancement))

```markdown
## Agent Brief

**Category:** enhancement
**Summary:** Add `.out-of-scope/` directory support for tracking rejected feature requests

**Current behavior:**
When a feature request is rejected, the issue is closed with a `wontfix` label
and a comment. There is no persistent record of the decision or reasoning.
Future similar requests require the maintainer to recall or search for the
prior discussion.

**Desired behavior:**
Rejected feature requests should be documented in `.out-of-scope/<concept>.md`
files that capture the decision, reasoning, and links to all issues that
requested the feature. When triaging new issues, these files should be
checked for matches.

**Key interfaces:**
- Markdown file format in `.out-of-scope/` — each file should have a
  `# Concept Name` heading, a `**Decision:**` line, a `**Reason:**` line,
  and a `**Prior requests:**` list with issue links
- The triage workflow should read all `.out-of-scope/*.md` files early
  and match incoming issues against them by concept similarity

**Acceptance criteria:**
- [ ] Closing a feature as wontfix creates/updates a file in `.out-of-scope/`
- [ ] The file includes the decision, reasoning, and link to the closed issue
- [ ] If a matching `.out-of-scope/` file already exists, the new issue is
      appended to its "Prior requests" list rather than creating a duplicate
- [ ] During triage, existing `.out-of-scope/` files are checked and surfaced
      when a new issue matches a prior rejection

**Out of scope:**
- Automated matching (human confirms the match)
- Reopening previously rejected features
- Bug reports (only enhancement rejections go to `.out-of-scope/`)
```

### 坏的 agent brief (Bad agent brief)

```markdown
## Agent Brief

**Summary:** Fix the triage bug

**What to do:**
The triage thing is broken. Look at the main file and fix it.
The function around line 150 has the issue.

**Files to change:**
- src/triage/handler.ts (line 150)
- src/types.ts (line 42)
```

这是坏的因为：

> This is bad because:

- 没有类别
  > No category
- 模糊的描述（"分诊那东西坏了"）
  > Vague description ("the triage thing is broken")
- 引用会过时的文件路径和行号
  > References file paths and line numbers that will go stale
- 没有验收标准
  > No acceptance criteria
- 没有范围边界
  > No scope boundaries
- 没有当前行为与期望行为的描述
  > No description of current vs desired behavior

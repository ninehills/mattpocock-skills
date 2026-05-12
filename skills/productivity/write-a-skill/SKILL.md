---
name: write-a-skill
description: 创建具有正确结构、渐进式披露和捆绑资源的新 agent 技能。当用户想要创建、编写或构建新技能时使用。 (Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.)
---

# 编写技能 (Writing Skills)

## 流程 (Process)

1. **收集需求** - 向用户询问：
   > **Gather requirements** - ask user about:
   - 该技能涵盖什么任务/领域？
     > What task/domain does the skill cover?
   - 它应该处理哪些具体用例？
     > What specific use cases should it handle?
   - 它需要可执行脚本还是只需要说明？
     > Does it need executable scripts or just instructions?
   - 有要包含的参考材料吗？
     > Any reference materials to include?

2. **起草技能** - 创建：
   > **Draft the skill** - create:
   - 包含简洁说明的 SKILL.md
     > SKILL.md with concise instructions
   - 如果内容超过500行，添加额外的参考文件
     > Additional reference files if content exceeds 500 lines
   - 如果需要确定性操作，添加实用脚本
     > Utility scripts if deterministic operations needed

3. **与用户一起审查** - 展示草稿并询问：
   > **Review with user** - present draft and ask:
   - 这涵盖了你的用例吗？
     > Does this cover your use cases?
   - 有什么遗漏或不清楚的吗？
     > Anything missing or unclear?
   - 某个部分应该更详细/更简洁吗？
     > Should any section be more/less detailed?

## 技能结构 (Skill Structure)

```
skill-name/
├── SKILL.md           # 主要说明（必需）
├── REFERENCE.md       # 详细文档（如果需要）
├── EXAMPLES.md        # 使用示例（如果需要）
└── scripts/           # 实用脚本（如果需要）
    └── helper.js
```

## SKILL.md 模板 (Template)

```md
---
name: skill-name
description: Brief description of capability. Use when [specific triggers].
---

# Skill Name

## Quick start

[Minimal working example]

## Workflows

[Step-by-step processes with checklists for complex tasks]

## Advanced features

[Link to separate files: See [REFERENCE.md](REFERENCE.md)]
```

## 描述要求 (Description Requirements)

描述是**你的 agent 在决定加载哪个技能时看到的唯一内容**。它在系统提示中与所有其他已安装的技能一起显示。你的 agent 读取这些描述并根据用户的请求选择相关技能。

> The description is **the only thing your agent sees** when deciding which skill to load. It's surfaced in the system prompt alongside all other installed skills. Your agent reads these descriptions and picks the relevant skill based on the user's request.

**目标**：给你的 agent 足够的信息来知道：
> **Goal**: Give your agent just enough info to know:

1. 该技能提供什么能力
   > What capability this skill provides
2. 何时/为什么触发它（特定关键词、上下文、文件类型）
   > When/why to trigger it (specific keywords, contexts, file types)

**格式**：
> **Format**:

- 最多1024个字符
  > Max 1024 chars
- 用第三人称书写
  > Write in third person
- 第一句话：它做什么
  > First sentence: what it does
- 第二句话："Use when [specific triggers]"
  > Second sentence: "Use when [specific triggers]"

**好的示例**：
> **Good example**:

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.
```

**不好的示例**：
> **Bad example**:

```
Helps with documents.
```

不好的示例让你的 agent 无法将其与其他文档技能区分开来。
> The bad example gives your agent no way to distinguish this from other document skills.

## 何时添加脚本 (When to Add Scripts)

在以下情况下添加实用脚本：
> Add utility scripts when:

- 操作是确定性的（验证、格式化）
  > Operation is deterministic (validation, formatting)
- 同样的代码会被重复生成
  > Same code would be generated repeatedly
- 错误需要显式处理
  > Errors need explicit handling

脚本比生成的代码更节省 token 并提高可靠性。
> Scripts save tokens and improve reliability vs generated code.

## 何时拆分文件 (When to Split Files)

在以下情况下拆分为单独的文件：
> Split into separate files when:

- SKILL.md 超过100行
  > SKILL.md exceeds 100 lines
- 内容有明显的领域划分（如财务 vs 销售模式）
  > Content has distinct domains (finance vs sales schemas)
- 高级功能很少被需要
  > Advanced features are rarely needed

## 审查清单 (Review Checklist)

起草后，请验证：
> After drafting, verify:

- [ ] 描述包含触发条件（"Use when..."）
  > Description includes triggers ("Use when...")
- [ ] SKILL.md 保持在100行以内
  > SKILL.md under 100 lines
- [ ] 没有时效性信息
  > No time-sensitive info
- [ ] 术语一致
  > Consistent terminology
- [ ] 包含具体示例
  > Concrete examples included
- [ ] 引用深度为一级
  > References one level deep

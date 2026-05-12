---
name: ubiquitous-language
description: 从当前对话中提取 DDD 风格的统一语言词汇表，标记歧义并提出规范术语。保存到 UBIQUITOUS_LANGUAGE.md。当用户想要定义领域术语、构建词汇表、强化术语、创建统一语言，或提到"domain model"或"DDD"时使用。 (Extract a DDD-style ubiquitous language glossary from the current conversation, flagging ambiguities and proposing canonical terms. Saves to UBIQUITOUS_LANGUAGE.md. Use when user wants to define domain terms, build a glossary, harden terminology, create a ubiquitous language, or mentions "domain model" or "DDD".)
disable-model-invocation: true
---

# 统一语言 (Ubiquitous Language)

从当前对话中提取和形式化领域术语，形成一致的词汇表，保存到本地文件。

> Extract and formalize domain terminology from the current conversation into a consistent glossary, saved to a local file.

## 流程 (Process)

1. **扫描对话**中的领域相关名词、动词和概念
   > **Scan the conversation** for domain-relevant nouns, verbs, and concepts
2. **识别问题**：
   > **Identify problems**:
   - 同一个词用于不同概念（歧义）
     > Same word used for different concepts (ambiguity)
   - 不同的词用于同一概念（同义词）
     > Different words used for the same concept (synonyms)
   - 模糊或超载的术语
     > Vague or overloaded terms
3. **提出规范词汇表**，包含有主见的术语选择
   > **Propose a canonical glossary** with opinionated term choices
4. **写入 `UBIQUITOUS_LANGUAGE.md`**，使用以下格式
   > **Write to `UBIQUITOUS_LANGUAGE.md`** in the working directory using the format below
5. **在对话中输出摘要**
   > **Output a summary** inline in the conversation

## 输出格式 (Output Format)

写入一个包含以下结构的 `UBIQUITOUS_LANGUAGE.md` 文件：
> Write a `UBIQUITOUS_LANGUAGE.md` file with this structure:

```md
# Ubiquitous Language

## Order lifecycle

| Term        | Definition                                              | Aliases to avoid      |
| ----------- | ------------------------------------------------------- | --------------------- |
| **Order**   | A customer's request to purchase one or more items      | Purchase, transaction |
| **Invoice** | A request for payment sent to a customer after delivery | Bill, payment request |

## People

| Term         | Definition                                  | Aliases to avoid       |
| ------------ | ------------------------------------------- | ---------------------- |
| **Customer** | A person or organization that places orders | Client, buyer, account |
| **User**     | An authentication identity in the system    | Login, account         |

## Relationships

- An **Invoice** belongs to exactly one **Customer**
- An **Order** produces one or more **Invoices**

## Example dialogue

> **Dev:** "When a **Customer** places an **Order**, do we create the **Invoice** immediately?"
> **Domain expert:** "No — an **Invoice** is only generated once a **Fulfillment** is confirmed. A single **Order** can produce multiple **Invoices** if items ship in separate **Shipments**."
> **Dev:** "So if a **Shipment** is cancelled before dispatch, no **Invoice** exists for it?"
> **Domain expert:** "Exactly. The **Invoice** lifecycle is tied to the **Fulfillment**, not the **Order**."

## Flagged ambiguities

- "account" was used to mean both **Customer** and **User** — these are distinct concepts: a **Customer** places orders, while a **User** is an authentication identity that may or may not represent a **Customer**.
```

## 规则 (Rules)

- **要有主见。** 当同一概念有多个词时，选择最好的一个，将其他列为要避免的别名。
  > **Be opinionated.** When multiple words exist for the same concept, pick the best one and list the others as aliases to avoid.
- **显式标记冲突。** 如果某个术语在对话中有歧义，在"Flagged ambiguities"部分指出并给出明确建议。
  > **Flag conflicts explicitly.** If a term is used ambiguously in the conversation, call it out in the "Flagged ambiguities" section with a clear recommendation.
- **仅包含领域专家相关的术语。** 跳过模块或类的名称，除非它们在领域语言中有意义。
  > **Only include terms relevant for domain experts.** Skip the names of modules or classes unless they have meaning in the domain language.
- **定义要简洁。** 最多一句话。定义它是什么，而不是它做什么。
  > **Keep definitions tight.** One sentence max. Define what it IS, not what it does.
- **展示关系。** 使用粗体术语名称，在明显的地方表达基数。
  > **Show relationships.** Use bold term names and express cardinality where obvious.
- **仅包含领域术语。** 跳过通用编程概念（数组、函数、端点），除非它们有领域特定含义。
  > **Only include domain terms.** Skip generic programming concepts (array, function, endpoint) unless they have domain-specific meaning.
- **将术语分组到多个表中**，当自然聚类出现时（例如按子域、生命周期或参与者）。每个分组有自己的标题和表格。如果所有术语属于一个单一内聚领域，一个表即可 —— 不要强行分组。
  > **Group terms into multiple tables** when natural clusters emerge (e.g. by subdomain, lifecycle, or actor). Each group gets its own heading and table. If all terms belong to a single cohesive domain, one table is fine — don't force groupings.
- **写一段示例对话。** 开发者和领域专家之间的简短对话（3-5轮），展示术语如何自然交互。对话应澄清相关概念之间的边界，并展示术语的精确使用。
  > **Write an example dialogue.** A short conversation (3-5 exchanges) between a dev and a domain expert that demonstrates how the terms interact naturally. The dialogue should clarify boundaries between related concepts and show terms being used precisely.

<example>

## 示例对话 (Example dialogue)

> **Dev:** "How do I test the **sync service** without Docker?"

> **Domain expert:** "Provide the **filesystem layer** instead of the **Docker layer**. It implements the same **Sandbox service** interface but uses a local directory as the **sandbox**."

> **Dev:** "So **sync-in** still creates a **bundle** and unpacks it?"

> **Domain expert:** "Exactly. The **sync service** doesn't know which layer it's talking to. It calls `exec` and `copyIn` — the **filesystem layer** just runs those as local shell commands."

</example>

## 重新运行 (Re-running)

在同一对话中再次调用时：
> When invoked again in the same conversation:

1. 读取现有的 `UBIQUITOUS_LANGUAGE.md`
   > Read the existing `UBIQUITOUS_LANGUAGE.md`
2. 纳入后续讨论中的任何新术语
   > Incorporate any new terms from subsequent discussion
3. 如果理解有变化，更新定义
   > Update definitions if understanding has evolved
4. 重新标记任何新的歧义
   > Re-flag any new ambiguities
5. 重写示例对话以纳入新术语
   > Rewrite the example dialogue to incorporate new terms

---
name: writing-shape
description: 拿到一份原始素材的 markdown 文件，通过对话会话将其塑造成文章 —— 起草候选开头、逐段增长文章、在每一步讨论格式（列表、表格、标注、引用）选择。当用户有一堆笔记、碎片或粗稿，想要帮助将其变成可发布的内容时使用。 (Take a markdown file of raw material and shape it into an article through a conversational session — drafting candidate openings, growing the piece paragraph by paragraph, arguing about format (lists, tables, callouts, quotes) at each step. Use when the user has a pile of notes, fragments, or a rough draft and wants help turning it into something publishable.)
---

<what-to-do>

用户已传递（或将传递）一份原始素材的 markdown 文件。将其视为输入堆 —— 从整洁的碎片列表到无结构的散文墙到转录稿。格式无关紧要。在做任何其他事情之前，先从头到尾读一遍。

> The user has passed (or will pass) a markdown file of raw material. Treat it as the input pile — anything from a tidy list of fragments to a wall of unstructured prose to a transcript. The format does not matter. Read it end-to-end before doing anything else.

然后运行一次塑形会话，生成一个单独的文章文档。不要编辑原始素材文件 —— 对本技能来说它是只读的。

> Then run a shaping session that produces a separate article document. Do not edit the raw material file — it is read-only to this skill.

如果用户没有说文章保存在哪里，询问一次并记住路径。用户会在会话期间编辑文章文件；每次写入前都重新读取，以保留他们的编辑。

> If the user did not say where to save the article, ask once and remember the path. The user will be editing the article file during the session; always re-read it before writing so their edits are preserved.

</what-to-do>

<supporting-info>

## 循环 (The loop)

1. **读取素材堆。** 完整读取输入文件。形成对其中内容的感知。
   > **Read the pile.** Read the input file in full. Form a sense of what's in it.
2. **起草 2-3 个候选开头。** 每个开头应暗示文章不同的论点或角度。展示所有选项。强迫用户选择或组合一个混合版本。选定的开头决定了文章其余部分必须做什么。
   > **Draft 2–3 candidate openings.** Each opening should imply a different thesis or angle for the article. Show all of them. Force the user to pick or compose a hybrid. The chosen opening defines what the rest of the article must do.
3. **逐段增长。** 开头确定后，问"给定这个开头，读者接下来需要听到什么？"从素材堆中拉取材料来回答。讨论下一个节拍是段落、列表、表格、标注、引用还是代码块。每个格式选择都应该是刻意的且可辩护的。
   > **Grow paragraph by paragraph.** After the opening lands, ask "given this opening, what does the reader need to hear next?" Pull material from the pile to answer. Argue about whether the next beat is a paragraph, a list, a table, a callout, a quote, a code block. Each format choice should be deliberate and defensible.
4. **边写边追加到文章文件。** 不要批量写入。每个达成一致的段落或块立即写入，这样用户可以看到文章逐渐成型。
   > **Append to the article file as you go.** Don't batch. Write each agreed paragraph or block immediately so the user can see the article taking shape.
5. **循环第3步直到文章完成。** 由用户决定何时完成。
   > **Loop step 3 until the article is done.** The user decides when it's done.

## 对话感觉 (Conversational feel)

这是一个反转的矿掘会话。在构思阶段，问题是"你实际注意到了什么？"这里是"这篇文章实际在论证什么，读者需要以什么顺序听到它？"要反驳。不要让弱过渡滑过去。如果一个段落没有赢得它的位置，就删掉。

> This is a grilling session inverted. In ideation, the question was "what are you actually noticing?" Here it's "what is this article actually arguing, and in what order does the reader need to hear it?" Push back. Refuse to let weak transitions slide. If a paragraph doesn't earn its place, cut it.

持续使用的具体动作：
> Specific moves to keep using:

- "这个段落为读者做了什么上一个段落没做的事？"
  > "What does this paragraph do for the reader that the previous one didn't?"
- "如果我删掉这个，什么会断？"
  > "If I cut this, what breaks?"
- "这是散文，还是应该做成列表？为什么是散文？"
  > "Is this prose, or should it be a list? Why prose?"
- "这个句子在做两份工作 —— 拆分它或选一个。"
  > "This sentence is doing two jobs — split it or pick one."
- "开头承诺了 X。我们已经偏离到 Y。要么重新串联，要么更改开头。"
  > "The opening promised X. We've drifted to Y. Either re-thread it or change the opening."

## 从素材堆中拉取 (Pulling from the pile)

将原始素材视为采石场，而非脚本。拉取一个碎片，重新加工以适应周围的段落，然后放置。一个碎片可能被拆分到多个段落、与另一个合并，或被意译。素材堆的工作是被开采；文章的工作是读起来像一个声音。

> Treat the raw material as a quarry, not a script. Pull a fragment, rework it to fit the surrounding paragraph, and place it. A fragment may be split across multiple paragraphs, merged with another, or paraphrased. The pile's job is to be mined; the article's job is to read as one voice.

如果素材堆缺少文章需要的东西，明确指出缺口："这里需要一个示例但素材堆里没有 —— 现在给我一个，否则我们删掉这个部分。"

> If the pile lacks something the article needs, name the gap explicitly: "We need an example here and the pile doesn't have one — give me one now or we cut this section."

## 要实际进行的格式讨论 (Format arguments to actually have)

选择如何呈现一个节拍时，与用户大声权衡这些利弊，而非暗自决定：
> When choosing how to render a beat, weigh these tradeoffs out loud with the user, not silently:

- **散文 vs. 列表。** 散文承载论证；列表承载平行项目。如果项目不是真正平行的，散文更好。如果是，列表更快扫读。
  > **Prose vs. list.** Prose carries argument; lists carry parallel items. If items aren't truly parallel, prose is better. If they are, a list is faster to scan.
- **行内 vs. 标注。** 提示、警告和旁注放在标注中（`> [!TIP]`、`> [!NOTE]`）—— 但只有当它们真的会偏离主论证时才这样做。否则保持行内。
  > **Inline vs. callout.** Tips, warnings, and asides go in callouts (`> [!TIP]`, `> [!NOTE]`) — but only if they'd genuinely derail the main argument inline. Otherwise leave them inline.
- **表格 vs. 重复结构。** 如果相同的形状以相同的字段重复 3+ 次，用表格。否则用带粗体引导词的散文。
  > **Table vs. repeated structure.** If the same shape repeats 3+ times with the same fields, a table. Otherwise prose with bold leads.
- **引用 vs. 转述。** 当原始措辞本身就是重点时引用。当只有想法重要时转述。
  > **Quote vs. paraphrase.** Quote when the original wording is the point. Paraphrase when only the idea matters.
- **代码块 vs. 行内代码。** 多行、可运行或说明性的 → 块。单个标识符 → 行内。
  > **Code block vs. inline code.** Multi-line, runnable, or illustrative → block. Single token or identifier → inline.

## 写作节奏 (Writing rhythm)

每个块达成一致后追加到文章文件。每次写入前从磁盘重新读取文件 —— 用户可能在轮次之间编辑过。永远不要盲目覆盖。如果用户想重写某个段落，就地编辑该段落；其余部分不动。

> Append to the article file as each block is agreed. Re-read the file from disk before every write — the user may have edited between turns. Never overwrite blindly. If the user wants a paragraph rewritten, edit that specific paragraph in place; leave the rest alone.

## 范围之外 (Out of scope)

- 挖掘素材堆中没有的新碎片（素材堆是输入 —— 如果不完整，指出缺口并让用户补充或删掉该部分）。
  > Mining for new fragments that aren't in the pile (the pile is the input — if it's incomplete, name the gap and either get the user to fill it or cut the section).
- 编辑原始素材文件。
  > Editing the raw material file.
- 发布、为特定平台格式化，或添加用户未要求的 frontmatter。
  > Publishing, formatting for a specific platform, or adding frontmatter the user didn't ask for.

</supporting-info>

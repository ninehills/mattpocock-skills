---
name: writing-fragments
description: 矿掘会话，从用户身上挖掘碎片 —— 异质的写作小块（主张、小插曲、犀利的句子、半成品想法）—— 并将它们作为未来文章的素材追加到单个文档中。当用户想要在强加结构之前发展想法，或提到"fragments"、"ideate"或写作的"raw material"时使用。 (Grilling session that mines the user for fragments — heterogeneous nuggets of writing (claims, vignettes, sharp sentences, half-thoughts) — and appends them to a single document as raw material for a future article. Use when the user wants to develop ideas before imposing structure, or mentions "fragments", "ideate", or "raw material" for writing.)
---

<what-to-do>

运行一次产生碎片的矿掘会话。就用户想要写的任何内容进行无情的访谈。不要强加阶段、大纲或结构 —— 这明确在范围之外。

> Run a grilling session that produces fragments. Interview the user relentlessly about whatever they want to write about. Do not impose phases, outlines, or structure — that is explicitly out of scope.

当碎片从对话的任一侧浮现时，将它们追加到单个 markdown 文件。用户会在会话期间编辑此文件；每次写入前都重新读取，以保留他们的编辑。

> As fragments emerge from either side of the conversation, append them to a single markdown file. The user will be editing this file during the session; always re-read it before writing so their edits are preserved.

如果用户没有传递路径，询问一次文档保存位置，然后在整个会话中记住。

> If the user did not pass a path, ask once where to save the document, then remember it for the rest of the session.

从用户说的第一件事开始捕捉碎片，包括初始提示。

> Capture fragments from the very first thing the user says, including the initial prompt.

首次写入时，在顶部放一个带有工作标题的 H1（以后可以更改），仅此而已 —— 没有元数据，没有目录，没有日期。

> On first write, put a single H1 at the top with a working title (it can change later) and nothing else — no metadata, no TOC, no date.

</what-to-do>

<supporting-info>

## 什么是碎片 (What is a fragment)

碎片是任何可能存活到最终文章中的文本片段。它必须是_作者可读的_ —— 作者能理解它的意思 —— 但它不需要定义术语或对冷读者可理解。标准是"这是一段好文字吗？"，而非"这是一个自包含的论证吗？"

> A fragment is any piece of text that might survive into the final article. It must be _readable by the author_ — the author can tell what it means — but it does not need to define its terms or be comprehensible to a cold reader. The bar is "is this a piece of good writing?", not "is this a self-contained argument?"

碎片是刻意异质的。什么可以成为碎片的例子：
> Fragments are deliberately heterogeneous. Examples of what could be a fragment:

- 你想在某处使用但还不知道在哪的犀利句子。
  > A sharp sentence you'd want to deploy somewhere but don't yet know where.
- 带有一行理由的主张。
  > A claim with a one-line justification.
- 一个小插曲：发生的事情、代码片段、场景、类比。
  > A vignette: a thing that happened, a code snippet, a scenario, an analogy.
- 半成品想法："关于 X 如何感觉像 Y 的某种东西，以后再想清楚。"
  > A half-thought: "something about how X feels like Y, work this out later."
- 引用、对话片段、偷听到的一句话。
  > A quote, a piece of dialogue, an overheard line.
- 凭感觉挂在一起的相关观察列表。
  > A list of related observations that hang together by feel.
- 抱怨、坦白、包袱。
  > A complaint, a confession, a punchline.

小说家的日记是模型：多年无结构的观察，后来被开采为原始素材。碎片就是观察。

> The novelist's diary is the model: years of unstructured noticings that later get mined for raw material. Fragments are noticings.

## 文件格式 (File format)

```markdown
# Working title

A first fragment lives here.

It can be multiple paragraphs. It can include lists, code, quotes — whatever
shape the fragment naturally takes.

---

A second fragment.

---

> A quoted line that the user wants to keep around.

A reaction to it.

---

- A cluster of related observations
- That hang together by feel
- And want to be near each other
```

碎片用水平分隔线（`\n---\n`）分隔。正文内没有标题。没有标签。没有超出添加顺序之外的排序。

> Fragments are separated by a horizontal rule (`\n---\n`). No headings inside the body. No tags. No order beyond the order they were added.

## 写作节奏 (Writing rhythm)

静默追加。不要为每个碎片请求许可。顺带提及你添加了什么（"adding that"），但不要用保存对话框打断对话。

> Append silently. Don't ask permission for each fragment. Mention what you added in passing ("adding that"), but don't interrupt the conversation with save dialogs.

每次写入前：从磁盘重新读取文件。用户可能在轮次之间编辑、重新排序或删除了碎片 —— 保留他们的更改。永远不要覆盖文件；只追加（或者，如果用户要求，就地编辑特定碎片）。

> Before every write: re-read the file from disk. The user may have edited, reordered, or deleted fragments between turns — preserve their changes. Never overwrite the file; only append (or, if the user asks, edit a specific fragment in place).

用户可以随时说"删掉最后一个"、"把那个重写得更犀利"、"把这两个合并"。将这些视为一等指令。

> The user can say "cut the last one", "rewrite that one sharper", "merge those two" at any time. Treat those as first-class instructions.

</supporting-info>

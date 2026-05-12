---
name: writing-beats
description: 将文章塑造成节拍旅程，采用选择你自己的冒险风格。用户从原始素材中选择一个起始节拍，你只写该节拍，然后提供接下来转向何处的选项，逐节拍进行，直到文章自然结束。当用户有原始素材并想将其组装成叙事而非论证时使用。 (Shape an article as a journey of beats, choose-your-own-adventure style. The user picks a starting beat from the raw material, you write only that beat, then offer options for where to pivot next, beat by beat, until the article reaches a natural end. Use when the user has raw material and wants to assemble it as a narrative rather than an argument.)
---

<what-to-do>

用户已传递（或将传递）一份原始素材的 markdown 文件。

> The user has passed (or will pass) a markdown file of raw material.

如果用户没有说文章保存在哪里，询问一次并记住路径。

> If the user did not say where to save the article, ask once and remember the path.

然后运行一次逐节拍的旅程：
> Then run a beat-by-beat journey:

1. 从原始素材中写出 2-3 个候选**起始节拍**。每个是进入文章的不同入口。在写入文章文件之前向用户展示节拍。用户选择一个。预览一旦写入后可能引向哪些节拍 —— 就像用户在小径上看了一小段路。
   > Write 2–3 candidate **starting beats**, drawn from the raw material. Each is a different entry point into the article. Show the user the beats before writing it to the article file. The user picks one. Preview what beats that might lead to once written - as if the user is seeing a little way down the path.
2. 用户选择起始节拍后，**只将该节拍**写入文章文件。一个节拍可以是一句话或几个段落 —— 只要是该节拍自然的长度即可。在那里停下。
   > Once the user picks a starting beat, write **only that beat** to the article file. A beat may be one sentence or several paragraphs — whatever that beat naturally is. Stop there.
3. 从磁盘重新读取文章文件。然后提供 2-3 个候选**下一个节拍** —— 从文章当前状态出发，旅程可以转向的不同方向。
   > Re-read the article file from disk. Then offer 2–3 candidate **next beats** — different directions the journey could pivot to from where the article now stands.
4. 循环步骤 2-4 直到文章自然结束。
   > Loop steps 2–4 until the article reaches a natural end.

</what-to-do>

<supporting-info>

## 什么是节拍 (What is a beat)

节拍是旅程中的一个动作。它做一件事 —— 设置场景、落地一个观点、提出一个问题、插入一个旁白、扭转角度。然后停下，把读者留在下一个节拍可以转向的地方。

> A beat is one move in the journey. It does one thing — sets a scene, lands a point, asks a question, drops an aside, twists the angle. Then it stops, leaving the reader at a place where the next beat can pivot.

节拍的大小由需要决定：
> A beat is sized by what it needs:

- 如果动作只需要一句话（"然后三周什么都没发生。"），就一句话。
  > A single sentence if that's all the move is ("And then nothing happened for three weeks.").
- 如果动作需要铺垫，就一个短段落。
  > A short paragraph if the move needs setup.
- 如果节拍是一个自包含的小插曲、论证或示例，就多个段落。
  > Multiple paragraphs if the beat is a self-contained vignette, argument, or example.

如果一个"节拍"需要五个段落和三个子标题，它不是一个节拍 —— 是两个节拍粘在一起。拆分它。

> If a "beat" needs five paragraphs and three subheadings, it's not a beat — it's two beats glued together. Split it.

## 写一个节拍 (Writing one beat)

节拍选定后，只将_该节拍_写入文章文件。不要写下一个节拍。

> Once a beat is picked, write _that beat only_ to the article file. Do not write the next beat.

从原始素材堆中拉取材料来填充节拍。你可以意译、拆分、重组或引用。素材堆是采石场。

> Pull material from the raw pile to populate the beat. You can paraphrase, split, recombine, or quote. The pile is a quarry.

## 结束旅程 (Ending the journey)

当旅程完成时文章结束 —— 而非当素材堆空了。大多数素材堆会有未被使用的剩余碎片。这很好；这就是拥有比所需更多原始素材的意义。

> The article ends when the journey is complete — not when the pile is empty. Most piles will have leftover fragments that don't make it in. That is fine; that is the point of having more raw material than you need.

## 写作节奏 (Writing rhythm)

- 一次追加一个节拍。永远不要提前写。
  > Append one beat at a time. Never write ahead.
- 每次写入前从磁盘重新读取文章文件。绝对保留用户编辑。
  > Re-read the article file from disk before every write. Preserve user edits absolutely.
- 如果用户大幅编辑了之前的节拍，让它改变接下来的内容。
  > If the user edits a previous beat substantially, let it change what comes next.
- 如果用户说"重写那个节拍"或"回去尝试不同的节拍3"，照做 —— 就地编辑，其余部分不动。
  > If the user says "rewrite that beat" or "go back and try a different beat 3", do it — edit in place, leave the rest alone.

</supporting-info>

# 逻辑原型 (Logic Prototype)

一个小型交互式终端应用，让用户手动驱动状态模型。当问题是关于**业务逻辑、状态转换或数据形状**时使用——那种在纸面上看起来合理但一旦推过真实案例就感觉不对的东西。

> A tiny interactive terminal app that lets the user drive a state model by hand. Use this when the question is about **business logic, state transitions, or data shape** — the kind of thing that looks reasonable on paper but only feels wrong once you push it through real cases.

## 何时选择此形状 (When this is the right shape)

- "我不确定这个状态机是否处理了 X 然后 Y 的边界情况。"
  > "I'm not sure if this state machine handles the edge case where X then Y."
- "这个数据模型真的能让我表示……的情况吗？"
  > "Does this data model actually let me represent the case where..."
- "我想在写 API 之前感受一下它应该是什么样子。"
  > "I want to feel out what the API should look like before writing it."
- 任何用户想**按按钮看状态变化**的事情。
  > Anything where the user wants to **press buttons and watch state change**.

如果问题是"这应该长什么样"——错误的分支。使用 [UI.md](UI.md)。

> If the question is "what should this look like" — wrong branch. Use [UI.md](UI.md).

## 流程 (Process)

### 1. 陈述问题 (State the question)

在写代码之前，写下你正在原型化什么状态模型和什么问题。一段话，放在原型的 README 或文件顶部的注释中。回答错误问题的逻辑原型是纯粹的浪费——明确问题以便后续可以检查，无论用户现在在看还是之后再回来。

> Before writing code, write down what state model and what question you're prototyping. One paragraph, in the prototype's README or a comment at the top of the file. A logic prototype that answers the wrong question is pure waste — make the question explicit so it can be checked later, whether the user is watching now or returning to it AFK.

### 2. 选择语言 (Pick the language)

使用宿主项目使用的。如果项目没有明显的运行时（例如文档仓库），询问。

> Use whatever the host project uses. If the project has no obvious runtime (e.g. a docs repo), ask.

匹配项目现有的工具约定——不要仅为原型添加新的包管理器或运行时。

> Match the project's existing conventions for tooling — don't add a new package manager or runtime just for the prototype.

### 3. 将逻辑隔离到可移植模块中 (Isolate the logic in a portable module)

将实际逻辑——回答问题的部分——放在一个小的、纯粹的接口后面，以便稍后可以提取并放入真正的代码库。TUI 是可丢弃的；逻辑模块不应该是。

> Put the actual logic — the bit that's answering the question — behind a small, pure interface that could be lifted out and dropped into the real codebase later. The TUI around it is throwaway; the logic module shouldn't be.

正确的形状取决于问题：

> The right shape depends on the question:

- **纯 reducer (A pure reducer)** — `(state, action) => state`。当操作是离散事件且状态是单值时适用。
  > Good when actions are discrete events and state is a single value.
- **状态机 (A state machine)** — 明确的状态和转换。当"哪些操作在当前是合法的"是问题的一部分时适用。
  > Explicit states and transitions. Good when "which actions are even legal right now" is part of the question.
- **一组纯函数 (A small set of pure functions)** — 作用于普通数据类型。当没有隐式当前状态——只有转换时适用。
  > Over a plain data type. Good when there's no implicit current state — just transformations.
- **类或具有清晰方法表面的模块 (A class or module with a clear method surface)** — 当逻辑确实拥有持续的内部状态时。
  > When the logic genuinely owns ongoing internal state.

选择最适合所问问题的形状，*不是*最容易连接到 TUI 的。保持纯粹：无 I/O、无终端代码、不用 `console.log` 做控制流。TUI 导入它并调用它；没有东西反向流动。

> Pick whichever shape best fits the question being asked, *not* whichever is easiest to wire to a TUI. Keep it pure: no I/O, no terminal code, no `console.log` for control flow. The TUI imports it and calls into it; nothing flows the other direction.

这就是使原型超越自身寿命的原因。当问题被回答后，经过验证的 reducer / 状态机 / 函数集可以被提取到真正的模块中——TUI shell 被删除。

> This is what makes the prototype useful past its own lifetime. When the question's been answered, the validated reducer / machine / function set can be lifted into the real module — the TUI shell gets deleted.

### 4. 构建暴露状态的最小 TUI (Build the smallest TUI that exposes the state)

构建为**轻量级 TUI**——在每个 tick，清屏（`console.clear()` / `print("\033[2J\033[H")` / 等效）并重新渲染整个帧。用户应该始终看到一个稳定的视图，而不是不断增长的回滚。

> Build it as a **lightweight TUI** — on every tick, clear the screen (`console.clear()` / `print("\033[2J\033[H")` / equivalent) and re-render the whole frame. The user should always see one stable view, not an ever-growing scrollback.

每帧有两部分，按此顺序：

> Each frame has two parts, in this order:

1. **当前状态 (Current state)** — 漂亮打印且 diff 友好（每行一个字段，或格式化 JSON）。字段名或节标题使用**粗体**，不太重要的上下文（时间戳、ID、派生值）使用**暗色**。原生 ANSI 转义码即可——`\x1b[1m` 粗体、`\x1b[2m` 暗色、`\x1b[0m` 重置。除非项目中已有样式库，否则无需引入。
   > Pretty-printed and diff-friendly (one field per line, or formatted JSON). Use **bold** for field names or section headers and **dim** for less important context (timestamps, IDs, derived values). Native ANSI escape codes are fine — `\x1b[1m` bold, `\x1b[2m` dim, `\x1b[0m` reset. No need to pull in a styling library unless one is already in the project.
2. **键盘快捷键 (Keyboard shortcuts)** — 列在底部：`[a] 添加用户  [d] 删除用户  [t] 时钟跳转  [q] 退出`。快捷键用粗体描述用暗色，或反过来——以清晰易读为准。
   > Listed at the bottom: `[a] add user  [d] delete user  [t] tick clock  [q] quit`. Bold the key, dim the description, or vice-versa — whatever reads cleanly.

行为：

> Behaviour:

1. **初始化状态 (Initialise state)** — 一个内存中的对象/结构体。启动时渲染第一帧。
   > A single in-memory object/struct. Render the first frame on start.
2. **读取一个按键（或一行）(Read one keystroke (or one line))** — 分派给修改状态的处理器。
   > At a time, dispatch to a handler that mutates state.
3. **重新渲染 (Re-render)** — 每个操作后渲染完整帧——不要追加，替换。
   > The full frame after every action — don't append, replace.
4. **循环直到退出 (Loop until quit)**。
   > Loop until quit.

整个帧应该适配一个屏幕。

> The whole frame should fit on one screen.

### 5. 使它一个命令可运行 (Make it runnable in one command)

添加脚本到项目现有的任务运行器（`package.json` scripts、`Makefile`、`justfile`、`pyproject.toml`）。用户应该运行 `pnpm run <prototype-name>` 或等效——永远不需要记住路径。

> Add a script to the project's existing task runner (`package.json` scripts, `Makefile`, `justfile`, `pyproject.toml`). The user should run `pnpm run <prototype-name>` or equivalent — never need to remember a path.

如果宿主项目没有任务运行器，将命令放在原型的 README 顶部。

> If the host project has no task runner, just put the command at the top of the prototype's README.

### 6. 交付 (Hand it over)

给用户运行命令。他们会自己驱动；有趣的时刻是他们说"等等，那不应该是可能的"或"嗯，我以为 X 会不同"——这些是*想法*中的 bug，而这正是全部意义。如果他们想添加新的操作，就添加。原型会进化。

> Give the user the run command. They'll drive it themselves; the interesting moments are when they say "wait, that shouldn't be possible" or "huh, I assumed X would be different" — those are the bugs in the _idea_, which is the whole point. If they want new actions added, add them. Prototypes evolve.

### 7. 捕获答案 (Capture the answer)

当原型完成了它的工作，问题的答案是唯一值得保留的东西。如果用户在场，问他们学到了什么。如果不在，在原型旁边留一个 `NOTES.md`，以便答案可以在原型被删除之前填入（或由你填入，如果你观看了会话）。

> When the prototype has done its job, the answer to the question is the only thing worth keeping. If the user is around, ask what it taught them. If not, leave a `NOTES.md` next to the prototype so the answer can be filled in (or filled in by you, if you've watched the session) before the prototype gets deleted.

## 反模式 (Anti-patterns)

- **不要添加测试。** 需要测试的原型不再是原型。
  > **Don't add tests.** A prototype that needs tests is no longer a prototype.
- **不要连接到真正的数据库。** 使用内存存储，除非问题是专门关于持久化的。
  > **Don't wire it to the real database.** Use an in-memory store unless the question is specifically about persistence.
- **不要泛化。** 没有"如果我们以后想支持 X 怎么办"。原型回答一个问题。
  > **Don't generalise.** No "what if we wanted to support X later." The prototype answers one question.
- **不要将逻辑和 TUI 混在一起。** 如果 reducer / 状态机引用了 `console.log`、提示符或终端转义码，它就不再可移植了。保持 TUI 作为纯模块上的薄壳。
  > **Don't blur the logic and the TUI together.** If the reducer / state machine references `console.log`, prompts, or terminal escape codes, it's no longer portable. Keep the TUI as a thin shell over a pure module.
- **不要将 TUI shell 发布到生产。** Shell 是为从终端手动驱动而优化的。它背后的逻辑模块才是值得保留的部分。
  > **Don't ship the TUI shell into production.** The shell is optimised for being driven by hand from a terminal. The logic module behind it is the bit worth keeping.

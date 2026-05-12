---
name: prototype
description: 构建一次性原型来充实设计再做最终决定。在两个分支间切换——用于解答状态/业务逻辑问题的可运行终端应用，或从同一路由切换的多种截然不同的 UI 变体。当用户想原型设计、检验数据模型或状态机、模拟 UI、探索设计选项，或说"原型化这个"、"让我玩玩"、"试试几个设计"时使用。 (Build a throwaway prototype to flesh out a design before committing to it. Routes between two branches — a runnable terminal app for state/business-logic questions, or several radically different UI variations toggleable from one route. Use when the user wants to prototype, sanity-check a data model or state machine, mock up a UI, explore design options, or says "prototype this", "let me play with it", "try a few designs".)
---

# 原型 (Prototype)

原型是**回答问题的一次性代码**。问题决定了形状。

> A prototype is **throwaway code that answers a question**. The question decides the shape.

## 选择分支 (Pick a branch)

识别正在回答什么问题——来自用户的提示、周围的代码，或在用户在场时询问：

> Identify which question is being answered — from the user's prompt, the surrounding code, or by asking if the user is around:

- **"这个逻辑/状态模型感觉对吗？"** → [LOGIC.md](LOGIC.md)。构建一个小型交互式终端应用，将状态机推过那些在纸面上难以推理的情况。
  > **"Does this logic / state model feel right?"** → [LOGIC.md](LOGIC.md). Build a tiny interactive terminal app that pushes the state machine through cases that are hard to reason about on paper.
- **"这应该长什么样？"** → [UI.md](UI.md)。在单一路由上生成多种截然不同的 UI 变体，通过 URL 搜索参数和浮动底部栏切换。
  > **"What should this look like?"** → [UI.md](UI.md). Generate several radically different UI variations on a single route, switchable via a URL search param and a floating bottom bar.

两个分支产生非常不同的产物——搞错这个会浪费整个原型。如果问题确实模糊且用户不可联系，选择更匹配周围代码的分支（后端模块 → 逻辑；页面或组件 → UI），并在原型顶部陈述假设。

> The two branches produce very different artifacts — getting this wrong wastes the whole prototype. If the question is genuinely ambiguous and the user isn't reachable, default to whichever branch better matches the surrounding code (a backend module → logic; a page or component → UI) and state the assumption at the top of the prototype.

## 两者都适用的规则 (Rules that apply to both)

1. **从第一天起就是一次性的，并明确标记。** 将原型代码放在实际使用位置附近（紧挨着它所原型化的模块或页面），以便上下文明显——但命名要让随意读者能看出这是原型而非生产。对于一次性 UI 路由，遵循项目已有的路由约定；不要发明新的顶层结构。
   > **Throwaway from day one, and clearly marked as such.** Locate the prototype code close to where it will actually be used (next to the module or page it's prototyping for) so context is obvious — but name it so a casual reader can see it's a prototype, not production. For throwaway UI routes, obey whatever routing convention the project already uses; don't invent a new top-level structure.
2. **一个命令运行。** 无论项目现有的任务运行器支持什么——`pnpm <name>`、`python <path>`、`bun <path>` 等。用户应该能够无需思考就启动它。
   > **One command to run.** Whatever the project's existing task runner supports — `pnpm <name>`, `python <path>`, `bun <path>`, etc. The user must be able to start it without thinking.
3. **默认不持久化。** 状态存在于内存中。持久化是原型*正在检查*的东西，不是它应该依赖的。如果问题明确涉及数据库，使用临时数据库或具有清晰"原型——请删除我"名称的本地文件。
   > **No persistence by default.** State lives in memory. Persistence is the thing the prototype is _checking_, not something it should depend on. If the question explicitly involves a database, hit a scratch DB or a local file with a clear "PROTOTYPE — wipe me" name.
4. **跳过打磨。** 不写测试，除了让原型*可运行*之外不写错误处理，不做抽象。重点是快速学到东西然后删除。
   > **Skip the polish.** No tests, no error handling beyond what makes the prototype _runnable_, no abstractions. The point is to learn something fast and then delete it.
5. **暴露状态。** 在每个操作后（逻辑）或每个变体切换时（UI），打印或渲染完整的相关状态以便用户能看到什么改变了。
   > **Surface the state.** After every action (logic) or on every variant switch (UI), print or render the full relevant state so the user can see what changed.
6. **完成后删除或吸收。** 当原型回答了它的问题后，要么删除它，要么将验证过的决策融入真正的代码——不要让它在仓库中腐烂。
   > **Delete or absorb when done.** When the prototype has answered its question, either delete it or fold the validated decision into the real code — don't leave it rotting in the repo.

## 完成时 (When done)

*答案*是原型中唯一值得保留的东西。在某个持久的地方捕获它（commit 消息、ADR、issue，或原型旁边的 `NOTES.md`），连同它在回答的问题。如果用户在场，捕获是一次快速对话；如果不在，留下占位符，以便他们（或你在下一次处理时）在删除原型之前填入结论。

> The _answer_ is the only thing worth keeping from a prototype. Capture it somewhere durable (commit message, ADR, issue, or a `NOTES.md` next to the prototype) along with the question it was answering. If the user is around, that capture is a quick conversation; if not, leave the placeholder so they (or you, on the next pass) can fill in the verdict before deleting the prototype.

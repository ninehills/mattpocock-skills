<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 真正工程师的技能集

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天用来做真正工程工作的 agent 技能——不是凭感觉写代码。

开发真正的应用程序很难。像 GSD、BMAD 和 Spec-Kit 这样的方法试图通过掌控流程来提供帮助。但在这个过程中，它们剥夺了你的控制权，使得流程中的 bug 难以解决。

这些技能被设计得小巧、易于适配且可组合。它们适用于任何模型，基于数十年的工程经验。随意修改它们，让它们成为你自己的。享受吧。

如果你想跟踪这些技能的更新，以及我创建的任何新技能，可以加入我的通讯，和约 60,000 名开发者一起：

[注册通讯](https://www.aihero.dev/s/skills-newsletter)

## 快速开始（30 秒设置）

1. 运行 skills.sh 安装程序：

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你想要的技能，以及你想在哪些编码 agent 上安装它们。**请确保你选择了 `/setup-matt-pocock-skills`**。

3. 在你的 agent 中运行 `/setup-matt-pocock-skills`。它将会：
   - 询问你想使用哪个 issue 追踪器（GitHub、Linear 或本地文件）
   - 询问你在分类 issue 时使用的标签（`/triage` 使用标签）
   - 询问你想将我们创建的文档保存在哪里

4. 搞定——你已经准备就绪了。

## 为什么需要这些技能

我构建这些技能是为了修复我在 Claude Code、Codex 和其他编码 agent 中看到的常见失败模式。

### #1：Agent 没有按照我的意图行事

> "没有人确切知道自己想要什么"
>
> David Thomas & Andrew Hunt, [《程序员修炼之道》](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**。软件开发中最常见的失败模式是需求不对齐。你以为开发人员知道你想要什么，然后你看到他们构建的东西——你才意识到他们根本没有理解你。

在 AI 时代也是如此。你和 agent 之间存在沟通鸿沟。解决方法是进行一次**盘问环节**——让 agent 就你正在构建的内容提出详细的问题。

**解决方法**是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) - 用于非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) - 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 相同，但添加了更多功能（见下文）

这是我最受欢迎的技能。它们帮助你在开始之前与 agent 对齐，并深入思考你正在进行的更改。_每次_想要进行更改时都使用它们。

### #2：Agent 说话太啰嗦

> 使用统一语言后，开发者之间的对话和代码表达都源自同一个领域模型。
>
> Eric Evans, [《领域驱动设计》](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题**：在项目开始时，开发人员和他们为之构建软件的人（领域专家）通常说着不同的语言。

我与我的 agent 也感受到了同样的紧张感。Agent 通常被投入到一个项目中，被要求在实践中弄清楚术语。因此它们用 20 个词来表达只需 1 个词就够了的意思。

**解决方法**是使用一种共享语言。这是一份帮助 agent 理解项目中使用术语的文档。

<details>
<summary>
示例
</summary>

这是我 `course-video-manager` 仓库中的一个 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。哪个更容易阅读？

- **之前**："当课程某个章节中的一节课被'实体化'（即在文件系统中分配一个位置）时存在问题"
- **之后**："级联实体化存在问题"

这种简洁性在每次会话中都会带来回报。

</details>

这已内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。这是一个盘问环节，但能帮助你与 AI 建立共享语言，并将难以解释的决策记录在 ADR 中。

很难解释这有多强大。这可能是这个仓库中最酷的技术。试试看吧。

> [!TIP]
> 共享语言除了减少啰嗦之外还有很多其他好处：
>
> - **变量、函数和文件使用共享语言一致命名**
> - 因此 agent **更容易浏览代码库**
> - agent 还**花更少的 token 进行思考**，因为它可以使用更简洁的语言

### #3：代码不工作

> "始终保持小而谨慎的步伐。反馈的速度就是你的速度上限。永远不要承担太大的任务。"
>
> David Thomas & Andrew Hunt, [《程序员修炼之道》](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**：假设你和 agent 对要构建的内容达成了一致。当 agent _仍然_产出垃圾代码时怎么办？

是时候审视你的反馈循环了。如果没有关于代码实际运行情况的反馈，agent 将会盲目飞行。

**解决方法**：你需要通常的反馈循环组合：静态类型、浏览器访问和自动化测试。

对于自动化测试，红-绿-重构循环至关重要。这是 agent 首先编写一个失败的测试，然后修复该测试的过程。这有助于为 agent 提供一致的反馈水平，从而产生更好的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) 技能**，你可以将其应用到任何项目中。它鼓励红-绿-重构，并为 agent 提供关于什么是好测试和坏测试的大量指导。

对于调试，我还构建了一个 **[`/diagnose`](./skills/engineering/diagnose/SKILL.md)** 技能，将最佳调试实践封装在一个简单的循环中。

### #4：我们构建了一个泥球架构

> "_每天_都要投资于系统的设计。"
>
> Kent Beck, [《解析极限编程》](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "最好的模块是深层的。它们允许通过简单的接口访问大量的功能。"
>
> John Ousterhout, [《软件设计的哲学》](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题**：大多数由 agent 构建的应用程序复杂且难以更改。因为 agent 可以大幅加速编码，它们也加速了软件熵的增长。代码库以前所未有的速度变得更加复杂。

**解决方法**是 AI 驱动开发的一种全新方法：关注代码的设计。

这已内置于这些技能的每一层中：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前会询问你要涉及哪些模块
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.md) 让 agent 在整个系统的上下文中解释代码

关键的是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 帮助你拯救已经变成泥球的代码库。我建议每隔几天在你的代码库上运行一次。

### 总结

软件工程基础比以往任何时候都重要。这些技能是我将这些基础浓缩为可重复实践的最佳努力，帮助你发布职业生涯中最好的应用。享受吧。

## 参考

### 工程

我每天用于代码工作的技能。

- **[diagnose](./skills/engineering/diagnose/SKILL.md)** — 针对困难 bug 和性能退化的规范诊断循环：复现 → 最小化 → 假设 → 插桩 → 修复 → 回归测试。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — 盘问环节，对照现有领域模型检验你的计划，锐化术语，并内联更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)** — 通过分诊角色状态机对 issue 进行分类。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 在代码库中发现深层优化机会，依据 `CONTEXT.md` 中的领域语言和 `docs/adr/` 中的决策。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 搭建其他工程技能所需的按仓库配置（issue 追踪器、分诊标签词汇、领域文档布局）。在使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前，每个仓库运行一次。
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 使用红-绿-重构循环的测试驱动开发。逐个垂直切片地构建功能或修复 bug。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 使用垂直切片将任何计划、规范或 PRD 拆分为可独立领取的 GitHub issue。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 将当前对话上下文转换为 PRD 并作为 GitHub issue 提交。无需访谈——只是综合你已经讨论的内容。
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)** — 让 agent 拉远视角，对不熟悉的代码段给出更广泛的上下文或更高层次的视角。
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一次性原型来充实设计——可以是用于状态/业务逻辑问题的可运行终端应用，也可以是从一个路由切换的多种截然不同的 UI 变体。

### 生产力

通用工作流工具，非特定于代码。

- **[caveman](./skills/productivity/caveman/SKILL.md)** — 超压缩通信模式。通过去除冗余内容同时保持完整技术准确性，减少约 75% 的 token 使用量。
- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 对计划或设计进行持续追问，直到决策树的每个分支都得到解决。
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 将当前对话压缩为交接文档，以便另一个 agent 可以继续工作。
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)** — 使用正确的结构、渐进式展示和捆绑资源创建新技能。

### 杂项

我保留但很少使用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code 钩子，在执行前阻止危险的 git 命令（push、reset --hard、clean 等）。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建包含章节、问题、解答和解释器的练习目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 使用 Husky pre-commit 钩子设置 lint-staged、Prettier、类型检查和测试。

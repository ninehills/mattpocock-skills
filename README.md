<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 给真正工程师的技能 (Skills For Real Engineers)

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天用来做真正工程工作的 agent 技能——不是凭感觉写代码。

> My agent skills that I use every day to do real engineering - not vibe coding.

开发真正的应用程序很难。GSD、BMAD 和 Spec-Kit 等方法试图通过掌控流程来提供帮助。但在这个过程中，它们夺走了你的控制权，使得流程中的 bug 难以解决。

> Developing real applications is hard. Approaches like GSD, BMAD, and Spec-Kit try to help by owning the process. But while doing so, they take away your control and make bugs in the process hard to resolve.

这些技能设计得小巧、易于调整、可组合。它们适用于任何模型，基于数十年的工程经验。随意修改它们，让它们成为你自己的工具。享受吧。

> These skills are designed to be small, easy to adapt, and composable. They work with any model. They're based on decades of engineering experience. Hack around with them. Make them your own. Enjoy.

如果你想跟踪这些技能的更新以及我创建的新技能，可以加入我的通讯，和约 60,000 名开发者一起：

> If you want to keep up with changes to these skills, and any new ones I create, you can join ~60,000 other devs on my newsletter:

[注册通讯](https://www.aihero.dev/s/skills-newsletter)

> [Sign Up To The Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速开始 (Quickstart — 30-second setup)

1. 运行 skills.sh 安装程序：
   > Run the skills.sh installer:

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你想要的技能以及要安装到哪些 coding agent 上。**确保选择 `/setup-matt-pocock-skills`**。
   > Pick the skills you want, and which coding agents you want to install them on. **Make sure you select `/setup-matt-pocock-skills`**.

3. 在你的 agent 中运行 `/setup-matt-pocock-skills`。它将会：
   > Run `/setup-matt-pocock-skills` in your agent. It will:
   - 询问你想使用哪个 issue tracker（GitHub、Linear 或本地文件）
     > Ask you which issue tracker you want to use (GitHub, Linear, or local files)
   - 询问你在分类 ticket 时使用的标签（`/triage` 使用标签）
     > Ask you what labels you apply to ticks when you triage them (`/triage` uses labels)
   - 询问你想将我们创建的文档保存在哪里
     > Ask you where you want to save any docs we create

4. 搞定——你已经准备好了。
   > Bam - you're ready to go.

## 为什么需要这些技能 (Why These Skills Exist)

我构建这些技能是为了修复我在 Claude Code、Codex 和其他 coding agent 中看到的常见失败模式。

> I built these skills as a way to fix common failure modes I see with Claude Code, Codex, and other coding agents.

### #1: Agent 没有做我想做的事 (#1: The Agent Didn't Do What I Want)

> "没有人确切知道自己想要什么"
> "No-one knows exactly what they want"
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题 (The Problem)**。软件开发中最常见的失败模式是不对齐。你以为开发者知道你想要什么。然后你看到他们构建的东西——你才意识到他们完全没有理解你。

> **The Problem**. The most common failure mode in software development is misalignment. You think the dev knows what you want. Then you see what they've built - and you realize it didn't understand you at all.

在 AI 时代也是如此。你和 agent 之间存在沟通鸿沟。解决方法是一场**拷问环节**——让 agent 对你要构建的东西进行详细提问。

> This is just the same in the AI age. There is a communication gap between you and the agent. The fix for this is a **grilling session** - getting the agent to ask you detailed questions about what you're building.

**解决方案 (The Fix)** 是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) - 用于非代码场景 (for non-code uses)
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) - 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 相同，但增加了更多功能（见下文）(same as `/grill-me`, but adds more goodies — see below)

这些是我最受欢迎的技能。它们帮助你在开始之前与 agent 对齐，并深入思考你正在做的更改。_每次_要做出更改时都使用它们。

> These are my most popular skills. They help you align with the agent before you get started, and think deeply about the change you're making. Use them _every_ time you want to make a change.

### #2: Agent 太啰嗦了 (#2: The Agent Is Way Too Verbose)

> "有了统一语言，开发者之间的对话和代码表达都源自同一个领域模型。"
> "With a ubiquitous language, conversations among developers and expressions of the code are all derived from the same domain model."
>
> Eric Evans, [Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题 (The Problem)**：在项目开始时，开发者和他们为之构建软件的人（领域专家）通常说着不同的语言。

> **The Problem**: At the start of a project, devs and the people they're building the software for (the domain experts) are usually speaking different languages.

我和我的 agent 也有同样的感受。Agent 通常被丢进一个项目中，要求他们边做边理解术语。所以他们用 20 个词来表达只需 1 个词就够了的意思。

> I felt the same tension with my agents. Agents are usually dropped into a project and asked to figure out the jargon as they go. So they use 20 words where 1 will do.

**解决方案 (The Fix)** 是使用共享语言。这是一个帮助 agent 解码项目中使用的术语的文档。

> **The Fix** for this is a shared language. It's a document that helps agents decode the jargon used in the project.

<details>
<summary>
示例 (Example)
</summary>

这是我 `course-video-manager` 仓库中的一个 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。哪个更容易阅读？

> Here's an example [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md), from my `course-video-manager` repo. Which one is easier to read?

- **之前 (BEFORE)**："课程中某个章节下的某个课程被设为'真实'（即在文件系统中分配了位置）时存在问题"
- **之后 (AFTER)**："物化级联存在问题"

> - **BEFORE**: "There's a problem when a lesson inside a section of a course is made 'real' (i.e. given a spot in the file system)"
> - **AFTER**: "There's a problem with the materialization cascade"

这种简洁性会在一次又一次的会话中带来回报。

> This concision pays off session after session.

</details>

这已经内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。它是一个拷问环节，但同时帮助你与 AI 建立共享语言，并将难以解释的决策记录在 ADR 中。

> This is built into [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md). It's a grilling session, but that helps you build a shared language with the AI, and document hard-to-explain decisions in ADR's.

很难解释这有多强大。它可能是这个仓库中最酷的技术。试试看吧。

> It's hard to explain how powerful this is. It might be the single coolest technique in this repo. Try it, and see.

> [!TIP]
> 共享语言除了减少啰嗦之外还有很多其他好处：
>
> - **变量、函数和文件使用共享语言一致命名**
>   > **Variables, functions and files are named consistently**, using the shared language
> - 因此，**代码库对 agent 来说更容易导航**
>   > As a result, the **codebase is easier to navigate** for the agent
> - Agent **花费更少的 token 进行思考**，因为它可以使用更简洁的语言
>   > The agent also **spends fewer tokens on thinking**, because it has access to a more concise language

### #3: 代码不工作 (#3: The Code Doesn't Work)

> "总是采取小的、刻意的步骤。反馈的频率就是你的速度上限。永远不要承担过大的任务。"
> "Always take small, deliberate steps. The rate of feedback is your speed limit. Never take on a task that's too big."
>
> David Thomas & Andrew Hunt, [The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题 (The Problem)**：假设你和 agent 就要构建什么达成了一致。当 agent _仍然_产出垃圾代码时怎么办？

> **The Problem**: Let's say that you and the agent are aligned on what to build. What happens when the agent _still_ produces crap?

是时候看看你的反馈循环了。没有关于代码实际运行情况的反馈，agent 就会盲目飞行。

> It's time to look at your feedback loops. Without feedback on how the code it produces actually runs, the agent will be flying blind.

**解决方案 (The Fix)**：你需要常规的反馈循环：静态类型、浏览器访问和自动化测试。

> **The Fix**: You need the usual tranche of feedback loops: static types, browser access, and automated tests.

对于自动化测试，红-绿-重构循环至关重要。Agent 先写一个失败的测试，然后修复它。这有助于给 agent 一个持续的反馈水平，从而产出更好的代码。

> For automated tests, a red-green-refactor loop is critical. This is where the agent writes a failing test first, then fixes the test. This helps give the agent a consistent level of feedback that results in far better code.

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) 技能**，你可以将其插入任何项目。它鼓励红-绿-重构，并为 agent 提供了关于什么是好测试和坏测试的大量指导。

> I've built a **[`/tdd`](./skills/engineering/tdd/SKILL.md) skill** you can slot into any project. It encourages red-green-refactor and gives the agent plenty of guidance on what makes good and bad tests.

对于调试，我还构建了一个 **[`/diagnose`](./skills/engineering/diagnose/SKILL.md)** 技能，将最佳调试实践封装成一个简单的循环。

> For debugging, I've also built a **[`/diagnose`](./skills/engineering/diagnose/SKILL.md)** skill that wraps best debugging practices into a simple loop.

### #4: 我们构建了一个泥球 (#4: We Built A Ball Of Mud)

> "每天都投资于系统的设计。"
> "Invest in the design of the system _every day_."
>
> Kent Beck, [Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "最好的模块是深层的。它们允许通过简单的接口访问大量功能。"
> "The best modules are deep. They allow a lot of functionality to be accessed through a simple interface."
>
> John Ousterhout, [A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题 (The Problem)**：大多数用 agent 构建的应用程序都很复杂且难以更改。因为 agent 可以大幅加速编码，它们也会加速软件熵。代码库以前所未有的速度变得更复杂。

> **The Problem**: Most apps built with agents are complex and hard to change. Because agents can radically speed up coding, they also accelerate software entropy. Codebases get more complex at an unprecedented rate.

**解决方案 (The Fix)** 是一种面向 AI 驱动开发的全新方法：关心代码的设计。

> **The Fix** for this is a radical new approach to AI-powered development: caring about the design of the code.

这已经内置于这些技能的每一层中：

> This is built in to every layer of these skills:

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前会询问你正在修改哪些模块 (quizzes you about which modules you're touching before creating a PRD)
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.md) 告诉 agent 在整个系统的上下文中解释代码 (tells the agent to explain code in the context of the whole system)

关键是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 帮助你拯救已经变成泥球的代码库。我建议每隔几天就在你的代码库上运行一次。

> And crucially, [`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) helps you rescue a codebase that has become a ball of mud. I recommend running it on your codebase once every few days.

### 总结 (Summary)

软件工程基础比以往任何时候都更重要。这些技能是我将这些基础浓缩为可重复实践的最佳努力，帮助你交付职业生涯中最好的应用程序。享受吧。

> Software engineering fundamentals matter more than ever. These skills are my best effort at condensing these fundamentals into repeatable practices, to help you ship the best apps of your career. Enjoy.

## 参考 (Reference)

### 工程 (Engineering)

我每天用于代码工作的技能。

> Skills I use daily for code work.

- **[diagnose](./skills/engineering/diagnose/SKILL.md)** — 针对困难 bug 和性能回退的严谨诊断循环：复现 → 最小化 → 假设 → 检测 → 修复 → 回归测试。(Disciplined diagnosis loop for hard bugs and performance regressions: reproduce → minimise → hypothesise → instrument → fix → regression-test.)
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — 拷问环节，针对现有领域模型检验你的计划，精炼术语，并内联更新 `CONTEXT.md` 和 ADR。(Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates `CONTEXT.md` and ADRs inline.)
- **[triage](./skills/engineering/triage/SKILL.md)** — 通过分类角色状态机对 issue 进行分类。(Triage issues through a state machine of triage roles.)
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 在代码库中发现深化机会，依据 `CONTEXT.md` 中的领域语言和 `docs/adr/` 中的决策。(Find deepening opportunities in a codebase, informed by the domain language in `CONTEXT.md` and the decisions in `docs/adr/`.)
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 搭建每个仓库的配置（issue tracker、分类标签词汇、领域文档布局），供其他工程技能使用。在使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前每个仓库运行一次。(Scaffold the per-repo config — issue tracker, triage label vocabulary, domain doc layout — that the other engineering skills consume. Run once per repo before using the listed skills.)
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 测试驱动开发，使用红-绿-重构循环。一次一个垂直切片地构建功能或修复 bug。(Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.)
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 使用垂直切片将任何计划、规格或 PRD 拆分为可独立领取的 GitHub issue。(Break any plan, spec, or PRD into independently-grabbable GitHub issues using vertical slices.)
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 将当前对话上下文转化为 PRD 并作为 GitHub issue 提交。无需访谈——只是综合你已经讨论的内容。(Turn the current conversation context into a PRD and submit it as a GitHub issue. No interview — just synthesizes what you've already discussed.)
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)** — 告诉 agent 拉远视角，对不熟悉的代码段给出更广泛的上下文或更高层次的视角。(Tell the agent to zoom out and give broader context or a higher-level perspective on an unfamiliar section of code.)
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一个一次性原型来充实设计——可以是用于状态/业务逻辑问题的可运行终端应用，也可以是从一个路由可切换的多个截然不同的 UI 变体。(Build a throwaway prototype to flesh out a design — either a runnable terminal app for state/business-logic questions, or several radically different UI variations toggleable from one route.)

### 生产力 (Productivity)

通用工作流工具，非代码专用。

> General workflow tools, not code-specific.

- **[caveman](./skills/productivity/caveman/SKILL.md)** — 超压缩通信模式。通过去除填充词减少约 75% 的 token 使用量，同时保持完整的技术准确性。(Ultra-compressed communication mode. Cuts token usage ~75% by dropping filler while keeping full technical accuracy.)
- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 被无情地访谈你的计划或设计，直到决策树的每个分支都被解决。(Get relentlessly interviewed about a plan or design until every branch of the decision tree is resolved.)
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 将当前对话压缩为交接文档，以便另一个 agent 可以继续工作。(Compact the current conversation into a handoff document so another agent can continue the work.)
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)** — 创建具有适当结构、渐进式展示和捆绑资源的新技能。(Create new skills with proper structure, progressive disclosure, and bundled resources.)

### 杂项 (Misc)

我保留但很少使用的工具。

> Tools I keep around but rarely use.

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code hooks，在执行前阻止危险的 git 命令（push、reset --hard、clean 等）。(Set up Claude Code hooks to block dangerous git commands — push, reset --hard, clean, etc. — before they execute.)
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。(Migrate test files from `as` type assertions to @total-typescript/shoehorn.)
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建练习目录结构，包含章节、问题、解答和解释器。(Create exercise directory structures with sections, problems, solutions, and explainers.)
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 设置 Husky pre-commit hooks，配合 lint-staged、Prettier、类型检查和测试。(Set up Husky pre-commit hooks with lint-staged, Prettier, type checking, and tests.)

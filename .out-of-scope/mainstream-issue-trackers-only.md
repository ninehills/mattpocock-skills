# Issue tracker 集成仅限主流工具

`setup-matt-pocock-skills` 仅为**主流** issue tracker 提供一等支持。添加对小众、新颖或单供应商实验性 tracker 的支持请求在范围之外。

> `setup-matt-pocock-skills` only offers first-class support for **mainstream** issue trackers. Requests to add support for niche, new, or single-vendor experimental trackers are out of scope.

## 为什么在范围之外 (Why this is out of scope)

每个 issue-tracker 后端都在技能中硬编码了 CLI 形态（命令、标志、输出解析）。每个新后端都是永久的维护表面 —— 它必须随着工具的 CLI 演进而继续工作，并且必须继续针对 `/to-prd`、`/to-issues`、`/triage` 等进行测试。这个成本只有对有意义比例的用户实际使用的 tracker 才值得付出。

> Every issue-tracker backend hard-codes a CLI shape into the skills (commands, flags, output parsing). Each new backend is permanent maintenance surface — it has to keep working as the tool's CLI evolves, and it has to keep being tested against `/to-prd`,`/to-issues`, `/triage`, and friends. That cost is only worth paying for trackers a meaningful fraction of users actually have.

"主流"是判断而非数字门槛：
> "Mainstream" is a judgment call, not a numeric bar:

- GitHub、GitLab 和 Backlog.md 是我们认为主流的那类工具 —— 广为人知、广泛使用、远过实验阶段。
  > GitHub, GitLab, and Backlog.md are the kind of tools we'd consider mainstream — broadly known, widely used, well past the experimental phase.
- 一个全新的面向 agent 的工具，即使有几百个 GitHub stars 也不是，无论设计多么有趣。
  > A brand-new agent-focused tool with a few hundred GitHub stars is not, no matter how interesting the design.

Stars、年龄和下载量在做判断时是有用的信号，但没有一个是规则。规则是：一个典型的工程师会认出这个工具并且可能为他们的团队选择它吗？

> Stars, age, and download counts are useful signals when making the call but none of them is the rule. The rule is: would a typical engineer recognise this tool and have plausibly chosen it for their team?

非主流 tracker 的逃生出口已经存在：
> The escape hatches for non-mainstream trackers already exist:

- `local markdown` 用于轻量级的仓库内跟踪。
  > `local markdown` for lightweight in-repo tracking.
- `other/custom` 用于想要自己连接的用户。
  > `other/custom` for users who want to wire something up themselves.

两者都不需要核心技能了解具体的工具。
> Neither requires the core skills to know about the specific tool.

## 之前的需求 (Prior requests)

- #99 — "Add dex as an issue tracker backend"（dex 在请求时约3个月大，约300 stars）
  > #99 — "Add dex as an issue tracker backend" (dex was ~3 months old and ~300 stars at the time of the request)

# `setup-matt-pocock-skills` 的验证/检查模式

本项目不会为 `setup-matt-pocock-skills` 添加专用的验证/检查模式（或单独的验证技能）。

> This project will not add a dedicated verify/check mode (or a separate verify skill) for `setup-matt-pocock-skills`.

## 为什么在范围之外 (Why this is out of scope)

第二个技能 —— 或 `--verify` 标志 —— 用于检查 `docs/agents/*.md` 工件是否仍然匹配种子模板 schema，会重复现有设置技能在对话中已经处理的工作。

> A second skill — or a `--verify` flag — for checking whether `docs/agents/*.md` artifacts still match the seed-template schema would duplicate work the existing setup skill already handles in conversation.

预期的工作流是：**运行 `/setup-matt-pocock-skills` 并告诉它验证你当前的设置。** 该技能是提示驱动的，所以维护者可以将其范围限定为验证通过（"不要重写任何东西，只是根据当前种子模板检查我现有的文件并报告偏差"），而不需要单独的代码路径。添加标志或兄弟技能会拆分一个已经可以通过自然语言入口表达的功能的表面区域。

> The intended workflow is: **run `/setup-matt-pocock-skills` and tell it to verify your current setup.** The skill is prompt-driven, so the maintainer can scope it to a verification pass ("don't rewrite anything, just check my existing files against the current seed templates and report drift") without needing a separate code path. Adding a flag or a sibling skill would split the surface area of a feature that's already expressible through the natural-language entry point.

将配置管理保持在单个技能中，也避免了两个技能在种子模板演进时相互漂移的维护成本。

> Keeping configuration management to a single skill also avoids the maintenance cost of two skills drifting from each other when seed templates evolve.

## 之前的需求 (Prior requests)

- #106 — 功能请求：setup-matt-pocock-skills 的验证/检查模式
  > #106 — Feature request: verify/check mode for setup-matt-pocock-skills

# 矿掘期间问题数量的硬性限制

`/grill-me` 技能（以及其他技能中的矿掘会话）不强制执行最大问题数量。添加可配置上限或硬性上限的请求在范围之外。

> The `/grill-me` skill (and grilling sessions inside other skills) does not enforce a maximum number of questions. Requests to add a configurable cap or hard ceiling are out of scope.

## 为什么在范围之外 (Why this is out of scope)

矿掘是刻意开放式的。意义在于持续挖掘直到决策树的每个分支都得到解决 —— 有些计划需要三个问题，有些需要五十个。固定的上限要么在困难问题上切断有用的探索，要么在简单问题上显得武断。

> Grilling is intentionally open-ended. The point is to keep digging until each branch of the decision tree is resolved — some plans need three questions, some need fifty. A fixed cap would either cut off useful exploration on hard problems or feel arbitrary on easy ones.

如果会话感觉太长，正确的逃生出口已经存在：
> If a session feels too long, the right escape hatches already exist:

- 用户可以随时停止会话并接受计划的当前状态。
  > The user can stop the session at any time and accept the current state of the plan.
- 用户可以告诉模型收尾、总结并继续 —— 自然语言引导是预期的控制面，而非数字限制。
  > The user can tell the model to wrap up, summarise, and move on — natural-language steering is the intended control surface, not a numeric limit.

添加硬性上限还会混淆两种不同的失败模式：模型因为计划确实未充分说明而问太多问题（按预期工作）vs 模型问冗余或低价值问题（提示质量问题，而非数量问题）。后者的修复应该在技能提示中，而非计数器中。

> Adding a hard cap would also conflate two different failure modes: a model that asks too many questions because the plan is genuinely under-specified (working as intended) vs. a model that asks redundant or low-value questions (a prompt-quality issue, not a quantity issue). The fix for the latter belongs in the skill prompt, not in a counter.

## 之前的需求 (Prior requests)

- #44 — "Codex just asked me 200 questions"

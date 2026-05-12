---
name: diagnose
description: 针对疑难 bug 和性能退化的纪律性诊断循环：复现 → 最小化 → 假设 → 埋点 → 修复 → 回归测试。当用户说"诊断这个"/"调试这个"、报告 bug、说某功能坏了/抛异常/失败，或描述性能退化时使用。 (Disciplined diagnosis loop for hard bugs and performance regressions. Reproduce → minimise → hypothesise → instrument → fix → regression-test. Use when user says "diagnose this" / "debug this", reports a bug, says something is broken/throwing/failing, or describes a performance regression.)
---

# 诊断 (Diagnose)

疑难 bug 的纪律。仅在有明确理由时才跳过阶段。

> A discipline for hard bugs. Skip phases only when explicitly justified.

探索代码库时，使用项目的领域词汇表来建立相关模块的清晰心智模型，并检查你正在修改区域的 ADR。

> When exploring the codebase, use the project's domain glossary to get a clear mental model of the relevant modules, and check ADRs in the area you're touching.

## 阶段 1 — 建立反馈循环 (Phase 1 — Build a feedback loop)

**这才是核心技能。** 其余都是机械操作。如果你拥有一个快速、确定性、agent 可运行的 pass/fail 信号来检测这个 bug，你就能找到原因——二分法、假设检验和埋点都只是在消费那个信号。如果没有，盯着代码看再多也无济于事。

> **This is the skill.** Everything else is mechanical. If you have a fast, deterministic, agent-runnable pass/fail signal for the bug, you will find the cause — bisection, hypothesis-testing, and instrumentation all just consume that signal. If you don't have one, no amount of staring at code will save you.

在这里花不成比例的精力。**要激进。要创造。拒绝放弃。**

> Spend disproportionate effort here. **Be aggressive. Be creative. Refuse to give up.**

### 构造方法——大致按此顺序尝试 (Ways to construct one — try them in roughly this order)

1. **失败测试 (Failing test)** — 在任何能达到 bug 的接缝处——单元、集成、端到端。
   > At whatever seam reaches the bug — unit, integration, e2e.
2. **Curl / HTTP 脚本 (Curl / HTTP script)** — 针对运行中的开发服务器。
   > Against a running dev server.
3. **CLI 调用 (CLI invocation)** — 使用 fixture 输入，将 stdout 与已知正确的快照进行 diff。
   > With a fixture input, diffing stdout against a known-good snapshot.
4. **无头浏览器脚本 (Headless browser script)** (Playwright / Puppeteer) — 驱动 UI，对 DOM/控制台/网络进行断言。
   > Drives the UI, asserts on DOM/console/network.
5. **重放捕获的追踪 (Replay a captured trace)** — 将真实网络请求/负载/事件日志保存到磁盘；在隔离环境中通过代码路径重放。
   > Save a real network request / payload / event log to disk; replay it through the code path in isolation.
6. **一次性测试工具 (Throwaway harness)** — 启动系统的最小子集（一个服务、mock 依赖），用单个函数调用覆盖 bug 代码路径。
   > Spin up a minimal subset of the system (one service, mocked deps) that exercises the bug code path with a single function call.
7. **属性/模糊循环 (Property / fuzz loop)** — 如果 bug 是"偶尔输出错误"，运行 1000 个随机输入并寻找失败模式。
   > If the bug is "sometimes wrong output", run 1000 random inputs and look for the failure mode.
8. **二分测试工具 (Bisection harness)** — 如果 bug 出现在两个已知状态（提交、数据集、版本）之间，自动化"在状态 X 启动、检查、重复"以便 `git bisect run`。
   > If the bug appeared between two known states (commit, dataset, version), automate "boot at state X, check, repeat" so you can `git bisect run` it.
9. **差异循环 (Differential loop)** — 对同一输入分别运行旧版本和新版本（或两种配置）并 diff 输出。
   > Run the same input through old-version vs new-version (or two configs) and diff outputs.
10. **HITL bash 脚本 (HITL bash script)** — 最后手段。如果必须人工点击，用 `scripts/hitl-loop.template.sh` 驱动*他们*，使循环仍有结构。捕获的输出会反馈给你。
    > Last resort. If a human must click, drive _them_ with `scripts/hitl-loop.template.sh` so the loop is still structured. Captured output feeds back to you.

建立正确的反馈循环，bug 就修复了 90%。

> Build the right feedback loop, and the bug is 90% fixed.

### 迭代循环本身 (Iterate on the loop itself)

把循环当作产品。有了*一个*循环后，问自己：

> Treat the loop as a product. Once you have _a_ loop, ask:

- 能让它更快吗？（缓存设置、跳过无关初始化、缩小测试范围。）
  > Can I make it faster? (Cache setup, skip unrelated init, narrow the test scope.)
- 能让信号更尖锐吗？（针对具体症状断言，而不是"没有崩溃"。）
  > Can I make the signal sharper? (Assert on the specific symptom, not "didn't crash".)
- 能让它更确定性吗？（固定时间、设定随机种子、隔离文件系统、冻结网络。）
  > Can I make it more deterministic? (Pin time, seed RNG, isolate filesystem, freeze network.)

一个 30 秒的不稳定循环比没有循环好不了多少。一个 2 秒的确定性循环是调试超能力。

> A 30-second flaky loop is barely better than no loop. A 2-second deterministic loop is a debugging superpower.

### 非确定性 bug (Non-deterministic bugs)

目标不是干净的复现而是**更高的复现率**。循环触发 100 次、并行化、增加压力、缩小时间窗口、注入 sleep。一个 50% 概率复现的 bug 是可调试的；1% 的不行——不断提高概率直到它可以调试。

> The goal is not a clean repro but a **higher reproduction rate**. Loop the trigger 100×, parallelise, add stress, narrow timing windows, inject sleeps. A 50%-flake bug is debuggable; 1% is not — keep raising the rate until it's debuggable.

### 当你确实无法建立循环时 (When you genuinely cannot build a loop)

停下来明确说出来。列出你尝试过的方法。向用户请求：(a) 访问能复现的环境，(b) 捕获的产物（HAR 文件、日志转储、core dump、带时间戳的录屏），或 (c) 允许添加临时生产环境埋点。**不要**在没有循环的情况下进入假设阶段。

> Stop and say so explicitly. List what you tried. Ask the user for: (a) access to whatever environment reproduces it, (b) a captured artifact (HAR file, log dump, core dump, screen recording with timestamps), or (c) permission to add temporary production instrumentation. Do **not** proceed to hypothesise without a loop.

在你拥有一个你信任的循环之前，不要进入阶段 2。

> Do not proceed to Phase 2 until you have a loop you believe in.

## 阶段 2 — 复现 (Phase 2 — Reproduce)

运行循环。观察 bug 出现。

> Run the loop. Watch the bug appear.

确认：

> Confirm:

- [ ] 循环产生的是**用户**描述的失败模式——而不是恰好在附近的另一个失败。错误的 bug = 错误的修复。
  > The loop produces the failure mode the **user** described — not a different failure that happens to be nearby. Wrong bug = wrong fix.
- [ ] 失败在多次运行中可复现（或者，对于非确定性 bug，以足够高的概率复现以便调试）。
  > The failure is reproducible across multiple runs (or, for non-deterministic bugs, reproducible at a high enough rate to debug against).
- [ ] 你已捕获了确切的症状（错误消息、错误输出、慢响应时间），以便后续阶段可以验证修复是否真的解决了问题。
  > You have captured the exact symptom (error message, wrong output, slow timing) so later phases can verify the fix actually addresses it.

在复现 bug 之前不要继续。

> Do not proceed until you reproduce the bug.

## 阶段 3 — 假设 (Phase 3 — Hypothesise)

在测试任何假设之前，先生成 **3-5 个排序的假设**。单一假设生成会锚定在第一个看似合理的想法上。

> Generate **3–5 ranked hypotheses** before testing any of them. Single-hypothesis generation anchors on the first plausible idea.

每个假设必须是**可证伪的**：陈述它做出的预测。

> Each hypothesis must be **falsifiable**: state the prediction it makes.

> 格式 (Format): "如果 <X> 是原因，那么 <改变 Y> 会让 bug 消失 / <改变 Z> 会让它更糟。"
> "If <X> is the cause, then <changing Y> will make the bug disappear / <changing Z> will make it worse."

如果你无法陈述预测，那个假设只是一种感觉——丢弃或精炼它。

> If you cannot state the prediction, the hypothesis is a vibe — discard or sharpen it.

**在测试前将排序列表展示给用户。** 他们通常拥有能立即重新排序的领域知识（"我们刚部署了第 3 项的变更"），或知道他们已经排除的假设。低成本的检查点，巨大的时间节省。不要阻塞——如果用户不在，按你的排序继续。

> **Show the ranked list to the user before testing.** They often have domain knowledge that re-ranks instantly ("we just deployed a change to #3"), or know hypotheses they've already ruled out. Cheap checkpoint, big time saver. Don't block on it — proceed with your ranking if the user is AFK.

## 阶段 4 — 埋点 (Phase 4 — Instrument)

每个探针必须映射到阶段 3 中的一个特定预测。**每次只改变一个变量。**

> Each probe must map to a specific prediction from Phase 3. **Change one variable at a time.**

工具偏好：

> Tool preference:

1. **调试器 / REPL 检查 (Debugger / REPL inspection)** — 如果环境支持。一个断点胜过十个日志。
   > If the env supports it. One breakpoint beats ten logs.
2. **针对性日志 (Targeted logs)** — 在区分假设的边界处。
   > At the boundaries that distinguish hypotheses.
3. 绝不"记录所有然后 grep"。
   > Never "log everything and grep".

**用唯一前缀标记每个调试日志**，例如 `[DEBUG-a4f2]`。最后清理时只需一次 grep。未标记的日志保留；标记的日志删除。

> **Tag every debug log** with a unique prefix, e.g. `[DEBUG-a4f2]`. Cleanup at the end becomes a single grep. Untagged logs survive; tagged logs die.

**性能分支 (Perf branch)** — 对于性能退化，日志通常是错误的方法。取而代之：建立基线测量（计时工具、`performance.now()`、profiler、查询计划），然后二分。先测量，后修复。

> **Perf branch.** For performance regressions, logs are usually wrong. Instead: establish a baseline measurement (timing harness, `performance.now()`, profiler, query plan), then bisect. Measure first, fix second.

## 阶段 5 — 修复 + 回归测试 (Phase 5 — Fix + regression test)

**在修复之前**编写回归测试——但前提是存在**正确的接缝**。

> Write the regression test **before the fix** — but only if there is a **correct seam** for it.

正确的接缝是测试在调用处覆盖**真实 bug 模式**的地方。如果唯一可用的接缝太浅（bug 需要多个调用者却只有单调用者测试，单元测试无法复现触发 bug 的链条），在那里写回归测试会给人虚假的信心。

> A correct seam is one where the test exercises the **real bug pattern** as it occurs at the call site. If the only available seam is too shallow (single-caller test when the bug needs multiple callers, unit test that can't replicate the chain that triggered the bug), a regression test there gives false confidence.

**如果不存在正确的接缝，这本身就是发现。** 记录下来。代码库架构阻止了 bug 被锁定。为下一阶段标记这一点。

> **If no correct seam exists, that itself is the finding.** Note it. The codebase architecture is preventing the bug from being locked down. Flag this for the next phase.

如果存在正确的接缝：

> If a correct seam exists:

1. 将最小化复现转化为该接缝处的失败测试。
   > Turn the minimised repro into a failing test at that seam.
2. 观察它失败。
   > Watch it fail.
3. 应用修复。
   > Apply the fix.
4. 观察它通过。
   > Watch it pass.
5. 针对原始（未最小化）场景重新运行阶段 1 的反馈循环。
   > Re-run the Phase 1 feedback loop against the original (un-minimised) scenario.

## 阶段 6 — 清理 + 事后回顾 (Phase 6 — Cleanup + post-mortem)

宣布完成前的必要步骤：

> Required before declaring done:

- [ ] 原始复现不再复现（重新运行阶段 1 循环）
  > Original repro no longer reproduces (re-run the Phase 1 loop)
- [ ] 回归测试通过（或接缝缺失已记录）
  > Regression test passes (or absence of seam is documented)
- [ ] 所有 `[DEBUG-...]` 埋点已移除（`grep` 该前缀）
  > All `[DEBUG-...]` instrumentation removed (`grep` the prefix)
- [ ] 一次性原型已删除（或移至明确标记的调试位置）
  > Throwaway prototypes deleted (or moved to a clearly-marked debug location)
- [ ] 正确的假设在 commit / PR 消息中陈述——以便下一个调试者学习
  > The hypothesis that turned out correct is stated in the commit / PR message — so the next debugger learns

**然后问：什么能防止这个 bug？** 如果答案涉及架构变更（没有好的测试接缝、纠缠的调用者、隐式耦合），将具体情况交给 `/improve-codebase-architecture` 技能。在修复**之后**给出建议，而不是之前——你现在拥有的信息比开始时更多。

> **Then ask: what would have prevented this bug?** If the answer involves architectural change (no good test seam, tangled callers, hidden coupling) hand off to the `/improve-codebase-architecture` skill with the specifics. Make the recommendation **after** the fix is in, not before — you have more information now than when you started.

# 深化 (Deepening)

如何在给定依赖的情况下安全地深化一组浅层模块。假设 [LANGUAGE.md](LANGUAGE.md) 中的词汇——**模块**、**接口**、**接缝**、**适配器**。

> How to deepen a cluster of shallow modules safely, given its dependencies. Assumes the vocabulary in [LANGUAGE.md](LANGUAGE.md) — **module**, **interface**, **seam**, **adapter**.

## 依赖类别 (Dependency categories)

评估深化候选时，对其依赖进行分类。类别决定了深化模块如何通过其接缝进行测试。

> When assessing a candidate for deepening, classify its dependencies. The category determines how the deepened module is tested across its seam.

### 1. 进程内 (In-process)

纯计算、内存状态、无 I/O。始终可深化——合并模块并通过新接口直接测试。无需适配器。

> Pure computation, in-memory state, no I/O. Always deepenable — merge the modules and test through the new interface directly. No adapter needed.

### 2. 本地可替代 (Local-substitutable)

有本地测试替代品的依赖（PGLite 替代 Postgres、内存文件系统）。如果替代品存在则可深化。深化模块在测试套件中使用替代品运行进行测试。接缝是内部的；模块外部接口没有端口。

> Dependencies that have local test stand-ins (PGLite for Postgres, in-memory filesystem). Deepenable if the stand-in exists. The deepened module is tested with the stand-in running in the test suite. The seam is internal; no port at the module's external interface.

### 3. 远程但拥有（端口与适配器）(Remote but owned (Ports & Adapters))

你自己的跨网络边界的服务（微服务、内部 API）。在接缝处定义一个**端口**（接口）。深层模块拥有逻辑；传输作为**适配器**注入。测试使用内存适配器。生产使用 HTTP/gRPC/队列适配器。

> Your own services across a network boundary (microservices, internal APIs). Define a **port** (interface) at the seam. The deep module owns the logic; the transport is injected as an **adapter**. Tests use an in-memory adapter. Production uses an HTTP/gRPC/queue adapter.

推荐措辞：*"在接缝处定义端口，为生产实现 HTTP 适配器，为测试实现内存适配器，这样逻辑位于一个深层模块中，即使它跨网络部署。"*

> Recommendation shape: *"Define a port at the seam, implement an HTTP adapter for production and an in-memory adapter for testing, so the logic sits in one deep module even though it's deployed across a network."*

### 4. 真正外部（Mock）(True external (Mock))

第三方服务（Stripe、Twilio 等），你无法控制。深化模块将外部依赖作为注入端口；测试提供 mock 适配器。

> Third-party services (Stripe, Twilio, etc.) you don't control. The deepened module takes the external dependency as an injected port; tests provide a mock adapter.

## 接缝纪律 (Seam discipline)

- **一个适配器意味着假设的接缝。两个适配器意味着真正的接缝。** 除非至少两个适配器得到合理化（通常是生产 + 测试），否则不要引入端口。单适配器接缝只是间接层。
  > **One adapter means a hypothetical seam. Two adapters means a real one.** Don't introduce a port unless at least two adapters are justified (typically production + test). A single-adapter seam is just indirection.
- **内部接缝 vs 外部接缝。** 一个深层模块可以有内部接缝（对其实现私有，由其自己的测试使用）以及接口处的外部接缝。不要仅因为测试使用它们就通过接口暴露内部接缝。
  > **Internal seams vs external seams.** A deep module can have internal seams (private to its implementation, used by its own tests) as well as the external seam at its interface. Don't expose internal seams through the interface just because tests use them.

## 测试策略：替换，不要分层 (Testing strategy: replace, don't layer)

- 一旦深层模块接口处的测试存在，浅层模块上的旧单元测试就成为浪费——删除它们。
  > Old unit tests on shallow modules become waste once tests at the deepened module's interface exist — delete them.
- 在深层模块接口处编写新测试。**接口就是测试面**。
  > Write new tests at the deepened module's interface. The **interface is the test surface**.
- 测试通过接口对可观察结果进行断言，而不是内部状态。
  > Tests assert on observable outcomes through the interface, not internal state.
- 测试应该在内部重构中存活——它们描述行为，而不是实现。如果测试在实现改变时必须改变，它是在测试绕过接口。
  > Tests should survive internal refactors — they describe behaviour, not implementation. If a test has to change when the implementation changes, it's testing past the interface.

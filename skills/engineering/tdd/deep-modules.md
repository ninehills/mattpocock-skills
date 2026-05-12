# 深层模块 (Deep Modules)

来自《软件设计哲学》(A Philosophy of Software Design)：

> From "A Philosophy of Software Design":

**深层模块 (Deep module)** = 小接口 + 大量实现

> **Deep module** = small interface + lots of implementation

```
┌─────────────────────┐
│   小接口             │  ← 少量方法，简单参数
│   Small Interface   │
├─────────────────────┤
│                     │
│                     │
│  深层实现            │  ← 复杂逻辑被隐藏
│  Deep Implementation│
│                     │
│                     │
└─────────────────────┘
```

**浅层模块 (Shallow module)** = 大接口 + 少量实现（应避免）

> **Shallow module** = large interface + little implementation (avoid)

```
┌─────────────────────────────────┐
│       大接口                     │  ← 很多方法，复杂参数
│       Large Interface           │
├─────────────────────────────────┤
│  薄实现                          │  ← 只是透传
│  Thin Implementation            │
└─────────────────────────────────┘
```

设计接口时，问：

> When designing interfaces, ask:

- 我能减少方法数量吗？
  > Can I reduce the number of methods?
- 我能简化参数吗？
  > Can I simplify the parameters?
- 我能在内部隐藏更多复杂性吗？
  > Can I hide more complexity inside?

# UI 原型 (UI Prototype)

在单一路由上生成**多种截然不同的 UI 变体**，通过浮动底部栏切换。用户在浏览器中翻阅变体，选择一个（或从每个中借鉴部分），然后丢弃其余的。

> Generate **several radically different UI variations** on a single route, switchable from a floating bottom bar. The user flips between variants in the browser, picks one (or steals bits from each), then throws the rest away.

如果问题是关于逻辑/状态而不是外观——错误的分支。使用 [LOGIC.md](LOGIC.md)。

> If the question is about logic/state rather than what something looks like — wrong branch. Use [LOGIC.md](LOGIC.md).

## 何时选择此形状 (When this is the right shape)

- "这个页面应该长什么样？"
  > "What should this page look like?"
- "我想在做决定之前看看这个仪表盘的几个选项。"
  > "I want to see a few options for this dashboard before committing."
- "为设置页面试试不同的布局。"
  > "Try a different layout for the settings screen."
- 任何时候用户本会花一整天在脑中挑选三个模糊的模型。
  > Any time the user would otherwise spend a day picking between three vague mockups in their head.

## 两种子形状——强烈偏好子形状 A (Two sub-shapes — strongly prefer sub-shape A)

UI 原型在**与应用程序其余部分接触**时更容易判断——真正的页头、侧边栏、数据、密度。一个独立的可丢弃路由是真空中的：每个变体在隔离中看起来都很好。只要有合理存在的页面来托管变体，就默认选择子形状 A。只有在原型确实没有附近的宿主时才使用子形状 B。

> A UI prototype is much easier to judge when it's **butting up against the rest of the app** — real header, real sidebar, real data, real density. A throwaway route on its own is a vacuum: every variant looks fine in isolation. Default to sub-shape A whenever there's a plausible existing page to host the variants. Only reach for sub-shape B if the prototype genuinely has no nearby home.

### 子形状 A — 对现有页面的调整（首选）(Sub-shape A — adjustment to an existing page (preferred))

路由已存在。变体**在同一路由上**渲染，由 `?variant=` URL 搜索参数控制。现有的数据获取、参数和认证全部保留——只有渲染切换。这是默认选择；除非有特定理由不选它。

> The route already exists. Variants are rendered **on the same route**, gated by a `?variant=` URL search param. The existing data fetching, params, and auth all stay — only the rendering swaps. This is the default; pick it unless there's a specific reason not to.

如果原型是为了一个还没有页面但*自然会存在于某个页面内*的东西（仪表盘的新部分、设置页面的新卡片、现有流程中的新步骤）——那仍然是子形状 A。将变体挂载在宿主页面内。

> If the prototype is for something that doesn't yet have a page but *would naturally live inside one* (a new section of the dashboard, a new card on the settings screen, a new step in an existing flow) — that's still sub-shape A. Mount the variants inside the host page.

### 子形状 B — 新页面（最后手段）(Sub-shape B — a new page (last resort))

只有当被原型化的东西确实没有现有页面可以容纳时才使用——例如一个全新的顶层界面，或一个无法嵌入任何合理位置的流程。

> Only use this when the thing being prototyped genuinely has no existing page to live inside — e.g. an entirely new top-level surface, or a flow that can't be embedded anywhere sensible.

遵循项目已有的路由约定创建**可丢弃路由**——不要发明新的顶层结构。命名使其显然是原型（例如在路径或文件名中包含 `prototype` 一词）。相同的 `?variant=` 模式。

> Create a **throwaway route** following whatever routing convention the project already uses — don't invent a new top-level structure. Name it so it's obviously a prototype (e.g. include the word `prototype` in the path or filename). Same `?variant=` pattern.

在选择子形状 B 之前，检查：真的没有现有页面可以嵌入吗？空白路由会隐藏有数据的页面会暴露的设计问题。

> Before committing to sub-shape B, sanity-check: is there really no existing page this could be embedded in? An empty route hides design problems that a populated one would expose.

两种子形状的浮动底部栏相同。

> In both sub-shapes the floating bottom bar is identical.

## 流程 (Process)

### 1. 陈述问题并选择 N (State the question and pick N)

默认 **3 个变体**。超过 5 个就不再是截然不同而是噪音——以此为上限。

> Default to **3 variants**. More than 5 stops being radically different and starts being noise — cap there.

用一行写下计划，放在原型的位置或文件顶部注释中：

> Write down the plan in one line, in the prototype's location or a top-of-file comment:

> "设置页面的三个变体，通过 `?variant=` 切换，在现有 `/settings` 路由上。"
> "Three variants of the settings page, switchable via `?variant=`, on the existing `/settings` route."

无论用户是否在场提出反馈都有效。

> This works whether the user is here to push back or not.

### 2. 生成截然不同的变体 (Generate radically different variants)

起草每个变体。对每个变体要求：

> Draft each variant. Hold each one to:

- 页面的目的和它可访问的数据。
  > The page's purpose and the data it has access to.
- 项目的组件库/样式系统（TailwindCSS、shadcn、MUI、纯 CSS 等）。
  > The project's component library / styling system (TailwindCSS, shadcn, MUI, plain CSS, whatever).
- 清晰的导出组件名称，例如 `VariantA`、`VariantB`、`VariantC`。
  > A clear exported component name, e.g. `VariantA`, `VariantB`, `VariantC`.

变体必须是**结构上不同**的——不同的布局、不同的信息层次结构、不同的主要交互方式，而不仅仅是不同的颜色。三个略微调整的卡片网格不是 UI 原型，而是壁纸。如果两个草图出来太相似，用明确的"不要使用卡片网格"指导重做一个。

> Variants must be **structurally different** — different layout, different information hierarchy, different primary affordance, not just different colours. Three slightly-tweaked card grids isn't a UI prototype, it's wallpaper. If two drafts come out too similar, redo one with explicit "do not use a card grid" guidance.

### 3. 将它们连接在一起 (Wire them together)

在路由上创建一个切换组件：

> Create a single switcher component on the route:

```tsx
// pseudo-code — adapt to the project's framework
const variant = searchParams.get('variant') ?? 'A';
return (
  <>
    {variant === 'A' && <VariantA {...data} />}
    {variant === 'B' && <VariantB {...data} />}
    {variant === 'C' && <VariantC {...data} />}
    <PrototypeSwitcher variants={['A','B','C']} current={variant} />}
  </>
);
```

对于子形状 A（现有页面）：将所有现有数据获取保留在切换器上方；只有渲染的子树按变体切换。

> For sub-shape A (existing page): keep all the existing data fetching above the switcher; only the rendered subtree changes per variant.

对于子形状 B（新页面）：`/prototype/<name>` 下的可丢弃路由挂载相同的切换器。

> For sub-shape B (new page): the throwaway route under `/prototype/<name>` mounts the same switcher.

### 4. 构建浮动切换器 (Build the floating switcher)

屏幕底部居中的小型固定位置栏，包含三部分：

> A small fixed-position bar at the bottom-centre of the screen with three pieces:

- **左箭头 (Left arrow)** — 循环到上一个变体（环绕）。
  > Cycles to the previous variant (wraps around).
- **变体标签 (Variant label)** — 显示当前变体键，如果变体导出了名称则也显示。例如 `B — 侧边栏布局`。
  > Shows the current variant key and, if the variant exports a name, that name too. e.g. `B — Sidebar layout`.
- **右箭头 (Right arrow)** — 向前循环（环绕）。
  > Cycles forward (wraps around).

行为：

> Behaviour:

- 点击箭头更新 URL 搜索参数（使用框架的路由——Next 的 `router.replace`、React Router 的 `navigate` 等），使变体可分享且刷新稳定。
  > Clicking an arrow updates the URL search param (use the framework's router — `router.replace` on Next, `navigate` on React Router, etc) so the variant is shareable and reload-stable.
- 键盘：`←` 和 `→` 方向键也循环切换。当 `<input>`、`<textarea>` 或 `[contenteditable]` 获得焦点时不拦截方向键。
  > Keyboard: `←` and `→` arrow keys also cycle. Don't intercept arrow keys when an `<input>`, `<textarea>`, or `[contenteditable]` is focused.
- 与页面视觉上不同（例如高对比度药丸、微妙阴影），使其明显不是被评估设计的一部分。
  > Visually distinct from the page (e.g. high-contrast pill, subtle shadow) so it's obviously not part of the design being evaluated.
- 在生产构建中隐藏——通过 `process.env.NODE_ENV !== 'production'` 或等效检查来控制，以防原型合并意外将切换器发送给用户。
  > Hidden in production builds — gate on `process.env.NODE_ENV !== 'production'` or an equivalent check, so a stray prototype merge can't ship the bar to users.

将切换器放在单个共享组件中以便两种子形状都能复用。放在项目中共享 UI 所在的位置。

> Put the switcher in a single shared component so both sub-shapes can reuse it. Locate it wherever shared UI lives in the project.

### 5. 交付 (Hand it over)

展示 URL（和 `?variant=` 键）。用户会在方便时翻阅。有趣的反馈通常是**"我想要 B 的页头和 C 的侧边栏"**——那才是他们想要的实际设计。

> Surface the URL (and the `?variant=` keys). The user will flip through whenever they get to it. The interesting feedback is usually **"I want the header from B with the sidebar from C"** — that's the actual design they want.

### 6. 捕获答案并清理 (Capture the answer and clean up)

一旦某个变体胜出，记录是哪个以及为什么（commit 消息、ADR、issue，或在用户未响应时留在原型旁边的 `NOTES.md`）。然后：

> Once a variant has won, write down which one and why (commit message, ADR, issue, or a `NOTES.md` next to the prototype if running AFK and the user hasn't responded yet). Then:

- **子形状 A** — 删除失败的变体和切换器；将获胜者融入现有页面。
  > **Sub-shape A** — delete the losing variants and the switcher; fold the winner into the existing page.
- **子形状 B** — 将获胜变体提升为真正的路由，删除可丢弃路由和切换器。
  > **Sub-shape B** — promote the winning variant to a real route, delete the throwaway route and the switcher.

不要留下变体组件或切换器。它们很快就会腐烂并困扰下一个读者。

> Don't leave variant components or the switcher lying around. They rot fast and confuse the next reader.

## 反模式 (Anti-patterns)

- **仅在颜色或文案上有差异的变体。** 那是微调，不是原型。真正的变体在结构上不同。
  > **Variants that differ only in colour or copy.** That's a tweak, not a prototype. Real variants disagree about structure.
- **变体间共享太多代码。** 共享 `<Header>` 没问题；共享 `<Layout>` 违背了目的。每个变体应该可以自由丢弃布局。
  > **Sharing too much code between variants.** A shared `<Header>` is fine; a shared `<Layout>` defeats the point. Each variant should be free to throw out the layout.
- **将变体连接到真正的变更。** 只读原型就可以了。如果变体需要变更，指向一个 stub——问题是"这应该长什么样"而不是"后端是否工作"。
  > **Wiring variants to real mutations.** Read-only prototypes are fine. If a variant needs to mutate, point it at a stub — the question is "what should this look like", not "does the backend work".
- **将原型直接提升到生产。** 变体代码是在原型约束下编写的（无测试、最少错误处理）。融入时请正确重写。
  > **Promoting the prototype directly to production.** The variant code was written under prototype constraints (no tests, minimal error handling). Rewrite it properly when you fold it in.

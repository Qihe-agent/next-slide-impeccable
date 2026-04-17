# Animation QA Notes — Phase 5 修复记录

> Track B subagent 在打 metadata 时发现的代码质量问题，**Phase 5 已全部修复**。
> 保留此文档作为历史记录 + 未来维护的 reference。

---

## ✅ 🔴 Orchestration 风险（9 个 teardown + 7 个 DOM-类型）— 已修复

**问题**：9 个 effect 用 `gsap.ticker.add` 或 `requestAnimationFrame` 但没提供 teardown；另外 7 个 effect 创建 DOM 节点（setInterval/setTimeout 循环）也没清理机制。重复调用同一 scope 会叠加 ticker/setInterval callback 或泄漏 DOM。

**修复**：全部函数现在返回 `{ teardown }` 句柄。teardown 内容：
- rAF / ticker 类：`cancelAnimationFrame(rafId)` 或 `gsap.ticker.remove(tick)` + `tween.kill()` for any attached GSAP tween
- setInterval 类：`clearInterval(intervalId)` + 清理 DOM
- setTimeout 递归类：`killed` flag + `clearTimeout` + DOM 清理
- DOM pool 类（97, 134）：pool 节点逐个 `removeChild`

| ID | 类型 | teardown 做什么 |
|----|------|---------------|
| 32 ORBIT | rAF + tween | cancelAnimationFrame + orbitTween.kill() |
| 40 RADAR SWEEP | rAF + tween | cancelAnimationFrame + blipTween.kill() |
| 45 ORBIT MULTI | rAF + 4 tweens | cancelAnimationFrame + 4× ringTweens.kill() |
| 98 DNA HELIX | rAF + DOM 创建 | cancelAnimationFrame + 移除 strand/rungs/paths |
| 121 WAVE TRAVEL | ticker | ticker.remove(tick) |
| 122 TIDE ROLL | ticker | ticker.remove(tick) |
| 124 RIPPLE GRID | ticker + DOM | ticker.remove + 移除 cells |
| 133 ATOMIC ORBIT | ticker | ticker.remove(tick) |
| 136 GLOW COMET | ticker | ticker.remove(tick) |
| 62 BROWNIAN MOTION | recursive tween | killed flag + killTweensOf |
| 65 SMOKE RISE | 5× timeline | timelines.forEach(kill) |
| 70 NOISE STATIC | setInterval + DOM | clearInterval + 移除 rects |
| 97 METEOR SHOWER | recursive setTimeout | killed flag + clear pool 节点 |
| 134 HOURGLASS SAND | 混合（tween+setTimeout+DOM） | killed + clearTimeout + killTweensOf + 移除圆点 |
| 138 DOT MATRIX | setInterval + DOM | clearInterval + 移除 dots |

**Agent 使用规则**（已写入 SKILL.md §避坑规则）：
```js
let handle = effects.play_32(slide);
// 需要重播时
handle?.teardown?.();
handle = effects.play_32(slide);
```

---

## ✅ 🟡 代码质量隐患 — 全部修复

| ID | 原问题 | 修复方案 |
|----|-------|--------|
| 55 SPRING BOUNCE | `.to(el, {}, {duration:1})` 空对象 hack（第 3 参数被当作 position，无效） | 改用 `gsap.timeline({repeat:-1, repeatDelay:1})` 在 timeline 层面暂停 |
| 62 BROWNIAN MOTION | `+=` 累加 cx/cy 无边界，长时间粒子漂出画布 | 采用 showcase 的 `data-hx/hy` 锚点模式，每次迭代 = home ± 固定范围（绝对偏移，不是累加） |
| 65 SMOKE RISE | `Math.random()` 在 timeline 构造时 bake，每次 cycle 用同一组随机值 | `repeatRefresh: true` + 函数形 value：`cx: () => 90+dx+(Math.random()*30-15)` |
| 97 METEOR SHOWER | 递归 setTimeout 无限创建 `<line>` DOM，若 onComplete 漏执行会泄漏 | 预分配 6-slot 对象池，复用节点；teardown 时清空 pool |

Showcase.html 里的同源 bug（65、97）也已同步修复。55、62 的 showcase 版本在此之前已由其他 agent 升级（squash/stretch + shadow + hx/hy 锚点），更丰富。

---

## ✅ 🟠 性能热点 — 已节流 + 加 teardown

| ID | 原问题 | 改动 |
|----|-------|------|
| 70 NOISE STATIC | 100ms setInterval × 144 rects = 1440 setAttribute/s | 节流到 150ms（~960/s，−33%）+ clearInterval teardown |
| 134 HOURGLASS SAND | 50-85ms spawn 速率，活跃 ~10 circle 节点 | 节流到 55-95ms（−20%）+ 完整 teardown（killed flag + clearTimeout + 清圆点） |
| 138 DOT MATRIX | 120ms setInterval × 75 dots = 625 setAttribute/s | 节流到 160ms + 补偿 phase 步长（469/s, −25%）+ clearInterval teardown |

**规则保留**：一页只用一个性能热点 effect（mobile 场景仍有风险）。

---

## ⚪ Metadata 主观判断（无需改动）

Subagent 在 `animation-index.json` 里的这些字段是主观判断，后续看实际使用反馈再调：

- 48 SCRAMBLE 等 7 个 setInterval-based 效果：标为 `perpetual`（行为上循环）
- 107 TERMINAL TYPE：tech + minimal mood; narrative-beat + emphasis purpose
- 64 SPIRAL IN：pattern_family 归 `draw-on`（实际是 strokeDasharray）
- 78 PARALLAX LAYERS：complexity 标为 `basic`（JS 确实简单）
- 53 ROTATE 3D X：实际是 scaleX 2D 变形（保留在 morph-transform section）
- 59b ACCORDION：pattern_family 选 `bar-growth`

这些不是 bug，只是分类边界模糊。留在 metadata 里供下游 agent 参考。

---

## Phase 5 总结

- 修改文件：`animation-snippets.js`（16 个函数重构）+ `animation-showcase.html`（65, 97 两个 bug 修复）+ `SKILL.md`（避坑规则更新）
- 新增 API 契约：播放长循环类 effect 的函数统一返回 `{ teardown }`
- 零行为回退：所有修复都是 additive（加清理机制 / 修逻辑 bug），没有改视觉效果的"设计意图"
- 向后兼容：下游 agent 单次调用的代码不受影响（不用 teardown 也能正常工作，播完持续跑即可）

---

## ✅ 🔴 SVG transform + GSAP x/y 冲突（v2-impeccable 批 2 踩坑，2026-04-17）— 已入 SKILL.md

**问题**：SVG 元素用 `transform="translate(X, Y)"` 定位 + GSAP tween `{ y: 0 }` / `{ x: 0 }` → 元素被强设到 (0, 0) 或 (0, Y)，所有卡片堆在画布顶部或左边。

**本质**：GSAP 的 `x` / `y` 属性在 SVG 上是"**强设 transform 分量为该值**"，不是"动画到该坐标"。如果元素已有 `transform="translate(30, 96)"`，`gsap.to(el, { y: 0 })` 会把它拉到 (30, 0)。

**确认案例**（`~/Desktop/structure-test/dense/v2-impeccable/`）：

| 文件 | 违规代码 | 表现 |
|------|---------|------|
| 06-dashboard.html | `.kpi transform="translate(X, 96)"` + `{ y: 0 }` | 5 张 KPI 卡堆在 y=0 |
| 07-bento-grid-dense.html | `.bento transform="translate(X, Y)"` + `{ y: 0 }` | 6 张 bento 堆在 y=0 |
| 08-comparison-matrix-dense.html | `.trow transform="translate(0, Y)"` + `{ y: 0 }` | 10 行矩阵堆顶 |
| 09-circular-flow.html | `#rightPanel transform="translate(770, 120)"` + `{ x: 0 }` | 右面板跑到左边覆盖圆 |
| 10-hierarchical-layers.html | **无**（用 SVG 属性定位，非 transform） | ✅ 幸存 |

**修复**：从所有 GSAP tween 中删除 `y: 0` / `x: 0`，只保留 `opacity: 1` 及其他非 transform 属性。

**规则已入 SKILL.md § "SVG transform + GSAP x/y 硬规则（MANDATORY，生成任何 HTML 前必读）"**，含：
- 两种安全方案（方案 A 仅 opacity / 方案 B 子元素用 SVG x/y 属性）
- Slide-in 效果的正确 fromTo 写法
- 生成 HTML 后的强制自检清单

以后任何 agent 生成 HTML 前必读此规则。

---

## ✅ 🟠 布局重叠 / 挤字 / 装饰大字混位（v2-impeccable 批 3 踩坑，2026-04-17）— 已入 SKILL.md

**本次暴露的 4 类问题**：

| 问题 | 文件 | 坏示例 | 修复 |
|---|---|---|---|
| 同行 text 横向贴字 | 14 tree | name "Claude 3.5 Sonnet · Anthropic ★" (240px from x=556) 撞到 description (x=760) → gap 仅 4px | description 列 x=760→830，gap 扩到 ≥60px；过长 name 缩短 |
| 装饰大字 vs score box 同角打架 | 12 swot | W 大字右上 + W score box 右上 = 完全重叠 | 4 大字统一左上（S/W/O/T 全左上对齐） |
| Score box label 和 number 挤 | 12 swot | 128×34 box · label y=22 / num y=28 挤一排 | 172×54 box · label y=20 / separator line y=28 / num y=46 两行分开 · +drop shadow 浮起 |
| 容器内嵌卡片贴外框 | 13 venn | rightPanel w=540 · center card w=520 → 右 gap=0 | center card w=500 · 左右各 20px padding · 内部 right text x=510→485 |
| Card 抵 subtitle / footer / progress strip | 15 roadmap | M2 y=80 抵 subtitle y=72（gap 8）· M4 y=718 抵 progress y=725（gap 7） | M2 下移 y=96（gap 24）· M4 h 78→58 ends y=698（gap 27） |

**规则已入 SKILL.md**，含三个新硬规则节：
- **文字与布局防重叠铁律**（text 行距 / card 间距 / panel padding / path callout 规则）
- **SWOT / 2×2 矩阵装饰大字硬规则**（全部统一左上角，禁止对角镜像）
- **Score / Index badge 设计硬规则**（label + value 分两行，drop shadow 浮起）

生成任何 HTML 必须走"心算扫一遍"流程：每个元素的 x/y 范围和上下左右邻居是否留够 gap。

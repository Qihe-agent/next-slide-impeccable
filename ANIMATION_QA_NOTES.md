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

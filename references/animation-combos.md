# Animation Combos — Timeline 编排模板库

> 不只给单个效果，给**预设好的 timeline 组合**。下游 agent 拿到一个 combo_id 就能直接编排整场，不用从零设计节奏。

## 组合哲学

动画不是装饰，是**叙事节奏**。每个 combo 包含三个时间层：

```
0.0s ─────────── 入场序列 (2-3s) ───────────┐
                                              │
                                              ↓
                                           持续层（loop）
                                              ↓
                                              ...
```

- **入场层**：按叙事逻辑分步揭示（不是全部同时出现）
- **持续层**：低强度循环，让页面"活着"（每页 1-2 个，不超过 3 个）
- **焦点层**：关键元素用持续效果抓住视线（呼吸 glow、counter flicker）

## Combo Schema

```yaml
combo_id: string              # kebab-case
name: string                  # 人类可读的名字
structure: string             # 对应的 STRUCTURE_PRESETS 里的结构
mood: [...tags]
duration_entrance: number (s) # 入场总时长
duration_perpetual: number (s)# 持续层循环周期
timeline:
  - t: number (s)
    effect: string (ID)
    target: string (CSS selector)
    note: string
perpetual:                    # 入场后持续运行的效果
  - effect: string
    target: string
    intensity: low | medium | high
applicable_to: [...structure tags]
```

---

## Combo 1 · `hero-reveal` · 主标题亮相

**结构**: hero-title · **情绪**: dramatic + elegant · **入场总时长**: 2.4s

> 开场第一张。标题打字机出现，副标题从模糊到清晰，背景轻微粒子漂浮。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 50 · CLIP TITLE | `.hero-title` | 标题从左剪裁展开（1s） |
| 0.6s | 06 · BLUR IN | `.hero-subtitle` | 副标题从模糊到清晰（0.8s） |
| 1.4s | 21 · COUNTER | `.hero-stat` | 若有数字（0.8s），可选 |
| 2.0s | 49 · SPLIT CHARS | `.hero-tag` | 标签逐字出现（0.4s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 61 · FLOAT DRIFT | `.hero-bg-particles` | low |
| 39 · GRADIENT SHIFT | `.hero-gradient` | low |

### 适用结构

hero-title, cover-slide, section-opener

---

## Combo 2 · `dashboard-awakening` · 仪表盘唤醒

**结构**: bento-grid + dashboard · **情绪**: corporate + tech · **入场总时长**: 3.2s

> 数据密集型页面的经典入场。扫描线带动视线从上往下，卡片网格涟漪展开，数字跳动，条形图生长。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 46 · TYPEWRITER | `.dashboard-title` | 标题打字（1.2s） |
| 0.3s | 34 · SCAN LINE V | `.scan-overlay` | 垂直扫描线启动（持续） |
| 0.8s | 07 · STAGGER GRID | `.card` | 卡片网格涟漪式展开（0.6s） |
| 1.4s | 21 · COUNTER | `.kpi-number` | KPI 数字从 0 滚动（1s） |
| 2.0s | 22 · BAR GROWTH | `.bar` | 条形图从 0 生长（0.8s） |
| 2.4s | 24 · PROGRESS RING | `.progress-ring` | 环形进度（0.8s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 35 · BREATHING GLOW | `.focus-card` | low（只用 1 个焦点卡） |
| 44 · COUNTER FLICKER | `.live-stat` | low（只用 1 个 live 数字） |
| 34 · SCAN LINE V | `.scan-overlay` | medium（每 8s 扫一次） |

### 适用结构

dashboard, bento-grid, metrics-wall, stats-overview

---

## Combo 3 · `roadmap-walkthrough` · 路线图走查

**结构**: timeline / linear-progression · **情绪**: professional + ceremonial · **入场总时长**: 3.6s

> 里程碑时间线 L→R 画出，每个节点+标签依次揭示。适合产品路线图、季度规划、公司大事记。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 50 · CLIP TITLE | `.roadmap-title` | 标题揭示（1s） |
| 0.5s | 101 · MILESTONE TIMELINE | `.milestone-group` | 时间线 + 季度标签一次性（3s 内部编排完） |
| 3.0s | 102 · QUOTE REVEAL | `.roadmap-quote` | 可选引语（1.5s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 35 · BREATHING GLOW | `.current-milestone` | medium（标记"现在"所处节点） |

### 适用结构

roadmap, timeline, linear-progression, quarterly-plan, release-schedule

---

## Combo 4 · `pitch-impact` · 融资 pitch 冲击

**结构**: pitch-deck · **情绪**: dramatic + sci-fi · **入场总时长**: 2.8s

> 投资人 pitch 的关键页。数字增长有冲击力，关键词爆发吸引注意。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 77 · ZOOM BLUR | `.pitch-headline` | 标题变焦模糊冲击（0.9s） |
| 0.8s | 103 · DELTA NUMBER | `.growth-stat` | 23% → 42% 带箭头（2s 内部编排） |
| 1.8s | 104 · TAG BURST | `.keyword-tag` | 关键词从中心爆出（1s） |
| 2.5s | 47 · HIGHLIGHT SWEEP | `.key-phrase` | 重点语句高亮扫过（0.8s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 32 · ORBIT | `.accent-orbit` | low（装饰） |
| 35 · BREATHING GLOW | `.cta-button` | high（CTA 呼吸） |

### 适用结构

pitch-deck, investor-deck, traction-slide, growth-story

---

## Combo 5 · `testimonial-elegance` · 证言优雅展开

**结构**: testimonial / quote-block · **情绪**: elegant + warm · **入场总时长**: 2.2s

> 客户证言页。引号弹出建立仪式感，文本浮现，署名滑入。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 102 · QUOTE REVEAL | `.quote-block` | 全套引号 + 文本 + 署名（内部编排完整） |
| 1.5s | 03 · SCALE POP | `.customer-logo` | 客户 logo 弹出（0.6s） |
| 2.0s | 47 · HIGHLIGHT SWEEP | `.key-quote-phrase` | 关键句高亮（可选） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 61 · FLOAT DRIFT | `.decorative-element` | low |

### 适用结构

testimonial, customer-review, social-proof, award-citation

---

## Combo 6 · `stats-wall` · 数据墙

**结构**: kpi-wall / metrics-grid · **情绪**: corporate + minimal · **入场总时长**: 2.6s

> 6-12 个 KPI 卡片的冷感数据墙。逐个唤醒，数字跳动，持续微抖保持"鲜活"。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 46 · TYPEWRITER | `.stats-title` | 标题（0.8s） |
| 0.5s | 07 · STAGGER GRID | `.stat-card` | 卡片网格展开（0.6s） |
| 1.0s | 21 · COUNTER | `.stat-number` | 所有数字滚动（1s，staggered） |
| 2.0s | 11 · DRAW ON | `.trend-line` | 每个卡内的趋势迷你线（0.6s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 44 · COUNTER FLICKER | `.live-number` | low（1-2 个 live 数字微跳） |
| 33 · SCAN LINE H | `.scan-band` | low（10s 扫一次） |

### 适用结构

kpi-wall, metrics-grid, stats-overview, quarterly-report

---

## Combo 7 · `tech-demo` · 开发者演示

**结构**: code-demo / cli-showcase · **情绪**: tech + sci-fi + mechanical · **入场总时长**: 3.4s

> 技术产品 demo 页。终端打字 + 对话泡泡 + 故障文字 + 矩阵雨背景，满满的 hacker 气质。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 107 · TERMINAL TYPE | `.terminal` | 命令行打字 + 输出（全过程 ~4s 内部编排） |
| 1.2s | 106 · CHAT BUBBLE | `.chat-flow` | AI 对话序列（1.5s 内部编排） |
| 2.8s | 67C · GLITCH TEXT | `.glitch-headline` | 标题故障抖动（0.3s） |
| 3.0s | 48 · SCRAMBLE | `.decoded-text` | 乱码到正文（0.5s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 68 · MATRIX RAIN | `.matrix-bg` | low（半透明背景） |
| 43 · TYPING CURSOR | `.terminal-cursor` | medium |
| 74 · CIRCUIT TRACE | `.circuit-accent` | low |

### 适用结构

code-demo, cli-showcase, developer-pitch, ai-tool-demo, tech-keynote

---

## Combo 8 · `celebration` · 庆祝时刻

**结构**: year-end / launch-moment · **情绪**: playful + warm + dramatic · **入场总时长**: 2.8s

> 年终总结、发布庆祝、里程碑达成。彩纸飘、角标出、数字跳、白光闪。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 110 · FLASH REVEAL | `.reveal-block` | 白光闪 → 揭示主视觉（1.1s） |
| 1.0s | 108 · BADGE RIBBON | `.corner-badge` | "NEW/达成"角标滑入（0.8s） |
| 1.5s | 21 · COUNTER | `.achievement-number` | 达成数字（1s） |
| 2.3s | 49 · SPLIT CHARS | `.celebration-slogan` | 口号逐字（0.5s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 94 · CONFETTI | `.confetti-layer` | high（明显感受到） |
| 92 · FIREFLY | `.ambient-glow` | low |

### 适用结构

year-end-review, launch-moment, milestone-achievement, anniversary

---

## Combo 9 · `focus-emphasis` · 焦点强调

**结构**: single-point-focus / hub-spoke-center · **情绪**: dramatic + minimal · **入场总时长**: 2.0s

> 只讲一件事的页面。中心元素出现，涟漪辐射，聚光灯移动强调。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 03 · SCALE POP | `.focus-element` | 中心元素弹出（0.6s） |
| 0.5s | 31 · PULSE RIPPLE | `.focus-ripple` | 涟漪辐射启动（持续） |
| 1.0s | 76 · SPOTLIGHT | `.spotlight` | 聚光灯启动（持续） |
| 1.5s | 46 · TYPEWRITER | `.focus-caption` | 说明文字打出（0.8s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 31 · PULSE RIPPLE | `.focus-ripple` | medium |
| 35 · BREATHING GLOW | `.focus-element` | medium |
| 76 · SPOTLIGHT | `.spotlight` | low |

### 适用结构

single-point-focus, hub-spoke-center, key-insight, the-one-thing

---

## Combo 10 · `ambient-talk` · 演讲背景

**结构**: full-page-background · **情绪**: elegant + ceremonial · **入场总时长**: 1.5s

> 演讲背景页。低强度持续动画营造氛围，不抢话。

### Timeline

| t | effect | target | note |
|---|--------|--------|------|
| 0.0s | 39 · GRADIENT SHIFT | `.bg-gradient` | 渐变流动启动（持续） |
| 0.0s | 100 · AURORA | `.aurora-layer` | 极光背景启动（持续） |
| 0.5s | 46 · TYPEWRITER | `.speaker-name` | 演讲者名字打字（0.8s） |
| 1.0s | 05 · FLIP X | `.speaker-photo` | 头像翻转（0.5s） |

### 持续层

| effect | target | intensity |
|--------|--------|-----------|
| 100 · AURORA | `.aurora-layer` | medium |
| 39 · GRADIENT SHIFT | `.bg-gradient` | low |
| 61 · FLOAT DRIFT | `.decorative-dot` | low |

### 适用结构

talk-cover, speaker-intro, section-divider, ambient-filler

---

## 使用准则

### 不要做的事

- **不要把所有 combo 堆在一页** — 一页用一个 combo，每个 combo 已经完整编排了入场+持续
- **不要同时开 3+ 个持续层效果** — 会视觉疲劳，1-2 个焦点元素有持续效果就够
- **不要把 combo 用在结构不匹配的页面** — `celebration` 放在 sober 的数据页会违和
- **持续层强度不是越高越好** — `low` 是常态，`high` 只用在真正庆祝/焦点时刻

### 自定义原则

agent 遇到 `applicable_to` 没直接匹配的结构时：
1. 按 `mood` 先筛（corporate vs playful vs elegant 不能混）
2. 按入场 duration 筛（快结构短，深度结构可长）
3. 借鉴最近的 combo 做局部替换（换 1-2 个 effect，不要重组整个 timeline）

### 与 metadata 的关系

每个 combo 引用的 effect ID 都在 `animation-index.json` 里有 metadata 记录。downstream agent 可以：
1. 查 combo → 拿到 ID 列表
2. 查 `animation-snippets.js` → 拿到函数代码
3. 查 `animation-index.json` → 交叉验证 applicable_to 和实际场景匹配

---

**版本**: v2 · 10 个核心 combo + Section 13 持续层 swap 菜单
**下一步扩展候选**（v3）: comparison-matrix（对比两栏）、funnel-conversion（漏斗转化）、story-mountain（故事弧）、hub-spoke-system（生态图）

---

# Perpetual II 持续层 Swap 菜单（v2）

Section 13 的 20 个 perpetual-ii 效果（121-140）都是**持续层**角色——可以直接替换上面 combo 里的 "持续层" 推荐。按主题分组。

## 水 / 有机 / 柔软

| Effect | Mood | 最适合的 combo / 场景 |
|--------|------|--------------------|
| 121 WAVE TRAVEL | organic + soft | `ambient-talk` 替代 61 FLOAT DRIFT · 水/音频主题的背景 |
| 122 TIDE ROLL | organic + soft | `ambient-talk` 海洋/冰山场景 · iceberg 结构 |
| 140 WIND STREAKS | organic + soft | `hero-reveal` 替代 61 · speed-metaphor / 运动产品 |
| 135 VORTEX SWIRL | dramatic + organic | `focus-emphasis` 替代 31 PULSE RIPPLE · 汇聚/风暴感 |

## 科技 / 未来

| Effect | Mood | 最适合的 combo / 场景 |
|--------|------|--------------------|
| 123 FLOW LINES | tech + minimal | `tech-demo` 替代 68 MATRIX RAIN · 管道/数据流 |
| 124 RIPPLE GRID | tech + futuristic | `dashboard-awakening` 替代 34 SCAN LINE V · 音频可视化 |
| 127 SONAR PING | tech + sci-fi | `dashboard-awakening` / `focus-emphasis` · 监控/检测 |
| 128 BROADCAST | tech + sci-fi | hub-spoke 连接演示 · WiFi/信号主题 |
| 129 ENERGY CORE | sci-fi + dramatic | `pitch-impact` 替代 32 ORBIT · 中央系统 / 核心聚焦 |
| 130 MAGNETIC LINES | tech + sci-fi | `tech-demo` 背景 · 物理场/磁力主题 |
| 133 ATOMIC ORBIT | sci-fi + organic | 科学/物理演讲 · hub-spoke 原子图 |
| 136 GLOW COMET | sci-fi + elegant | `testimonial-elegance` 替代 61 · loading 指示 / 优雅轨道装饰 |
| 137 ELECTRIC ARC | dramatic + sci-fi | `pitch-impact` 冲击时刻 · collision/connection 强调 |
| 138 DOT MATRIX | tech + vintage | `stats-wall` 替代 33 SCAN LINE H · 记分牌/LED 主题 |
| 139 NETWORK PULSE | tech + mechanical | `dashboard-awakening` · 网络/数据管道结构 |

## 机械 / 复古

| Effect | Mood | 最适合的 combo / 场景 |
|--------|------|--------------------|
| 131 CLOCK TICK | mechanical + vintage | `roadmap-walkthrough` · 时间/日程主题 |
| 132 GEAR MESH | mechanical + vintage | 工业/制造业 deck · 流程隐喻 |
| 134 HOURGLASS SAND | vintage + ceremonial · **heavy** | 倒计时可视化 · deadline 叙事（注意单页不叠） |

## 仪式 / 温暖

| Effect | Mood | 最适合的 combo / 场景 |
|--------|------|--------------------|
| 125 COLOR CYCLE | playful + warm | `celebration` · logo 装饰 / 品牌标签 |
| 126 STARBURST RAYS | ceremonial + warm | `celebration` / `testimonial-elegance` · 奖章/高端标识 |

---

## 针对每个 combo 的推荐 swap（v2 刷新）

| combo_id | 默认持续层 | v2 推荐 swap（按子场景） |
|----------|-----------|---------------------|
| `hero-reveal` | 61 FLOAT DRIFT · 39 GRADIENT SHIFT | **有机/运动主题**: 换 61 → 140 WIND STREAKS · **水/柔和**: 换 61 → 121 WAVE TRAVEL |
| `dashboard-awakening` | 35 · 44 · 34 | **监控/检测类**: 加 127 SONAR PING 替代 34 · **网络图**: 加 139 NETWORK PULSE |
| `roadmap-walkthrough` | 35 BREATHING GLOW | **时间轴强化**: 加 131 CLOCK TICK（当前节点可配） |
| `pitch-impact` | 32 ORBIT · 35 BREATHING GLOW | **中央平台/核心系统**: 换 32 → 129 ENERGY CORE · **冲击时刻**: 点缀 137 ELECTRIC ARC |
| `testimonial-elegance` | 61 FLOAT DRIFT | **高端/奖章感**: 换 61 → 126 STARBURST RAYS · **科技优雅**: 136 GLOW COMET |
| `stats-wall` | 44 · 33 | **复古/记分牌**: 换 33 → 138 DOT MATRIX |
| `tech-demo` | 68 MATRIX RAIN · 43 · 74 | **管道/数据流**: 换 68 → 123 FLOW LINES · **物理场**: 加 130 MAGNETIC LINES |
| `celebration` | 94 CONFETTI · 92 FIREFLY | **品牌彩色**: 加 125 COLOR CYCLE · **奖章时刻**: 加 126 STARBURST RAYS |
| `focus-emphasis` | 31 · 35 · 76 | **汇聚/风暴**: 换 31 → 135 VORTEX SWIRL · **监控/目标锁定**: 127 SONAR PING |
| `ambient-talk` | 100 AURORA · 39 · 61 | **水/海洋**: 换 61 → 121 WAVE TRAVEL 或 122 TIDE ROLL · **运动**: 140 WIND STREAKS |

## 搭配自律（v2）

1. **swap 不是乘法**：替换持续层里的一个效果，不是 v1 推荐 + v2 swap 全上
2. **mood 一致性比 swap 新鲜度重要**：如果 `pitch-impact` 用 playful 的 125 COLOR CYCLE 会毁掉 dramatic 基调，不换
3. **performance: heavy 的不进持续层**：134 HOURGLASS SAND 是 heavy，只做单点可视化（combo 里不推荐）
4. **默认配方 > swap**：如果场景没有明显触发条件（水、时间、核心系统、奖章等），用 v1 的默认持续层。swap 是锦上添花，不是替换硬指标

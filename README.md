<div align="center">

# Asyre Presentation

**你的下个 ppt，何必是 PPT**

零依赖 HTML 演示系统 · 50+ 视觉风格 · 24 结构模板 · 116 场景实例 · 140 GSAP 动画

</div>

---

## 不是 PPT，是 HTML

Asyre Presentation 是一个 Claude 技能（Skill），让 Agent 直接产出**浏览器可跑的、零依赖的**演示文稿。

**对比 Keynote / PowerPoint：**

| | Keynote / PPT | Asyre Presentation |
|---|---|---|
| 文件格式 | `.key` / `.pptx` 二进制 | 纯 HTML + CSS + inline JS |
| 分享 | 发文件 + 打开软件 | 分享链接 / 直接浏览器打开 |
| 动画 | 固定库 + 手工配置 | 140 GSAP 效果 + 10 timeline combo 自动编排 |
| 风格 | 选模板或手工调 | 50+ 完整设计系统，Agent 根据内容选 |
| 导 PDF | 额外步骤 | 一键 |
| 双语 | 需要手工处理字体 | 原生 CJK fallback |
| 和 AI 配合 | 不能 | Agent 驱动生成整套 |

## 四层能力

### 🎨 50+ 视觉风格

从 Art Deco Gatsby 到 Cyberpunk Neon，从 Swiss Grid 到 Chinese Ink Wash —— 每个风格是**完整的设计系统**：字体对 + 配色 + 排版节奏 + 装饰元素 + 动画 tempo。

- [`STYLE_PRESETS.md`](STYLE_PRESETS.md) · 规格文档（每个风格的 CSS 变量、字体、签名元素）
- [`styles/`](styles/) · 43 个 live HTML 实例，可直接打开查看
- [`style-gallery.html`](style-gallery.html) · 风格浏览器（5 列网格）

**部分风格举例：**
art-deco-gatsby · aurora-mesh · bauhaus-primary · blueprint-tech · chinese-ink-wash · cinema-scope · claude · claymorphism-3d · creative-voltage · dark-botanical · dark-premium · editorial-serif · futuristic-blue · grainy-retro · ink-wash · keynote-noir · korean-soft · memphis-pop · neon-brutalism · newsprint-broadsheet · nihon-modern · notion · pop-art · retro-arcade · risograph-overprint · soft-dreamy · split-pastel · surrealism-gallery · swiss-grid · terminal-green · vintage-editorial · wabi-sabi-zen · watercolor · 等 43 个

### 📐 24 结构模板

内容怎么排？一页是 bento-grid 还是 funnel 还是 hub-spoke？Agent 用 `.impeccable.md` 的 context 或内容语义自动匹配。

- [`STRUCTURE_PRESETS.md`](STRUCTURE_PRESETS.md) · 24 结构定义 + 坐标规范 + 推荐动画组合
- [`SCENARIO_TEMPLATES.md`](SCENARIO_TEMPLATES.md) · 场景结构 + 叙事弧 + 额外 slide 类型

**常用结构：**
bento-grid · hub-spoke · iceberg · funnel · timeline · comparison-matrix · linear-progression · tree-branching · radar-chart · structural-breakdown · winding-roadmap · circular-flow · story-mountain · story-arc · dense-modules · periodic-table · jigsaw · hierarchical-layers · kpi-wall · etc.

### 📽️ 116 场景实例

不想从零开始？直接打开一个场景 HTML，看完整的 live deck 效果，再改内容。

- [`scenarios/`](scenarios/) · 116 个场景 live HTML

**场景覆盖：**
- **融资 / Pitch**：pitch-deck · pitch-deck-orbital · pitch-deck-greenplate · pitch-deck-clarity（多种 mood 变体）
- **产品发布**：product-launch · product-launch-xiaomi · product-launch-dji · product-launch-clarity
- **答辩 / 学术**：thesis-defense · thesis-mba · thesis-medical
- **电商 / 促销**：ecommerce-1111 · ecommerce-beauty · ecommerce-product
- **企业 / 年终**：year-end-review · year-end-sales · year-end-engineering
- **职场**：job-promotion · job-promotion-cto · job-promotion-sales · job-promo-marketing
- **作品集**：portfolio-motion · portfolio-photo · portfolio-cinema · portfolio-botanical
- **技术 talk**：tech-talk · tech-talk-pipeline · tech-talk-agent
- **教学**：teaching-ux · teaching-python · teaching-pastel · teaching-notebook
- **国风品牌**：chinese-brand-wine · chinese-brand-fashion · chinese-brand-starfield

每个场景都有多个**风格 × 情绪**变体（如 `-swiss` / `-noir` / `-midnight` / `-bold` / `-cinema` 等后缀）。

### 🎬 140 GSAP 动画（三层架构）

```
┌─ Choreography 层 ─ animation-combos.md (10 个 timeline 模板)
├─ Discovery 层 ─── animation-index.json (140 效果 metadata)
└─ Code 层 ───────── animation-snippets.js (ID 索引函数库)
```

- [`animation-showcase.html`](animation-showcase.html) · 140 效果实时跑，13 分类 × 5 列网格
- [`animation-combos.md`](animation-combos.md) · 10 个 combo（hero-reveal / dashboard-awakening / pitch-impact / tech-demo / celebration / testimonial / ambient / focus 等）+ v2 perpetual-ii swap 菜单
- [`animation-index.json`](animation-index.json) · 140 效果 metadata（mood / purpose / speed / applicable_to / pattern_family）
- [`animation-snippets.js`](animation-snippets.js) · 140 ID 索引函数，perpetual 类带 `teardown` 句柄
- [`ANIMATION_PATTERNS.md`](ANIMATION_PATTERNS.md) · 15 核心 pattern 方法论 + 编排原则

## Agent 工作流

在 Claude Code 里说："帮我做个产品发布会 slide" / "做个投资人 pitch" / "准备答辩演讲"：

```
1. 读 .impeccable.md（项目设计 context）或快速问几个问题
   ↓
2. 选风格：从 STYLE_PRESETS 匹配或按 context 生成自定义
   ↓
3. 选结构 / 场景：STRUCTURE_PRESETS → SCENARIO_TEMPLATES
   ↓
4. 选动画：先查 combo，没匹配就查 index，拿 ID 调 snippets
   ↓
5. 可选：Phase 2.8 AI 背景图（Gemini / Imagen 生成 per-slide 概念艺术）
   ↓
6. 输出单文件 HTML，内嵌完整 CSS + JS，浏览器直接跑
```

## 快速安装

```bash
git clone https://github.com/Qihe-agent/next-slide-impeccable.git \
  ~/.claude/skills/next-slide-impeccable
```

Claude Code 会自动识别这个 skill。之后对它说：

- "做个 pitch deck" / "make a pitch deck"
- "做个产品发布会 slide"
- "准备答辩演讲"
- "给我做个数据 dashboard 页"
- "convert this PPT to HTML"

就能触发。

## 浏览现有内容

```bash
# 43 个风格
open style-gallery.html

# 140 个动画效果
open animation-showcase.html

# 任选一个场景 deck
open scenarios/pitch-deck.html
open scenarios/product-launch-xiaomi.html
open scenarios/chinese-brand-wine-ink.html
```

## 文件速查

### Skill 入口

| 文件 | 角色 |
|------|------|
| [`SKILL.md`](SKILL.md) | 主技能定义，Agent 入口 |

### 视觉风格 & 结构

| 文件 | 角色 |
|------|------|
| [`STYLE_PRESETS.md`](STYLE_PRESETS.md) | 50+ 风格规格 |
| [`styles/`](styles/) | 43 风格 live HTML |
| [`style-gallery.html`](style-gallery.html) | 风格索引浏览器 |
| [`gallery.html`](gallery.html) | 总 gallery（风格 + 场景） |
| [`STRUCTURE_PRESETS.md`](STRUCTURE_PRESETS.md) | 24 结构模板 + 坐标规范 |
| [`SCENARIO_TEMPLATES.md`](SCENARIO_TEMPLATES.md) | 场景结构、叙事弧、额外 slide 类型 |
| [`scenarios/`](scenarios/) | 116 场景 live 实例 |

### 动画子系统

| 文件 | 角色 |
|------|------|
| [`animation-combos.md`](animation-combos.md) | Choreography · 10 combo + v2 swap 菜单 |
| [`animation-index.json`](animation-index.json) | Discovery · 140 效果 metadata |
| [`animation-snippets.js`](animation-snippets.js) | Code · ID 索引函数库（带 teardown） |
| [`animation-showcase.html`](animation-showcase.html) | 视觉预览 · 140 效果实时跑 |
| [`ANIMATION_PATTERNS.md`](ANIMATION_PATTERNS.md) | 方法论 · 15 核心 pattern |

### HTML 生成 & 工具

| 文件 | 角色 |
|------|------|
| [`html-template.md`](html-template.md) | HTML 架构 + JS 特性规范 |
| [`viewport-base.css`](viewport-base.css) | 强制 responsive CSS（所有 slide 共享） |
| [`scripts/`](scripts/) | PPT 内容抽取工具 |
| [`landing/`](landing/) | Landing 页模板 |

### 品牌 & 设计哲学

| 文件 | 角色 |
|------|------|
| [`ASYRE_BRAND_PRESET.md`](ASYRE_BRAND_PRESET.md) | Asyre Dark Gold 品牌 + AI 图像 prompt 系统 |
| [`DESIGN_ELEVATION.md`](DESIGN_ELEVATION.md) | Impeccable 设计原则（提升预设到艺术品级） |
| [`ASHER_PREFERENCES.md`](ASHER_PREFERENCES.md) | 作者使用偏好（SVG 图标、字号、颜色） |

### 开发历史

| 文件 | 角色 |
|------|------|
| [`ANIMATION_UPGRADE_PLAN.md`](ANIMATION_UPGRADE_PLAN.md) | 动画子系统架构升级蓝图 |
| [`ANIMATION_REVIEW.md`](ANIMATION_REVIEW.md) | Track A 质量审查决策 |
| [`ANIMATION_GAPS.md`](ANIMATION_GAPS.md) | Track E 新效果设计 |
| [`ANIMATION_QA_NOTES.md`](ANIMATION_QA_NOTES.md) | Phase 5 代码质量修复记录 |

## Powered by

- **GSAP 3.12.5** · 动画引擎（单 `<script>` tag，~30KB gzipped）
- **零运行时依赖** · 纯 HTML + CSS + inline JS，无打包无构建
- **Impeccable 设计原则** · 字重/间距/对比/动效节奏的系统化质量标准
- **Asyre Dark Gold** · 品牌 + AI 背景图生成提示词系统

## 简史

Next Slide Impeccable 源自 [next-slide](https://github.com/Qihe-agent) 的重大升级。Phase 1-5 迭代：

1. **Phase 1** — 140 效果 live 素材库 + snippet 库（从 110 扩到 140）
2. **Phase 2** — 给 140 效果打 metadata（mood / purpose / applicable_to / pattern_family）
3. **Phase 3** — 设计 10 个 combo timeline 模板
4. **Phase 4** — 重写 SKILL.md §动画选取规则为三层入口架构
5. **Phase 5** — 16 个 perpetual 函数加 teardown + 代码质量修复 + 性能节流

详情见 [`ANIMATION_UPGRADE_PLAN.md`](ANIMATION_UPGRADE_PLAN.md)。

---

<div align="center">
Made with 💛 by <a href="https://github.com/Qihe-agent">Qihe-agent (Emergence Studio)</a>
</div>

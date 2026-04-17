<div align="center">

# Asyre Presentation

**你的下个 ppt，何必是 PPT**

Zero-dependency, animation-rich HTML 演示系统 — 50+ 精选视觉风格 · 140 个 live GSAP 效果 · AI 背景图生成 · 双语原生支持

</div>

---

## 这是什么

Next Slide Impeccable 是一个 Claude 技能（Skill），让 Agent 能根据你的内容直接生成浏览器里跑的、零依赖的高质量 HTML 演示文稿。

**和普通 PPT 的区别：**
- 纯 HTML + CSS + GSAP，浏览器打开就能看，无需任何软件
- 动画是叙事工具，不是装饰 — 内置 10 个 combo 模板（hero / dashboard / pitch / testimonial / tech-demo 等）
- 140 个精心设计的 GSAP 动画效果，按 mood / purpose / applicable_to 索引
- 双语原生（中英文字体栈内置 CJK fallback）
- 一键导出 PDF / 可分享链接

## 三层动画架构

```
┌─ Choreography 层 ─ animation-combos.md（10 个 timeline 模板）
├─ Discovery 层 ─── animation-index.json（140 效果 metadata 索引）
└─ Code 层 ───────── animation-snippets.js（ID 索引的 GSAP 函数）
```

Agent 查 combo 拿 timeline → 没匹配就查 index 按字段筛 → 拿到 ID 调 `effects.play_XX(scope)`。

## 文件速查

### Skill 入口

| 文件 | 角色 |
|------|------|
| [SKILL.md](SKILL.md) | 主技能定义，Agent 的入口 |

### 动画子系统（三层架构）

| 文件 | 角色 |
|------|------|
| [animation-combos.md](animation-combos.md) | Choreography · 10 个 combo timeline 模板 + v2 swap 菜单 |
| [animation-index.json](animation-index.json) | Discovery · 140 效果 metadata（mood/purpose/applicable_to/pattern_family） |
| [animation-snippets.js](animation-snippets.js) | Code · 140 ID 索引的 GSAP 函数（带 teardown） |
| [animation-showcase.html](animation-showcase.html) | 视觉预览 · 140 效果实时跑 |
| [ANIMATION_PATTERNS.md](ANIMATION_PATTERNS.md) | 方法论 · 15 核心 pattern + 编排原则 |

### 视觉风格 & 结构

| 文件 | 角色 |
|------|------|
| [STYLE_PRESETS.md](STYLE_PRESETS.md) | 50+ 精选视觉风格规格 |
| [styles/](styles/) | 43 个风格的 live HTML 实例 |
| [style-gallery.html](style-gallery.html) | 风格索引浏览器 |
| [gallery.html](gallery.html) | 总 gallery（风格 + 场景） |
| [STRUCTURE_PRESETS.md](STRUCTURE_PRESETS.md) | 24 个结构模板（bento-grid、funnel、hub-spoke 等） |
| [SCENARIO_TEMPLATES.md](SCENARIO_TEMPLATES.md) | 场景结构、叙事弧、额外 slide 类型 |
| [scenarios/](scenarios/) | 116 个场景的 live 实例（pitch-deck、ecommerce、thesis、tech-talk 等） |

### HTML 生成 & 工具

| 文件 | 角色 |
|------|------|
| [html-template.md](html-template.md) | HTML 架构 + JS 特性规范 |
| [viewport-base.css](viewport-base.css) | 强制 responsive CSS（所有 slide 共享） |
| [scripts/](scripts/) | PPT 内容抽取工具 |
| [landing/](landing/) | Landing 页模板 |

### 品牌 & 设计哲学

| 文件 | 角色 |
|------|------|
| [ASYRE_BRAND_PRESET.md](ASYRE_BRAND_PRESET.md) | Asyre Dark Gold 品牌风格 + AI 图像 prompt 系统 |
| [DESIGN_ELEVATION.md](DESIGN_ELEVATION.md) | Impeccable 设计原则：提升预设到艺术品级 |
| [ASHER_PREFERENCES.md](ASHER_PREFERENCES.md) | Asher 使用偏好（字号、颜色、SVG 图表规则） |

## 快速使用

浏览器里直接看 140 个效果：
```bash
open animation-showcase.html
```

按 ID 调用效果：
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
<script src="animation-snippets.js"></script>
<script>
  // 播放效果 101（MILESTONE TIMELINE）
  const handle = effects.play_101(document.querySelector('.my-slide'));
  // 切页时先清理
  handle?.teardown?.();
</script>
```

按 mood 查效果：
```bash
jq '.effects[] | select((.mood | index("elegant")) and .loop == "perpetual") | {id, name}' animation-index.json
```

## 作为 Claude 技能安装

把整个目录放进 Claude Code 的 skills 目录：

```bash
git clone https://github.com/Qihe-agent/next-slide-impeccable.git \
  ~/.claude/skills/next-slide-impeccable
```

然后在 Claude Code 里说"帮我做个演示 / create a presentation"，Agent 会自动使用这个技能。

## 开发历史

- [ANIMATION_UPGRADE_PLAN.md](ANIMATION_UPGRADE_PLAN.md) — 架构升级蓝图
- [ANIMATION_REVIEW.md](ANIMATION_REVIEW.md) — Track A 质量审查决策
- [ANIMATION_GAPS.md](ANIMATION_GAPS.md) — Track E 新效果设计
- [ANIMATION_QA_NOTES.md](ANIMATION_QA_NOTES.md) — Phase 5 代码质量修复记录

## Powered by

Next Slide engine · Impeccable design principles · GSAP 3.12 · 零运行时依赖

---

<div align="center">
Made with 💛 by <a href="https://github.com/Qihe-agent">Qihe-agent (Emergence Studio)</a>
</div>

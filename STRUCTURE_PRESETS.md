# Structure Presets — 结构模板库

> 最终输出 = 结构模板 × 风格主题。结构决定信息架构，风格决定视觉表现。

## 速查表

| # | 结构 | 中文名 | 适用场景 | 节点数 | 复杂度 |
|---|------|-------|---------|--------|--------|
| 1 | bento-grid | 格子仪表盘 | KPI 概览、部门对比 | 4-9 卡片 | ★★ |
| 2 | funnel | 漏斗 | 转化阶段、采用路径 | 4-6 层 | ★★ |
| 3 | hub-spoke | 中心辐射 | 核心→部门、平台→模块 | 1+4~6 节点 | ★★★ |
| 4 | iceberg | 冰山 | 可见 vs 隐藏、表面 vs 深层 | 2 区×3~7 项 | ★★★ |
| 5 | bridge | 转型之桥 | 现状→未来、Before/After | 左+桥+右 | ★★ |
| 6 | radar-chart | 雷达图 | 多维能力评估、产品对比 | 5-8 轴 | ★★★ |
| 7 | comparison-table | 对比表 | 产品/方案并排对比 | 2-3 列 | ★ |
| 8 | binary-comparison | 二元对比 | A vs B 直接对决 | 2 列 | ★ |
| 9 | comparison-matrix | 多维矩阵 | 多产品×多维度详细评测 | 5-8 列×8-12 行 | ★★★ |
| 10 | circular-flow | 循环流程 | 生命周期、闭环流程 | 4-6 节点 | ★★★ |
| 11 | hierarchical-layers | 层级堆叠 | 技术栈、协议栈、架构层 | 3-5 层 | ★★ |
| 12 | tree-branching | 树状分支 | 组织架构、决策树、分类 | 1+4~12 节点 | ★★★ |
| 13 | linear-progression | 线性进程 | 路线图、时间线、里程碑 | 4-7 节点 | ★★ |
| 14 | dashboard | 仪表盘 | 实时监控、运营看板 | 5-10 模块 | ★★★ |
| 15 | periodic-table | 周期表 | 工具/产品分类网格 | 15-30 元素 | ★★★ |
| 16 | venn-diagram | 维恩图 | 交叉关系、共同点 | 2-3 圈 | ★★ |
| 17 | jigsaw | 拼图 | 互补模块、组合关系 | 4-6 块 | ★★ |
| 18 | winding-roadmap | 蜿蜒路线 | 长期规划、旅程地图 | 5-8 节点 | ★★★ |
| 19 | story-mountain | 故事山 | 叙事弧线、冲突-高潮-解决 | 5 阶段 | ★★ |
| 20 | swot-analysis | SWOT | 优劣机威四象限 | 4 象限 | ★ |
| 21 | dense-modules | 密集模块 | 技术架构、系统蓝图 | 8-16 模块 | ★★★ |
| 22 | structural-breakdown | 结构拆解 | 产品拆解、系统架构 | 1 主体+6-10 子模块 | ★★★ |
| 23 | isometric-map | 等距地图 | 办公室/工厂/系统空间 | 空间+标注 | ★★★★ |
| 24 | comic-strip | 漫画格 | 用户旅程、故事讲述 | 4 格 | ★★ |

---

## 结构详解

### 1. bento-grid（格子仪表盘）

**布局：** 2×3 或 3×3 卡片网格，卡片可大小不等
**骨架坐标：** 1400×788 画布，三列（col 宽 440, 间距 12）
**数据接口：** 每卡需 title, value, description, bar_data, detail_lines
**动画编排：** stagger-reveal (网格涟漪) → counter → bar-growth → scan-line (循环)
**变体：**
- 等大 2×3：均匀信息分布
- 混合尺寸：1 大+4 小，突出重点
- 带 KPI 顶栏：顶部一排 mini 卡 + 下方详情卡

### 2. funnel（漏斗）

**布局：** 5 层梯形从宽到窄垂直堆叠，左侧百分比，右侧注释
**骨架坐标：** 梯形宽度从 1080px → 540px，间距 12px
**数据接口：** 每层需 name, percentage, description, detail_items[3]
**动画编排：** 从顶层向下 stagger-reveal → 百分比 counter → 连接虚线 fade in
**变体：**
- 正漏斗：转化率递减
- 倒漏斗：价值递增
- 双漏斗：左右对照

### 3. hub-spoke（中心辐射）

**布局：** 中心圆 + 6 节点均匀分布在周围，虚线连接
**骨架坐标：** 中心 (700, 410), 节点 radius 250, 每 60° 一个
**数据接口：** hub: {name, subtitle}, nodes[6]: {name, description, stat_value, stat_label}
**动画编排：** hub pulse-ripple 弹入 → 连接线 draw-on → 节点从各方向 stagger 弹入 → path-flow 粒子 → orbit-rotate (循环)
**变体：**
- 标准 6 节点
- 4 节点（十字布局）
- 不等距（按重要性调整半径）

### 4. iceberg（冰山）

**布局：** 上半水面上（小三角），下半水面下（大三角），水平线分隔
**骨架坐标：** 水线 y=280，上方 3-4 项，下方 5-7 项
**数据接口：** above[3-4]: {name, description}, below[5-7]: {name, description}, annotations
**动画编排：** 上方 tip draw-on → 内容 cascade → 水面 wave 出现 → 下方 body draw-on + parallax-depth → bubble-rise (循环)
**变体：**
- 标准冰山（20/80）
- 宽版（30/70）
- 带侧边注释

### 5. bridge（转型之桥）

**布局：** 左塔（现状）+ 中间桥弧 + 右塔（未来），对称结构
**骨架坐标：** 左塔 x=40~390, 右塔 x=1010~1360, 桥弧 y=300
**数据接口：** left[5]: {pain, value}, right[5]: {outcome, value}, bridge_steps[3]
**动画编排：** 左塔 stagger-reveal → 桥弧 draw-on + 步骤石 fade → 右塔 stagger-reveal
**变体：**
- 标准双塔
- 三列（现状 → 过渡 → 未来）
- 上下分（现状上半 → 桥 → 未来下半）

### 6. radar-chart（雷达图）

**布局：** 左侧雷达多边形 + 右侧评分面板
**骨架坐标：** 雷达中心 (580, 420) r=260, 6 轴; 评分面板 x=960~1380
**数据接口：** axes[5-8]: {name, label}, datasets[1-2]: {name, values[], color}
**动画编排：** 网格 draw-on → 轴标签 fade → 数据多边形 draw-on → 数据点 pop → 评分条 bar-growth → radar-sweep (循环, 可选)
**变体：**
- 单数据集（能力自评）
- 双数据集（对比）
- 三数据集（趋势）

### 7. comparison-table（对比表）

**布局：** 2-3 列并排，行标签在左
**数据接口：** columns[2-3]: {name, color}, rows[6-8]: {label, values[]}
**动画编排：** 列头 stagger → 行 stagger-reveal 从上到下

### 8. binary-comparison（二元对比）

**布局：** 画面等分左右，中间 VS 分隔
**数据接口：** left: {name, items[5]}, right: {name, items[5]}
**动画编排：** 左侧 slide-in-left → VS 标记 scale → 右侧 slide-in-right

### 9. comparison-matrix（多维矩阵）

**布局：** 大表格，5-8 列×8-12 行，表头固定
**数据接口：** tools[5-8]: {name, price}, dimensions[8-12]: {name, ratings[]}
**动画编排：** 表头 stagger → 行 stagger-reveal → 综合评分 counter

### 10. circular-flow（循环流程）

**布局：** 4-6 节点沿圆形排列，箭头连接。**节点信息框必须在圆外**，通过虚线连接器连到圆上标记点。当有右侧详情面板时，圆心必须左移至 cx≤400（推荐380），半径缩小至 r≤200（推荐190），面板与右侧节点间距≥50px。
**骨架坐标：** 无面板时圆心 (700, 410) r=220；有面板时圆心 (380, 410) r=190，面板 x=770+
**动画编排：** 节点沿圆 stagger 出现 → 连接箭头 draw-on → path-flow 粒子沿圆流动（rAF，多色3粒子）→ 节点边框呼吸。**不用扫描线**，用粒子轨道代替。不要旋转虚线圆环（看起来眩晕）。
**踩坑：** 最常见错误是右侧节点框和详情面板重叠。原因：圆心太靠右（cx=540+）时，右侧节点的 x 会超过面板起始位置。必须提前算好空间分配。

### 11. hierarchical-layers（层级堆叠）

**布局：** 3-5 层水平条从上到下堆叠（或三角金字塔）
**动画编排：** 从底层向上 stagger-reveal → 标注 draw-on

### 12. tree-branching（树状分支）

**布局：** 根节点在左/上，分支向右/下展开
**动画编排：** 根节点 pulse → 一级分支 draw-on + node pop → 二级分支 draw-on + node pop → path-flow 数据流

### 13. linear-progression（线性进程）

**布局：** 水平时间线，节点均匀分布
**骨架坐标：** 线 y=400, 节点从 x=100 到 x=1300
**动画编排：** 主线 draw-on (从左到右) → 节点沿线 stagger pop → 标注 fade

### 14. dashboard（仪表盘）

**布局：** 混合模块：KPI 顶栏 + 图表区 + 表格区
**动画编排：** scan-line sweep → KPI counter → 图表 bar-growth → 表格 stagger-reveal

### 15. periodic-table（周期表）

**布局：** 按类别颜色编码的网格卡片
**动画编排：** stagger-reveal (grid 涟漪) → 类别颜色 fade

### 16. venn-diagram（维恩图）

**布局：** 2-3 个重叠圆，交集区域标注
**动画编排：** 圆从各自方向 slide-in → 交集区域 highlight pulse

### 17. jigsaw（拼图）

**布局：** 3×2 网格（6块），每块 400×270。相邻边用精确镜像贝塞尔曲线互锁（tab 突出 30px，blank 缩进 30px）。**外围四边也要有波浪不规则边缘**（±10-15px 偏移），不能全是直线——否则不像拼图。每块不同颜色但和谐（muted red/gold/blue/green/purple/teal）。
**动画编排：** 各块从散落位置 slide+rotate 到位（elastic.out 弹性缓动） → 完成时 completion glow flash → 文字淡入 → counter → 永续：呼吸边框光晕 + 接口点脉冲（7个）+ 高亮扫光循环
**踩坑：** 凸凹接口不吻合是最常见的错误。tab 和 blank 的控制点必须严格对称（tab 在 E+30，blank 在 E-30，同一 mid 点）。用同一组参数生成两侧路径。

### 18. winding-roadmap（蜿蜒路线）

**布局：** S 形或 Z 形路径，节点沿途分布。**里程碑信息框必须在路径旁边而非路径上**——曲线顶部的里程碑框放在上方，底部的放在下方，通过虚线连接器关联。路径本身应清晰可见，不被任何框遮挡。
**动画编排：** 路径 draw-on → 节点沿路径 stagger pop → 当前位置 pulse → 光点沿路径旅行（rAF + getPointAtLength）→ 里程碑呼吸。**不用扫描线**。
**踩坑：** 信息框和 S 曲线重叠是常见错误。框的位置必须根据曲线的走向动态调整（弯道顶部向上偏移，弯道底部向下偏移）。

### 19. story-mountain（故事山）

**布局：** 弧形曲线占上半区（y=70~480），阶段标记在弧线上。**所有信息卡片放在弧线下方的独立区域（y=530+），用垂直虚线连接到弧线上的标记点。** 绝不在弧线穿过的位置放卡片。高潮点（峰值）用更大标记+glow强调。
**动画编排：** 弧线 draw-on (从左到右) → 阶段标签 stagger → 卡片 stagger-reveal → 高潮点 pulse-ripple + 光点沿弧线旅行（rAF）+ 峰值粒子上升
**踩坑：** 卡片和弧线重叠是最常见的错误。弧线区和卡片区必须完全分离。

### 20. swot-analysis（SWOT 分析）

**布局：** 2×2 象限，每象限不同颜色
**动画编排：** 四象限从中心 scale 展开 → 内容 stagger-reveal

### 21. dense-modules（密集模块）

**布局：** 类蓝图的密集信息块，多区域
**动画编排：** frame draw-on → scan-line → 区域 stagger-reveal → 连接线 draw-on

### 22. structural-breakdown（结构拆解）

**布局：** 中心块（200×128）在画布正中，8 个子模块径向排列（上3+左1+右1+下3）。中心块和子模块之间 ≥100px 间距。子模块卡片 240×108，内部文字 ≤14px，不堆叠。连接线从中心块边缘到 junction dot 再到卡片边缘。
**动画编排：** header → 中心 scale-in (back.out) → 连接线 draw-on → junction dots → 子模块 stagger pop → counter → 永续：脉冲环 + 扫描线 + 粒子沿连接线流动（rAF）
**踩坑：** 文字堆叠是常见问题——卡片内行间距至少 16px，计数器数字用 14px 而非 18px。

### 23. isometric-map（等距地图）

**布局：** 30° 等距透视的空间图
**动画编排：** 地面网格 draw-on → 建筑/设备从底部 scale-up → 标注 fade → path-flow 数据流

### 24. comic-strip（漫画格）

**布局：** 6 格（3×2），每格 420×330，带微旋转（±0.3~0.8°）增加漫画感。每格有时间戳标签、标题、SVG 几何图标、数据图表。**不使用对话框/speech bubble**——用图形+数据+action words (PANIC! FIX! SHIP IT! BOOM!) 讲故事。**不用抽象隐喻图形**（天平、齿轮等看不懂的图），信息用文字+数字清晰表达。
**动画编排：** header → 边框逐格 draw-on（strokeDashoffset）→ 内容淡入 → 永续：咖啡蒸汽/图表线循环/进度条/警报闪烁/灯泡呼吸 + 扫描线
**踩坑：** 对话框标注会让漫画格显得杂乱且AI感重，已验证移除后效果更好。

---

## 使用方式

当用户需要结构图时：

1. **识别内容类型** → 匹配最适合的结构
2. **读取结构详解** → 获取布局坐标、数据接口
3. **选择风格主题** → 应用 Dark Gold / Blueprint / 和風 等配色
4. **组合动画模式** → 按结构的推荐编排 + [ANIMATION_PATTERNS.md](ANIMATION_PATTERNS.md) 中的代码片段
5. **生成 SVG** → 1400×788 画布，GSAP 动画

### 内容匹配指南

| 用户说 | 推荐结构 |
|--------|---------|
| "给我一个概览/dashboard" | bento-grid 或 dashboard |
| "分析一下转化/采用" | funnel |
| "画一个架构/生态图" | hub-spoke 或 structural-breakdown |
| "深层 vs 表面" | iceberg |
| "现在 vs 将来" | bridge |
| "能力评估/对比" | radar-chart |
| "产品对比" | comparison-matrix |
| "流程/生命周期" | circular-flow 或 linear-progression |
| "分类/工具列表" | periodic-table |
| "路线图/计划" | winding-roadmap 或 linear-progression |
| "A vs B" | binary-comparison |
| "优劣分析" | swot-analysis |
| "用户旅程/故事" | comic-strip 或 story-mountain |
| "技术栈/层级" | hierarchical-layers |

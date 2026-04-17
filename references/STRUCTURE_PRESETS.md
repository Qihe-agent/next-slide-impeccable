# Structure Presets — 结构模板库

> 最终输出 = 结构模板 × 风格主题。结构决定信息架构，风格决定视觉表现。

> ## 🔴 MUST-READ: [structures/STRUCTURES_INDEX.md](../structures/STRUCTURES_INDEX.md)
>
> 24 个结构都有**已跑通、已 QA、已修完所有踩坑**的 HTML 参考文件，放在 `structures/NN-name.html`。
>
> 生成任何结构图前**必须**先打开对应参考文件照抄布局 / 动画 / 公式，只换内容。本文档里的硬规则 0-11 已经全部固化在参考代码里——不读参考直接从零写，就是在重新踩一遍 radar sweep 飞出 / brightness 变黑 / PEAK 方形光晕 / winner 错位 / hover 失效 等已知坑。
>
> 本文档是**抽象规则 + 踩坑清单**（为什么这么做），`structures/` 是**具体实现**（怎么做）。两者配合用。

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
**骨架坐标：** 中心 (700, 400)；**hub 半径 R 必须够大容纳内部文字**（见踩坑）；节点位于半径 230，每 60° 一个（角度 -90/-30/30/90/150/210°，即北 / NE / SE / 南 / SW / NW）。节点矩形 180×70 居中于节点坐标。
**数据接口：** hub: {name, subtitle, stat_line}, nodes[6]: {name, description, stat_value, stat_label}
**动画编排：** hub pulse-ripple 弹入 → 连接线 draw-on → 节点从各方向 stagger 弹入 → path-flow 粒子 → hub 外环 dashed rotate (perpetual, 40s)
**踩坑：**
- **Hub 半径与内部文字行数成正比** — hub 内放几行文字决定 R 下限：
  - 1 行（仅名字）：R ≥ 50
  - 2 行（名字 + 副标）：R ≥ 65
  - 3 行（名字 + 副标 + stat）：**R ≥ 80**（实测 R=60 放 3 行文字会贴圆边，非常挤）
  - 4+ 行：考虑换成更大的圆或改成椭圆
- R 变化时**必须同步 6 条连线的 hub-side 起点、6 颗 junction dot、ripple 起始 r、hubGlow / hubRingOuter 半径、JS 粒子 lines 数组**。漏改任何一个会导致连线穿透 hub 边界或和 hub 断开。
- 节点矩形 180×70 的连线端点用 trig 算 rect 入口（见全局硬规则 3），不是 rect 中心。
- hub 和 6 节点之间的区域（放射带）是粒子流动的空间，**不要在这个环形带里放任何 callout 文字**。

**变体：**
- 标准 6 节点
- 4 节点（十字布局）
- 不等距（按重要性调整半径）

### 4. iceberg（冰山）

**布局：** 左侧冰山（上下两部分）+ 右侧信息面板（上方 3 项 / 下方 7 项 split by waterline）。冰山和面板之间留 30-40px 空白。
**骨架坐标：** 水线 y=320；冰山 tip y=110-320, 冰山 body y=320-760，中心 x ≈ 350；右侧面板 x=620-1380
**数据接口：** above[3]: {name, description, tag}, below[7]: {name, description, metric, unit}, summary?: {ratio, text}
**动画编排：** 上方 tip draw-on → above cards cascade from right → waterline wave fade-in → body scale-up from waterline → below cards cascade → waterline 持续 path-morph（rAF + 8段 Q）→ bubbles 水下随机上升（DOM pool）
**踩坑：**
- **禁止在 waterline 起点 (y=320) 附近加金色装饰层**（如 "light shafts" 光柱）：waterline 本身是金色线，grid pattern 也是金色，再叠一层金色光柱会混成一团乱，显得"隐隐约约很丑"。要增加水下氛围深度，用 waterGrad 深度渐变 + 气泡 + 冰山倒影即可。
- **水下 grid pattern 必须被完全盖住** — 如果 waterGrad 顶部 stop opacity < 0.9，grid 的金色线条会透过深蓝水层渗出，产生"金蓝斑马"视觉。修复：waterGrad 起始 stop 用 opacity ≥ 0.9（例如 #0e2338 @ 0.92 → #040d18 @ 0.99）。
- 气泡 DOM pool 的 clip-path 必须精确限制在 y≥waterline，否则气泡会飞到天上。
- 冰山 body 要比 tip 大 3-4 倍（体现 20/80 隐喻），tip apex y 和 body 最低点 y 的距离 ≥ 500px。

**变体：**
- 标准冰山（20/80）
- 宽版（30/70）
- 带侧边注释

### 5. bridge（转型之桥）

**布局：** 左塔（现状）+ 中间桥弧 + 右塔（未来），对称结构。**桥上 3 颗步骤石的 callout 必须遵守"两上一下"规则**——两端的 step callout 放在石头上方，中间 peak step 的 callout 必须翻到石头下方。否则相邻 callout 在同一 y 行上会挤成一团。
**骨架坐标：** 左塔 x=30~330, 右塔 x=1070~1370, 桥弧 y=220~380（峰值 y=220）。石头位置 t=0.25/0.5/0.75 → (520,320) / (700,300 peak) / (880,320)
**数据接口：** left[5]: {pain, value}, right[5]: {outcome, value}, bridge_steps[3]: {name, period, side: "above"|"below"}
**动画编排：** 左塔 stagger-reveal → 桥弧 draw-on + 步骤石 back.out 弹入 → 右塔 stagger-reveal → 粒子沿弧 getPointAtLength 流动（perpetual）
**踩坑：**
- 中间 peak step 的 callout 若放上方，会和任何"装饰性方向 label"（如 "→ TRANSFORM" 箭头标签）重叠成乱码。解决：peak callout 改放下方，且**不要在桥顶加冗余方向文字**——方向感由桥渐变（左红 #b33a3a → 中金 #c4a35a → 右绿 #3daa5a）和粒子左→右流动已经传达。
- 两端 step callout 的 connector line 必须足够长（≥8px），文字离石头 ≥20px，否则挤在石头上沿。
- 桥弧渐变三色方案是固化的（red→gold→green），不要换成单色——视觉"转型"叙事靠这个渐变。

**变体：**
- 标准双塔
- 三列（现状 → 过渡 → 未来）
- 上下分（现状上半 → 桥 → 未来下半）

### 6. radar-chart（雷达图）

**布局：** 左侧雷达多边形 + 右侧评分面板。**当 panel 占据右半边（x≥740）时，title 不能居中在 canvas 正中**——必须左移对齐到 radar 中心。
**骨架坐标：** 雷达中心 (470, 430) r=200, 6 轴；右侧面板 x=760~1380；title/subtitle text-anchor="middle" 且 x=470（对齐 radar 中心）；titleLine x=250~690
**数据接口：** axes[5-8]: {name, label}, datasets[1-3]: {name, values[], color, highlight?}
**动画编排：** 网格 draw-on (从小到大) → 轴标签 fade → 数据多边形 stagger draw-on（highlight 数据集最后+最上层）→ 数据点 back.out pop → 右侧评分条 bar-growth → radar-sweep 8s 旋转（perpetual）+ highlight 数据点 pulse（perpetual）
**踩坑：**
- **Title 居中撞车**：canvas 宽 1400，居中 title (x=700) 的右边界通常在 x=820~860；右 panel 从 x=740~760 开始；两者 x 方向重叠 60~120px，title 尾字会被 panel card 的 border/fill 挡住。修复：title + subtitle + titleLine 整体锚到 radar 中心（x=470），让右半边 x≥740 完全留给 panel。这和 circular-flow 的"圆心左移避让右面板"是同一类规则的不同表达。
- 三数据集的多边形**必须分层绘制**，highlight 那一条（如用户主推的 model）放最上层 + 更粗 stroke + filter glow + 数据点 pulse，否则 3 条多边形重叠看不清谁赢谁输。
- 不要给 highlight 数据集以外的 polygon 也加 glow filter，会让画面"糊"成一团。
- 当数据集间差距小（±5 分以内），雷达图会高度重合——这时必须用右侧 bar breakdown 面板逐维度展示 + WIN 标记，光靠雷达辨识度不够。
- **Sweep/scan 动画必须沿最外层 polygon（hexagon/octagon）的边走，不要从圆心做径向旋转**：径向 sweep line 长度固定 r=200，在顶点处正好贴边，在边中点处（apothem ≈ r·cos(30°) ≈ 173）会刺出 hex 外——视觉上"大小不适配"。正解：用 `<polygon>` + `stroke-dasharray="<bright> <dark>"` 沿周长循环，每帧 rAF 走 `scanDist += speed`；`stroke-dashoffset` 让亮段跟着走；beam line 端点用参数化折线插值 `posAtPerim(scanDist)` 在六个顶点之间滑动，端点永远落在 hex 边上。

- **双 beam + 30° 扇形 + hex 亮段四者必须联动**——单独做一个 sweep line 看起来单薄，radar 感靠三层 + 一对 beam：

  | 层 | 元素 | 颜色 | 驱动 |
  |---|---|---|---|
  | Back · 扫过扇形 | `<path>` 动态 d | 蓝半透明 fill 22% + 虚线 stroke | 每帧 `M center L sample(i=0..N) Z`，采样点 = rayToHex(headAngle - 30°·i/N) |
  | Mid · hex 周长亮段 | `<polygon>` + stroke-dasharray 动态 | 蓝 #5dade2（与外圈金形成冷暖对比）| `dasharray="brightLen ${PERIM-brightLen}"` + `dashoffset=-tailDist` |
  | Front · head beam | `<line>` gold + glow filter | 金 #c4a35a | 终点 `posAtPerim(scanDist)` |
  | Front · tail beam | `<line>` 蓝虚线 dasharray | 蓝 #5dade2 | 终点 `rayToHex(headAngle - 30°)` |

  **关键约束**：tail beam 的端点**不能**用 `posAtPerim(scanDist - TRAIL_LEN)`（周长驱动），因为 hex 不是圆，同样的周长距离对应不同中心角（边中点快、顶点慢）。必须用 `rayToHex(tailAngle)`（中心角驱动），才能保证两根 beam 严格 30° 夹角；hexScan 的亮段长度 `brightLen = (scanDist - tailDist + PERIM) % PERIM` 动态计算（而不是固定 100/120），这样亮段两端正好接两根 beam 的端点。

- **rayToHex 的 Cramer's rule 符号陷阱**：用射线-线段相交公式解 C+t·d = v1+s·e：

  ```js
  const dxe = dx*ey - dy*ex;              // d × e (denominator)
  const t = (vx*ey - vy*ex) / dxe;        // V × e / (d × e) — ray param
  const s = (vx*dy - vy*dx) / dxe;        // V × d / (d × e) — edge param, must ∈ [0,1]
  ```

  **常见错误**：把 s 的分子写成 `(dx*vy - dy*vx)` 符号正好反了——导致所有合法交点被 reject（因为 s 变负），bug 表现为扇形"飞出 hex 外"（因为回退到 minT=Infinity 时的默认值，或者打到错误的边）。记忆口诀：**t 用 e 做外积、s 用 d 做外积，分母都是 `d × e`，分子里 V 永远在前**。

- **胜者 ★ 标记不要塞进短 label**：`"CLA ★"` 连字 9px 渲染后星号和字母糊成一团，用户完全看不到哪行赢了。正解：独立 `<tspan>`，font-size **16px** (约主标 2×)、font-weight 700、fill 和该模型 polygon 同色，排在 `WIN CLAUDE` / `WIN GEMINI` 全大写 letter-spacing=2 标签左边，形成醒目的"★ WIN XXX"胜者 badge。

**变体：**
- 单数据集（能力自评）
- 双数据集（对比）
- 三数据集（趋势 / 多产品对比，必须 highlight 一条）

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

## 通用布局硬规则（MANDATORY）

这些规则不属于单个结构，而是**跨结构通用**。生成任何一张 1400×788 结构图前必须过一遍。

### 硬规则 0 · Title × 右侧面板 的 x 方向避让

只要结构有"主图在左 + 右侧详情面板"（radar-chart / comparison-matrix / dashboard / iceberg / bridge-with-panel / 任何 panel x≥740 的布局），**title 不能居中在 canvas 正中 x=700**。

理由：居中 title 文字宽度常在 200-300px，右边界落在 x=800-860，会被右侧 panel 的 card border/fill 盖住（实测 radar 案例：title "AI 模型能力雷达评估" 右缘 x≈820，panel card 起点 x=760，重叠 60px，尾字完全被遮）。

修复：title + subtitle + titleLine 整体左移，锚到**主图的中心** x 值（text-anchor="middle" 保持）：
- radar 中心 x=470 → title x=470, titleLine x=250~690
- iceberg 中心 x=350 → title x=350
- hub-spoke / funnel / circular-flow 这类主图本来就居中且没右 panel，title 才放 x=700

### 硬规则 1 · Callout 的"两上一下"节奏

多个 callout（里程碑标签、石头标签、节点标签、阶段标签）**不能全部一侧**。相邻 callout 会在同一 y 行上挤成一团。

规则：
- **3 个 callout**：两端上方 / 中间下方（bridge 3 石头范本）
- **5 个 callout**：1、3、5 上方 / 2、4 下方（或按语义交替）
- **偶数个**：前半上 / 后半下 或按叙事节奏分

并且**相邻 callout 的 y 差 ≥ 30px**——即便同侧，也要错开。

### 硬规则 2 · 装饰性 label 不占 callout 的 y 行

不要在已有 callout 区域内再加**装饰性方向/阶段文字**（"→ TRANSFORM"、"PHASE 1"、"JOURNEY"、"NEXT STAGE" 等）——这些标签会和相邻 callout 的 y 坐标撞车，渲染成乱码（bridge 实测：`→ TRANSFORM` y=258 + `流程重构` y=262 叠成 `→ 流程重构 RM`）。

方向感 / 阶段感 通过**图形语言**表达：
- 方向 → 渐变色（左红中金右绿等）+ 粒子从一端流向另一端
- 阶段 → 颜色分段 + 节点大小差异（peak 节点更大）
- 重点 → filter glow + 脉动 + 更粗 stroke

### 硬规则 3 · 连线端点严守节点边缘

（引自 SKILL.md §节点连线硬规则）所有连线必须止于节点圆/矩形边缘，绝不穿过几何中心。
- **圆节点**：沿径向偏移 R，`(Cx ± R·cos(θ), Cy ± R·sin(θ))`
- **矩形节点**：计算线段和矩形四边的交点，取进入矩形的第一个交点为连线终点
- 斜线的收端用三角法：`nx=dx/L, ny=dy/L; ex=x2-nx·R2, ey=y2-ny·R2`

适用：NETWORK / TREE / HUB-SPOKE / ARC / PULSE / BRIDGE / 任何节点+连线的结构。

### 硬规则 4 · Stagger 方向与叙事方向一致

入场动画的 stagger 方向不能和结构的叙事方向矛盾：
- funnel / iceberg：从上往下 stagger
- timeline / linear-progression / bridge：从左往右 stagger
- hub-spoke：hub 先，nodes 从某一固定方向（北 → 顺时针）依次 stagger
- circular-flow：沿圆周 stagger（不是随机）
- story-mountain：按弧线 t=0 → t=1 stagger（重演故事时间轴）

### 硬规则 5 · SVG 旋转必须用 svgOrigin，不要用 transformOrigin

**症状**：GSAP 旋转 SVG 元素，旋转中心"飞到天上去"或落到 viewBox 外（实测 radar sweep line 绕 (940, 660) 旋转而不是预期的 (470, 430)）。

**两个独立但看起来相似的坑：**

#### 坑 A · 静态 transform 被 GSAP rotation 覆盖

```svg
<g transform="translate(470 430)"><line .../></g>  <!-- 静态 translate -->
```
```js
gsap.to(el, { rotation: 360, transformOrigin: '470px 430px' });
```
GSAP 写 rotation 时会**完全覆盖**元素的 transform 属性，静态 translate 丢失。

#### 坑 B · transformOrigin px 在 SVG 里是 bbox-relative，不是世界坐标

即便去掉 static translate，写：
```svg
<g id="sweepLine">
  <line x1="470" y1="430" x2="470" y2="230"/>  <!-- 绝对坐标 -->
</g>
```
```js
gsap.to('#sweepLine', { rotation: 360, transformOrigin: '470px 430px' });
```

GSAP 把 `'470px 430px'` 解释成**相对该元素 bbox 左上角**的偏移。line 的 bbox = (470, 230) 到 (470, 430)（退化的零宽 bbox），bbox 原点 (470, 230)。`'470px 430px'` 变成 (470+470, 230+430) = **(940, 660)** —— 离 (470, 430) 差了 470px 水平 + 230px 垂直。

#### 正确解：用 `svgOrigin`（专为 SVG 用户空间设计）

```js
gsap.to('#sweepLine', { rotation: 360, svgOrigin: '470 430' });  // 世界坐标，直接写绝对数
```

`svgOrigin` 是 GSAP 为 SVG 旋转专门提供的，值是 SVG user-space 坐标，不受 bbox 影响。**永远用它，不要在 SVG 元素上用 transformOrigin 的 px 值。**

唯一例外：`transformOrigin: '50% 50%'`（百分比是 bbox 中心）对对称元素（如 circle）是安全的。

#### 同类陷阱 · radialGradient 默认锚 bbox

`<radialGradient cx="50%" cy="50%">` 默认 `gradientUnits="objectBoundingBox"`，gradient 中心跟元素 bbox 走。想锚世界坐标（比如 radar 扇形始终从 (470, 430) emanate），必须：
```svg
<radialGradient id="sweepFan" cx="470" cy="430" r="200" gradientUnits="userSpaceOnUse">
```

#### 坑 C · GSAP `y: 0` / `x: 0` tween 直接覆盖 SVG 静态 translate

**症状**：SVG 元素用了 `<g transform="translate(X, Y)">` 做静态定位；GSAP 写 `tl.to(el, { opacity: 1, y: 0, duration: 0.5 })`（本意只是"淡入 + 就地停"）；**实际元素飞到 canvas 左上角 y=0 位置**，无论刷新多少次。

**原因**：GSAP 的 `x` / `y` 属性语义不是"元素位置"，而是 transform 的 translate 分量。`y: 0` 的 tween 是"把 translate-y 动画到 0"。如果静态 translate 的 Y ≠ 0，GSAP 最终写出的 `matrix(1,0,0,1,X,0)` 把 Y 强行改为 0，**完全覆盖**原静态 translate 的 Y 分量（X 保留是因为 GSAP 不 tween x 就不动）。

**实测**（radar score-card）：
- 静态 `transform="translate(760, 480)"` + `tl.to('.score-card', { opacity: 1, y: 0 })`
- 跑完 tween 后 DOM 变成 `matrix(1,0,0,1,760,0)` → 卡片渲染在画面顶部 y=0 区域
- 刷新无数次都是卡片在顶部的鬼魂（用户看到截图的"顶部 3 张卡挡住 title"现象根因在此）

**正确做法：**

1. **只想 fade-in**：**只 tween `opacity`，不要写 `y: 0` 或 `x: 0`**：
   ```js
   tl.to('.score-card', { opacity: 1, duration: 0.5 }, ...);  // ✅
   // ❌ 不要 tl.to('.score-card', { opacity: 1, y: 0, ... });
   ```

2. **想 slide-in**：把静态定位搬到**子元素的 x/y 属性**（rect/text 用绝对坐标），`<g>` 不要有静态 transform。这样 GSAP 随便 tween x/y 都不会把元素飞走：
   ```svg
   <g class="card">  <!-- 无 transform -->
     <rect x="760" y="480" width="200" height="80" .../>
     <text x="772" y="504" ...>...</text>
   </g>
   ```
   然后 `tl.fromTo('.card', { opacity: 0, y: 20 }, { opacity: 1, y: 0, ... })` 是从下方 20px 滑入到 y=0 偏移（相对 rect/text 固有位置），符合直觉。

3. **一次成型检查**：任何 GSAP tween 里写了 `x:` 或 `y:` 的，回头看目标元素有没有静态 `transform="translate(...)"`。**两者不能共存**。

### 硬规则 6 · 主信息块与 title 的 y 方向缓冲 ≥ 40px

Title 行底部（最低文字 y + descent，通常是 subtitle 或 titleLine 底边）到第一个 content block / card 顶部的垂直间距**必须 ≥ 40px**。

实测：
- title y=45 / subtitle y=68 / titleLine y=82 → 最下 y ≈ 86
- 第一个 card 顶 y=105 → 间距 19px → **太挤**，card 看起来"飘在 title 下面"
- 改为 card 顶 y=130 → 间距 44px → 视觉呼吸感正常

同理适用于任何右侧 panel / KPI strip / breakdown 区块。

### 硬规则 7 · KPI 卡内 number 与 label 的字号 + 间距约定

任何卡片里放"大数字 + 小标签"（"$480K / 月度成本"、"240h / 每次训练"、"88 / avg / 100"），遵守：

**字号**：
- 主数字 / value：font-size 20-36（根据 card 高度而定）
- 单位 / label：font-size 9-10（**不能 ≤ 8**，实测 font-size="8" 在 1400×788 canvas 渲染出来是一片糊糊看不清字符）

**垂直顺序**：number 在上（视觉焦点），label 在下（caption）。不要反过来。

**间距**：number 文字底部到 label 文字顶部**必须 ≥ 15px**。实测：
- number baseline y=418 + font-size 18 → 文字底约 y=422
- label baseline y=408 + font-size 8 → 文字顶约 y=402
- label y < number baseline → 两者垂直堆叠 overlap → 渲染成糊团（实测 iceberg 卡出现此 bug）

**正确坐标模式**（80px 高 card，card_top = Y）：
- number baseline: `Y + 32` (font 22)
- label baseline: `Y + 60` (font 9)
- 间距 28px，视觉舒展

### 硬规则 8 · 不要用 GSAP `filter: 'brightness(x)'` tween 做呼吸感

**症状**：想给一排 card / element 做"轻微亮度脉动呼吸感"，写：

```js
gsap.to('.card', { filter: 'brightness(1.05)', duration: 3.4, repeat: -1, yoyo: true, stagger: 0.2 });
```

跑几秒后所有 card **全变成近黑**，只剩 0.06—0.26 的 brightness 值（查 DOM：`filter: brightness(0.1863)`），内部文字几乎看不见。

**原因**：元素初始 `filter` 为 `none`（没有 numeric baseline）。GSAP tween `filter` 时需要一个起始数值，`none` 解析成 0，于是它 tween 的是 `brightness(0) → brightness(1.05) → brightness(0) → ...`。加上 `stagger: 0.2` 让每张 card 相位不同，在任意时刻 screenshot 一截，就能看到 card 停在 brightness(0.06)、brightness(0.19)、brightness(0.27) 等"快全黑"的状态——用户看到的是：**"card 没渲染 / 文字全没了"**。

实测影响文件：v2-impeccable 16-story-mountain 的 `.card:not(.climax-card)`，19-periodic-table 的 `.eb`。两处都是"想做呼吸感"写成 brightness 动画，第一次 QA 都被误判为"文字丢失"。

**正确做法（按优先级）：**

1. **改用 `strokeOpacity` 呼吸**（最推荐——语义直接是"边框微亮"）：
   ```js
   gsap.to('.card rect:first-child', {
     strokeOpacity: 0.95,       // from 0.6 → 0.95
     duration: 3.4, repeat: -1, yoyo: true, stagger: 0.2
   });
   ```

2. **改用 `fillOpacity` 呼吸**（适合 bg 层微亮）：
   ```js
   gsap.to('.card rect:first-child', { fillOpacity: 0.95, ... });
   ```

3. **如果非要 filter brightness，先 `gsap.set()` 初始化 baseline**：
   ```js
   gsap.set('.card', { filter: 'brightness(1)' });   // ← 必须
   gsap.to('.card', { filter: 'brightness(1.08)', duration: 3.4, repeat: -1, yoyo: true });
   ```
   但 filter brightness 有额外成本（强制 composite 层），以及 Safari 表现不一致，能不用就不用。

4. **完全不做呼吸**也比坏呼吸好——静态卡片 + 连接线上的流动粒子 + 角落 ringInner 旋转已经给画面足够动感。"card 呼吸"属于锦上添花，一旦跑挂整版全黑。

**快速 QA**：Card 渲染后用 DOM 检查 `computedFilter`——如果出现 `brightness(<1)`，立刻说明 tween 初值没设。这是 brightness tween 独有的失败模式，其他属性（opacity / strokeOpacity / scale）不会有这个问题。

### 硬规则 9 · SVG filter 必须显式设 filter region，否则 glow 被截成方块

**症状**：用 `feGaussianBlur stdDeviation=6` 给一个圆形节点（或任何小元素）加光晕，期望得到**圆形辐射光**，实际得到**方形模糊方块**——用户原话"这光晕应该是圆的，而不是方的"。

**根因**：SVG `<filter>` 元素的默认 region 是 `x=-10% y=-10% width=120% height=120%`——只比元素 bbox 大 10%。对于 `stdDeviation=3` 以下的小模糊基本够用；但 `stdDeviation ≥ 4` 时，blur 的衰减半径 ≈ 3 × stdDeviation = 12-18px，远超 10% buffer，模糊被 filter region 的矩形边界截断，肉眼看到就是"圆节点外有一圈方形暗光晕"。

实测影响：v2-impeccable 16-story-mountain 的 climax marker（stdDeviation=6 softGlow），首次 QA 用户直接指出"光晕是方的"。

**固定写法**（按 stdDeviation 分级）：

```svg
<!-- 轻度 glow (stdDev ≤ 3) -->
<filter id="glow" x="-50%" y="-50%" width="200%" height="200%">
  <feGaussianBlur stdDeviation="3"/>...
</filter>

<!-- 重度 soft glow (stdDev 4-8) -->
<filter id="softGlow" x="-75%" y="-75%" width="250%" height="250%">
  <feGaussianBlur stdDeviation="6"/>...
</filter>
```

**经验值**：region 要预留 `3 × stdDeviation` px 的 buffer，折算成百分比：
- stdDev 2 → region -40%/180%（或直接用 -50%/200% 最保险）
- stdDev 4 → region -60%/220%
- stdDev 6 → region -75%/250%
- stdDev 8+ → region -100%/300%

**通用保险值**：所有 `<filter>` 都加 `x="-50%" y="-50%" width="200%" height="200%"` 当 baseline，重度模糊再放大。批量改不会误伤（region 太大只是多占一点 composite 缓存，没有视觉 artifact）。

### 硬规则 10 · 列对齐高亮框的 x/w 必须和列定义一致

**症状**：3-column 对比表 cycling highlight 某列某行的 winner 时，高亮框比列窄或偏移，视觉看起来"歪了一截"或"没盖满"。

**根因**：列边界定义在一处（column rect），winner highlight rect 定义在另一处。第一处改了第二处忘改，或者手写时打错数字，就会出现 column rect x=1000 w=360、winner highlight x=1020 w=340 这种"差 20px"错位。

实测：v2-impeccable 20-comparison-table，Gemini 列是 x=1000-1360（w=360），winner highlight 手写成 `{ x: 1020, w: 340 }`，视觉看起来框比 card 内凹 20px 空了一条竖缝。

**正解**：把列定义提到 JS 常量，winner 坐标从常量派生：

```js
const COLS = {
  label:  { x: 40,   w: 240 },
  claude: { x: 280,  w: 360 },
  gpt:    { x: 640,  w: 360 },
  gemini: { x: 1000, w: 360 },
};
// highlight 直接用：
{ ...COLS.gemini, y: 220, color: '#5dade2' }
```

**快速 QA**：切到某列 winner 高亮的瞬间 screenshot，检查高亮框左右边和该列 column header 左右边**严格对齐**——差 ≥ 5px 就是 bug。

### 硬规则 11 · GSAP 永续 tween 会干掉 CSS `:hover` — 需要 `!important`

**症状**：做 gallery / 索引页，给卡片写 `a:hover .card { opacity: 1 }` 想让鼠标悬浮时某元素变亮。**页面刚加载时 hover 正常**，但几秒后（某个永续 GSAP tween 启动后）**hover 彻底失效**——鼠标移上去没反应。

**根因**：永续 `gsap.to(target, { opacity: 0.2, repeat: -1, yoyo: true })` 把 opacity 写进目标元素的 **inline style**（如 `style="opacity: 0.1942"`）。CSS 优先级链：

```
inline style > id selector > class selector/:hover > 标签 selector > SVG 属性
```

普通的 `.card:hover { opacity: 1 }` 是 class-level + :hover，**打不过 inline style**。tween 跑起来后 hover 就变成了一个无效规则。

实测：v2-impeccable 25-index.html 的 `.card-num` ghost 数字，`delay: 3.2` 后 GSAP tween 启动，hover 立刻失效。

**修复（优先级从高到低）**：

1. **在 hover 规则上加 `!important`**（最省事，推荐）：
   ```css
   a:hover .card-num { opacity: 1 !important; }
   a:hover .card-bg  { fill-opacity: 0.95 !important; stroke-width: 1.6 !important; }
   ```
   `!important` 把 CSS 规则提升到 inline style 之上，只要 inline 自己没写 `!important` 就会赢。

2. **把 hover 状态也用 GSAP 管理**（JS 接管）：
   ```js
   el.addEventListener('mouseenter', () => gsap.to(el, { opacity: 1, duration: 0.3, overwrite: 'auto' }));
   el.addEventListener('mouseleave', () => gsap.to(el, { opacity: 0.2, duration: 0.3, overwrite: 'auto' }));
   ```
   这样 hover 也走 inline style 路线，不需要 `!important`。适合需要更复杂 hover 动效的情况。

3. **避免 tween 冲突属性**：GSAP breath 改用 `strokeOpacity` 或 `fillOpacity`，hover 改另一个属性（如 `transform: scale`）。只要两者不碰同一个属性就互不干扰。

**快速 QA**：写完 hover 效果后，**等 5 秒**再试鼠标悬浮。如果一开始有效、后面失效，99% 是 GSAP inline style 覆盖，加 `!important` 解决。

**同类陷阱**：`.card-bg` 的 hover `fill-opacity` / `stroke-width` 如果永续动画里也动了这些属性（哪怕是不同 target），记得一并加 `!important`；否则部分 hover 属性生效、部分失效，看起来像 hover 半工作。

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

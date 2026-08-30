# 构图详表 17–32：分区、网格、图文与密度

以下详细提示词中的色彩、字体、材质、电影感及平面／编辑风格词，是为忠实保留源文而收录的示例；只有用户或上游提供相应风格时才携带，否则只保留构图结构并将风格字段参数化或省略，不得自行选择外部 style atom。

本表用于多类信息共存、精确网格、图文主次、连续画幅和实验构图。图像模型只能把网格当作结构趋势；HTML/CSS 应把栏数、行数、跨度、沟槽、对齐线和裁切焦点直接编码。

表中的 `3:4`、`4×6`、`5 栏`、`62/38`、具体沟槽和跨度都是可调的起始示例，不是不可变规范。按用户给定画幅、内容量、最小字号、最小单元宽度、裁切安全和媒介约束调整，并在交付中说明实际采用的值。

## 17｜上下分屏 Horizontal Split

用水平分界把图像与文字或两个场景叠放，天然形成先后和上下文关系。

- 视线路径：通常从上方主视觉下落到下方解释，也可反向用文字引出图像。
- 适合：电影、旅行、讲座、产品说明、数据 + 图像。
- 避坑：分界不要卡在主体脸部或文字中线；上下比例体现主次。
- 标签：`上下分屏` `地平线分割` `图文两层` `horizontal split` `stacked panels` `image-text division`

图像模型提示：

```text
3:4 竖版海报，使用上下分屏构图：上方约 62% 为主视觉[对象/场景]，下方 38% 为纯色信息区，承载标题“[主标题]”、日期和说明；分界线清晰并与图像地平线或结构线呼应。避免分界切过关键主体。
Keywords: horizontal split composition, stacked panels, image-text division, 62/38 layout.
```

HTML/CSS 提示：

```text
容器 display:grid，grid-template-rows:62% 38%；图像区 overflow:hidden + object-fit:cover；信息区用内部 2 列网格；标题可轻微跨越分界但须有对比底。
```

## 18｜水平带状 Horizontal Bands

用多个横向色带或内容带分层，产生节拍、章节感与横向速度。

- 视线路径：从最强色带开始，按带宽、颜色或编号逐层移动。
- 适合：音乐、交通、体育、时间表、系列活动、信息密集海报。
- 避坑：每条带不能同等抢眼；避免把段落挤成过窄行长。
- 标签：`水平色带` `分层条带` `章节节奏` `horizontal bands` `striped layout` `stacked information bars`

图像模型提示：

```text
竖版海报由 4–6 条宽度不等的水平内容带构成；主标题“[主标题]”占最宽且对比最强的一带，主视觉[对象]跨越相邻两带，日期、地点、编号分布在较窄带中；形成节拍与横向速度。避免所有条带等宽等强。
Keywords: horizontal band composition, striped layout, stacked information bars, rhythmic widths.
```

HTML/CSS 提示：

```text
使用 grid-template-rows:10% 24% 12% 36% 18% 等不等比例；每一带设置独立背景与对齐；主视觉通过 grid-row 跨带；文本行高随带高用 clamp() 调整。
```

## 19｜单栏手稿网格 Single-Column Grid

所有信息落在一个连续主栏中，靠边距、段落间距与字号建立清晰阅读秩序。

- 视线路径：从栏顶到栏底线性阅读，把内容当成一篇短文本。
- 适合：宣言、文学、学术、演讲、极简产品说明。
- 避坑：海报不是论文页；行长、段落数和字号必须适合远读。
- 标签：`单栏网格` `线性阅读` `大边距` `single-column grid` `manuscript grid` `linear reading`

图像模型提示：

```text
竖版文字海报，使用单栏手稿网格：一条窄而明确的主栏贯穿页面，标题“[主标题]”在栏顶占 2–4 行，短正文与日期沿同一左边线向下排列；四周大边距、基线节奏一致、安静理性。避免长段小字和过宽行长。
Keywords: single-column grid, manuscript grid, linear reading, generous margins.
```

HTML/CSS 提示：

```text
海报内部 max-inline-size:62%，margin-inline:auto；display:flex; flex-direction:column；所有文本共用左边线；正文 max-width:28ch，使用固定 line-height 建立基线节奏。
```

## 20｜多栏编辑网格 Multi-Column Grid

多条纵向栏提供灵活跨度，图像与文本可占一栏或跨栏，适合复杂信息层级。

- 视线路径：大跨栏元素先定级，再按栏线读取细分信息。
- 适合：编辑海报、会议、课程表、展览目录、数据与说明。
- 避坑：不要让所有元素占不同的随机跨度；重复栏宽和沟槽。
- 标签：`多栏网格` `跨栏` `编辑布局` `multi-column grid` `column span` `editorial layout`

图像模型提示：

```text
3:4 竖版编辑海报，建立 4 栏等宽网格与清晰沟槽；标题“[主标题]”跨 3 栏，主视觉[对象]跨 2–4 栏，日期和说明分别占 1 栏；通过跨栏数量建立层级，左对齐、节奏严谨。避免每个元素随机跨栏。
Keywords: multi-column grid, editorial layout, column span, consistent gutters.
```

HTML/CSS 提示：

```text
display:grid; grid-template-columns:repeat(4,1fr); column-gap:clamp(8px,2vw,20px)；标题 grid-column:1/4，主视觉可用 2/5，元信息各占 1 栏；统一使用网格线编号。
```

## 21｜模块网格 Modular Grid

纵栏与横行交叉成可复用模块，图像、文字和编号按模块组合，适合高密度系统化信息。

- 视线路径：先看跨模块的大元素，再按模块群组扫描局部信息。
- 适合：瑞士风格、信息设计、活动日程、展览、系列视觉。
- 避坑：网格是底层关系，不必把每条线都画出来。
- 标签：`模块网格` `矩阵` `跨模块` `modular grid` `matrix layout` `grid modules`

图像模型提示：

```text
竖版信息海报，使用 4 列 × 6 行模块网格；标题“[主标题]”跨 3×2 模块，主视觉[对象]占 2×3 模块，编号和详情填入较小模块；重复边距与沟槽，系统感强，可局部越界形成层级。不要显示全部辅助线。
Keywords: modular grid, matrix layout, spanning modules, systematic hierarchy.
```

HTML/CSS 提示：

```text
display:grid; grid-template-columns:repeat(4,1fr); grid-template-rows:repeat(6,1fr); gap:10px；用 grid-area 明确每个内容块；调试态显示网格，发布态仅保留少量分隔线。
```

## 22｜瑞士非对称网格 Swiss Grid

严格网格、左对齐无衬线字、非对称留白与客观摄影共同形成清晰理性的国际主义语气。

- 视线路径：大字号无衬线标题进入，沿左对齐线和模块关系读取。
- 适合：设计展、建筑、音乐会、文化机构、理性科技品牌。
- 避坑：加红圆和 Helvetica 不等于瑞士风格；关键是结构与层级。
- 标签：`瑞士风格` `左齐右不齐` `客观排印` `Swiss style` `International Typographic Style` `flush left ragged right`

图像模型提示：

```text
竖版文化海报，采用瑞士国际主义非对称网格：无衬线标题“[主标题]”左齐右不齐，严格对齐到 4–6 栏网格；一个高对比几何形或客观主视觉偏置放置，红/黑/米白克制配色，信息清晰、功能优先。避免居中排满、避免把装饰当结构。
Keywords: Swiss grid, International Typographic Style, flush left ragged right, asymmetric grid.
```

HTML/CSS 提示：

```text
建立 6 栏网格；所有文本 text-align:left，主标题跨 4 栏，元信息占 2 栏；用单一强调色与 1px 规则线；字体使用系统无衬线并限制字重数量。
```

## 23｜层级卡片网格 Bento / Hierarchical Grid

不同尺寸的矩形区块共享统一间距与圆角，通过卡片面积表达信息优先级。

- 视线路径：最大卡片先读，随后按相邻关系在小卡片间跳转。
- 适合：科技、产品功能、数据、服务组合、HTML 落地页。
- 避坑：不是随意拼方块；同层卡片需要一致语义和间距。
- 标签：`Bento 网格` `层级卡片` `不等模块` `bento grid` `hierarchical cards` `variable-span grid`

图像模型提示：

```text
竖版科技海报，使用 Bento 层级卡片网格：最大卡片承载主标题“[主标题]”与主视觉[对象]，3–5 个较小卡片承载卖点、数字、日期和 CTA；卡片跨度不等但沟槽一致，信息像仪表盘一样清晰。避免十几个同权小格。
Keywords: bento grid, hierarchical cards, variable-span modules, dashboard composition.
```

HTML/CSS 提示：

```text
display:grid; grid-template-columns:repeat(4,1fr); grid-auto-rows:1fr；主卡 grid-column:span 3; grid-row:span 3，副卡使用 span 1/2；统一 gap、border-radius 与内边距 token。
```

## 24｜棋盘交替 Checkerboard

图像与文字在规则矩阵中交替出现，利用反转、重复和对应关系形成节奏。

- 视线路径：在深浅或图文单元间跳读，整体先于局部被感知。
- 适合：系列产品、作品集、活动阵容、对照概念、时尚。
- 避坑：矩阵过细会像社交媒体九宫格；需要一个破格焦点。
- 标签：`棋盘` `图文交替` `反转节奏` `checkerboard layout` `alternating tiles` `image-text rhythm`

图像模型提示：

```text
竖版海报使用 3×4 棋盘式模块，图像、色块与文字交替出现；标题“[主标题]”占据或打破其中 2–3 个相邻格，其他格维持深浅反转与重复节奏；整齐但有一个明确破格焦点。避免所有格同等复杂。
Keywords: checkerboard composition, alternating tiles, image-text rhythm, one deliberate break.
```

HTML/CSS 提示：

```text
使用 3 列 × 4 行网格；通过 :nth-child(even) 反转前景/背景；主标题块 grid-column:span 2。小屏仅在每格满足最小单元宽度、最小字号和可读行长时保留 3 列；否则减少列数或顺序重排，同时保留图文交替、对应关系、语义 DOM 顺序和主焦点。
```

## 25｜满版主视觉 Full-Bleed Hero

单张图像铺满画布，文字叠加或放入局部安全区，以情绪与沉浸感优先。

- 视线路径：先感受整体图像，再由高对比标题进入具体信息。
- 适合：电影、旅行、时尚、摄影、产品氛围、人物。
- 避坑：文字不能压在复杂纹理上；裁切焦点需要跨尺寸管理。
- 标签：`满版图像` `沉浸主视觉` `文字叠图` `full-bleed image` `hero image` `type overlay`

图像模型提示：

```text
3:4 竖版海报，一张高质感主视觉[场景/对象]满版铺设并延伸到四边；标题“[主标题]”放在图像中低纹理、高对比的安全区域，辅文控制在一个紧凑信息簇；电影感、沉浸、裁切明确。避免文字压在主体脸部或杂乱纹理上。
Keywords: full-bleed hero image, immersive poster, type overlay, controlled crop.
```

HTML/CSS 提示：

```text
容器内 img 使用 position:absolute; inset:0; width:100%; height:100%; object-fit:cover；用 object-position 自定义焦点；增加局部渐变遮罩而非整图变暗；文字置于安全区并设置 max-width。
```

## 26｜大字主导 Typographic Dominant

把标题本身当作主视觉，靠尺度、字重、字距、断行和裁切建立图像感。

- 视线路径：先识别巨型关键词，再在字形缝隙或边缘找到细节信息。
- 适合：观点、宣言、音乐、促销、社会议题、无图素材。
- 避坑：大字不等于全部加粗；断行必须保留语义与辨识度。
- 标签：`大字海报` `字体主视觉` `超尺度标题` `big type poster` `typographic dominant` `oversized headline`

图像模型提示：

```text
竖版纯文字海报，让标题“[主标题]”成为唯一主视觉，使用超大无衬线或展示字体，占画面 60%–85%，允许部分字形越界裁切；副标题、日期以极小字号贴合字形空隙；高对比、强节奏。避免随机断词、避免过多字体。
Keywords: big type poster, typographic dominant composition, oversized headline, cropped letterforms.
```

HTML/CSS 提示：

```text
标题使用 font-size:clamp(72px,24vw,260px)、line-height:.78、letter-spacing:-.07em；容器 overflow:hidden；用 max-inline-size 与手动 <br> 控制断行，元信息绝对定位到负空间。
```

## 27｜图上叠字 Type over Image

文字与图像共享同一空间，通过遮挡、穿插和对比建立一体化主视觉。

- 视线路径：图像与标题几乎同时进入，再通过前后层级辨认关系。
- 适合：时尚、人像、杂志、电影、音乐、文化活动。
- 避坑：明确字在前、字在后或字穿插；半透明一盖了之通常发灰。
- 标签：`图上叠字` `图文穿插` `前后层级` `type over image` `interleaved typography` `foreground-background layering`

图像模型提示：

```text
竖版海报，主视觉[人物/对象]占据中央，超大标题“[主标题]”跨越图像；让部分字在主体前、部分字被主体遮挡，形成明确的图文穿插与空间层级；文字仍可读，边缘干净。避免整行半透明文字、避免遮住脸和关键信息。
Keywords: type over image, interleaved typography, foreground-background layering, editorial poster.
```

HTML/CSS 提示：

```text
建立背景文字层、图像层、前景文字层三个 z-index；同一标题使用两个视觉副本，但只保留一个可访问副本，并给装饰副本 aria-hidden="true"；主体可用透明 PNG 或 clip-path，避免仅靠 opacity。
```

## 28｜双联／三联 Diptych / Triptych

把一个主题拆成两个或三个连续画幅，利用重复与变化表现时间、对比或多视角。

- 视线路径：从第一画幅横向或纵向移动，在相似结构中比较变化。
- 适合：叙事、前后、系列产品、人物组、电影、摄影展。
- 避坑：每格要有共同变量；三张无关图片并排不构成三联。
- 标签：`双联` `三联` `连续画幅` `diptych` `triptych` `sequential panels`

图像模型提示：

```text
竖版海报分成三个连续窄画幅，分别呈现[同一主题]的三个时刻/视角/状态；三格裁切、色调与视平线保持共同规则，标题“[主标题]”跨越三格或置于统一底栏；重复中有变化。避免三张无关图片简单拼接。
Keywords: triptych composition, sequential panels, repeated framing, variation within a system.
```

HTML/CSS 提示：

```text
使用 grid-template-columns:repeat(3,1fr) 与统一 gap；每格 img 使用相同 aspect-ratio、object-position 规则；标题可 grid-column:1/-1；用 data-state 标记三个状态。
```

## 29｜拼贴层叠 Overlapping Collage

图像、纸片、纹理和文字以遮挡与旋转叠加，制造手工、记忆与亚文化语气。

- 视线路径：从最高对比层进入，在遮挡边缘间跳读，依靠层级而非网格推进。
- 适合：音乐、青年文化、杂志、艺术、复古、手工品牌。
- 避坑：随机旋转不是拼贴；需要统一的材质、色彩或叙事线索。
- 标签：`拼贴` `层叠` `撕纸` `遮挡` `overlapping collage` `torn paper` `layered ephemera`

图像模型提示：

```text
竖版拼贴海报，用 4–7 层照片、撕纸、色块、印章与标题“[主标题]”相互遮挡，角度控制在少数几个方向；保持一个主图层最大、其他层辅助，统一纸张颗粒与有限配色。避免每层同等抢眼、避免无主题素材堆砌。
Keywords: overlapping collage, torn paper layers, controlled rotation, layered ephemera.
```

HTML/CSS 提示：

```text
容器 position:relative；每层 absolute + transform:translate/rotate，旋转角度限定为 -8deg/0deg/6deg；用 box-shadow、mix-blend-mode 谨慎模拟纸层；明确 z-index token。
```

## 30｜破格网格 Broken Grid

先建立可感知的网格，再让少数关键元素跨线、错位或越界，以破坏制造强调。受控破格通常由一个主破格元素或焦点簇承担；源文示例中的标题和主视觉可以是服务同一层级的两个相关破格。

- 视线路径：规则网格建立预期，破格元素因违背预期成为焦点。
- 适合：先锋文化、设计展、科技、实验编辑、作品集。
- 避坑：没有底层网格就没有破格；每个元素都错位只会混乱。
- 标签：`破格网格` `跨线` `错位` `控制性破坏` `broken grid` `intentional misalignment` `grid disruption`

图像模型提示：

```text
竖版实验海报，先建立清晰的 5 栏网格与重复对齐线，再让标题“[主标题]”跨出两条栏线、主视觉[对象]越过一个模块边界；两者形成一个相关焦点簇，其余信息严格归位，用少量控制性错位制造焦点。避免所有元素随机漂浮。
Keywords: broken grid, intentional misalignment, controlled disruption, visible underlying structure.
```

HTML/CSS 提示：

```text
底层使用 5 栏 Grid；约 80% 元素遵守 grid lines，主破格元素或相关焦点簇通过 grid-column 跨栏并 translateX(8%–15%)；避免全部使用 absolute；调试时显示网格覆盖层。
```

## 31｜重复节奏 Repetition & Pattern

重复同一形状、图像或文字建立节拍，再以一个差异项形成焦点。

- 视线路径：先感知整体模式，随后立刻捕捉破例元素。
- 适合：系列产品、群体、音乐、运动、数据、概念传播。
- 避坑：重复项过多且无变化会成为背景噪声；差异必须服务主题。
- 标签：`重复` `模式` `节奏` `破例焦点` `repetition` `pattern` `one anomaly` `rhythmic grid`

图像模型提示：

```text
竖版海报，以同一[形状/对象/词语]按规则阵列重复 12–30 次，保持相同间距与尺度；其中一个元素用颜色、方向或大小打破模式，并与标题“[主标题]”建立关系；整体节奏强、焦点明确。避免多个同时破例。
Keywords: repetition and pattern, rhythmic grid, one anomaly, pattern interruption.
```

HTML/CSS 提示：

```text
使用 CSS Grid + repeat(auto-fit,minmax()) 生成阵列；所有单元继承统一样式，仅用 .is-anomaly 改一个变量；标题与异常单元共享 accent color。
```

## 32｜密集全铺 Dense All-Over

高密度元素覆盖整个画面，通过整体纹理、局部对比与多尺度层级制造最大主义冲击。

- 视线路径：先感知整体密度，再由最大尺度或最强色差的元素抓住局部入口。
- 适合：节日、地下音乐、潮流、复古广告、阵容很多的活动。
- 避坑：密集不等于没有层级；标题、日期与 CTA 仍需至少一个清晰入口。
- 标签：`密集全铺` `最大主义` `多尺度` `纹理化信息` `dense all-over` `maximalist poster` `horror vacui` `multi-scale hierarchy`

图像模型提示：

```text
竖版最大主义海报，图形、纹理、短词和信息密集覆盖整个画面，但保留明确多尺度层级：超大标题“[主标题]”为入口，中型主视觉[对象]为第二层，小字与图案形成底层纹理；使用有限色盘统一复杂度。避免所有元素同尺寸、避免关键日期不可读。
Keywords: dense all-over composition, maximalist poster, multi-scale hierarchy, controlled visual density.
```

HTML/CSS 提示：

```text
使用多层 CSS Grid/absolute 混合：背景 pattern 层、信息矩阵层、超大标题层；限定 3 个字号级别与 4 色以内；为关键文字设置实色底或 outline，保证 WCAG 对比。
```

# 构图详表 01–16：焦点、平衡、几何与动势

以下详细提示词中的色彩、字体、材质、电影感及平面／编辑风格词，是为忠实保留源文而收录的示例；只有用户或上游提供相应风格时才携带，否则只保留构图结构并将风格字段参数化或省略，不得自行选择外部 style atom。

本表用于单一主角、强符号骨架、明确视线路径和双区域分割。每项都提供图像模型与 HTML/CSS 的结构表达；替换方括号内容，并把所选条目融合进完整任务提示词。

表中的 `3:4`、`5×6`、栏数、比例、偏移和具体尺寸都是可调的起始示例，不是不可变规范。按用户给定画幅、内容量、最小字号、最小单元宽度、裁切安全和媒介约束调整，并在交付中说明实际采用的值。

## 01｜中轴对称 Axial Symmetry

所有主要元素围绕一条中轴组织，左右视觉重量接近，产生庄重、稳定与仪式感。

- 视线路径：上方标题 → 中央主视觉 → 底部信息；沿中轴下行。
- 适合：品牌发布、展览、奢侈品、典礼、单一主角。
- 避坑：不要机械镜像每个小元素；信息很多时容易僵硬。
- 标签：`中轴` `镜像平衡` `中央焦点` `庄重` `axial symmetry` `centered composition` `mirrored balance`

图像模型提示：

```text
竖版 3:4 海报，主题为[主题]。
采用中轴对称构图：主标题“[主标题]”居上且居中，主视觉[对象]精确落在垂直中轴，副标题、日期与署名沿中线分层排列；左右视觉重量近似镜像，边距与留白均衡；高对比、清晰三层层级、平面设计。避免多焦点、随机偏移和无意义装饰。
Keywords: axial symmetry, centered composition, mirrored balance, central focal point.
```

HTML/CSS 提示：

```text
使用 aspect-ratio:3/4 的海报容器；display:grid，单列三行，place-items:center；用 ::before 绘制可隐藏的垂直中轴；标题、主视觉、元信息都以 inline-size 居中约束；左右 padding 相同。
```

## 02｜非对称平衡 Asymmetrical Balance

左右形状不同，但通过大小、明度、密度与距离交换视觉重量，形成有张力的稳定。

- 视线路径：大形体先进入，小而高对比的信息块在另一侧完成配重。
- 适合：文化海报、设计展、时尚、科技、当代品牌。
- 避坑：偏左不等于平衡；另一侧必须有足够对比或密度回应。
- 标签：`非对称` `视觉配重` `偏置焦点` `动态稳定` `asymmetrical balance` `visual counterweight` `offset focal point`

图像模型提示：

```text
竖版海报，主题[主题]，采用非对称平衡构图。将大尺度主视觉[对象]偏置于画面一侧，在对侧以更小但更高对比的标题“[主标题]”和信息组完成视觉配重；对齐线清晰、留白不等量但视觉重量稳定，现代编辑设计感。避免随意散落、两侧同等抢眼。
Keywords: asymmetrical balance, visual counterweight, offset focal point, dynamic equilibrium.
```

HTML/CSS 提示：

```text
使用 5×6 隐形 CSS Grid；主视觉跨 3 列 4 行，文本块占对角侧 2 列；通过 clamp() 控制尺度差，使用 align-self:end 与 justify-self:start 制造偏置；保留一条共同对齐线。
```

## 03｜三分法焦点 Rule of Thirds

把画面横竖各分三份，将焦点放在分割线或交点附近，兼顾稳定与自然偏移。

- 视线路径：先到三分交点的主视觉，再沿分割线进入标题和信息。
- 适合：摄影主导海报、人物、产品、活动、网页 Hero。
- 避坑：不要为卡交点牺牲主体朝向、文字空间或裁切。
- 标签：`三分法` `交点焦点` `偏心留白` `rule of thirds` `thirds intersection` `off-center subject`

图像模型提示：

```text
3:4 竖版海报，使用三分法构图。将主视觉[对象]的视觉中心放在右上三分交点附近，标题“[主标题]”沿左侧三分线对齐，保留与主体视线方向一致的大块留白；信息层级清楚、裁切有意图。避免把所有内容塞进交点、避免边距失衡。
Keywords: rule of thirds, thirds intersection, off-center focal point, directional negative space.
```

HTML/CSS 提示：

```text
容器使用 3 列 × 3 行 CSS Grid；主视觉以 grid-column:2/4、grid-row:1/3 定位，标题沿第 1 条纵向分割线对齐；调试时用 repeating-linear-gradient 显示三分线，完成后关闭。
```

## 04｜黄金螺旋 Golden Spiral

以逐级缩小的矩形与弧线组织焦点和节奏；把它作为比例草图，而不是必须服从的美学定律。

- 视线路径：从宽阔区域沿弧线逐渐收紧，最终落到内圈焦点。
- 适合：艺术、自然主题、电影、叙事型产品、概念海报。
- 避坑：不要把螺旋线贴在成品上；内容层级仍优先。
- 标签：`黄金螺旋` `比例递进` `收束焦点` `golden spiral` `Fibonacci layout` `progressive scale`

图像模型提示：

```text
竖版概念海报，主题[主题]，使用隐性的黄金螺旋组织画面：大面积主视觉从外圈进入，标题“[主标题]”与辅助元素按逐级缩小的比例沿弧形轨迹排列，最终焦点落在内圈的[关键对象]；节奏由宽到紧。不要显示数学辅助线，不要牺牲可读性。
Keywords: golden spiral composition, Fibonacci proportion, progressive scale, converging focal point.
```

HTML/CSS 提示：

```text
使用嵌套的绝对定位矩形，每一级尺寸约为前一级的 61.8%；用 border-radius 与伪元素暗示弧线，内容锚点放在最小矩形附近；把比例做成 CSS 自定义属性，勿硬编码为装饰线。
```

## 05｜大留白单点 Isolated Focal Point

用大面积主动留白隔离一个小而明确的焦点，让稀疏本身成为语气。

- 视线路径：被孤立度直接拉向单点，再读取附近的少量文字。
- 适合：高端品牌、艺术展、诗歌、香氛、安静的产品传播。
- 避坑：留白不是把内容缩小；焦点需要足够对比、质感或语义。
- 标签：`大留白` `孤立焦点` `低密度` `克制` `macro white space` `isolated focal point` `minimal composition`

图像模型提示：

```text
竖版极简海报，主题[主题]。保留 65%–80% 的主动留白，只设置一个小尺度但高辨识度的主视觉[对象]，位于下方或偏心位置；标题“[主标题]”靠近焦点，元信息极少、字号克制；安静、高级、精确。避免把留白填满、避免多个装饰点。
Keywords: macro white space, isolated focal point, low-density layout, restrained typography.
```

HTML/CSS 提示：

```text
使用单一相对定位容器；主视觉 inline-size:18%–28%，绝对定位在 60%–75% 高度；文本与主视觉组成一个小型内容簇，其余区域保持空；设置 generous padding 与 max-width。
```

## 06｜边缘锚定 Edge-Anchored

将主体推向画面边缘并允许裁切，通过屏外延伸感制造尺度与张力。

- 视线路径：从被裁切的大形体进入，沿其朝向或边缘移动到文字。
- 适合：时尚、人像、产品特写、社交媒体封面、强态度传播。
- 避坑：不要误裁脸、关节、文字或产品关键结构；必须留安全区。
- 标签：`边缘锚定` `越界裁切` `屏外延伸` `edge-anchored` `cropped subject` `off-canvas continuation`

图像模型提示：

```text
3:4 竖版海报，将主视觉[对象]锚定在右侧边缘并有意越界裁切，主体约占画面 55%–70%；标题“[主标题]”在相反边缘形成细长信息带，留出明确安全区；产生屏外延伸与大尺度感。避免误切关键部位、避免四边同时拥挤。
Keywords: edge-anchored composition, intentional crop, off-canvas continuation, edge tension.
```

HTML/CSS 提示：

```text
海报容器 overflow:hidden；主视觉 position:absolute，inline-size:70%，inset-inline-end:-16%；文字固定在相反侧安全区；响应式时用 object-position 控制裁切焦点。
```

## 07｜三角／金字塔 Triangular Hierarchy

元素构成稳定三角，顶点负责强调，底边负责承重，天然适合三层层级。

- 视线路径：顶点焦点 → 两侧下行 → 底边信息，或从底部汇聚到顶点。
- 适合：人物群像、电影、论坛、权威主题、层级明确的发布。
- 避坑：三角外的元素不能成为更强焦点，否则骨架会塌。
- 标签：`三角构图` `金字塔层级` `稳定底座` `triangular composition` `pyramid hierarchy` `stable base`

图像模型提示：

```text
竖版海报，主题[主题]，用稳定的三角/金字塔构图组织[主视觉或人物组]：最高点放置核心对象或标题“[主标题]”，两侧元素向下展开，底边承载日期与地点；三层尺度递减、重心低、整体庄重。避免三角外出现更强焦点。
Keywords: triangular composition, pyramid hierarchy, stable base, apex focal point.
```

HTML/CSS 提示：

```text
用 CSS Grid 3 列 4 行，核心元素置于中列第 1–2 行，次要元素位于左右第 3 行，元信息横跨底行；可用 clip-path:polygon() 绘制辅助三角形。
```

## 08｜倒三角悬置 Inverted Triangle

上宽下窄的倒三角把重量悬在上方，制造不稳定、压迫、速度或危险感。

- 视线路径：上方宽区域收束到下方尖点，像漏斗一样压向结论。
- 适合：先锋艺术、音乐、悬疑、警示、反传统文化活动。
- 避坑：不适合强调可靠与舒适的主题；底部文字避免被尖点挤压。
- 标签：`倒三角` `悬置重心` `漏斗收束` `inverted triangle` `unstable balance` `downward convergence`

图像模型提示：

```text
实验性竖版海报，采用倒三角构图：宽大的标题“[主标题]”与图形占据上半部，向下逐级收窄，最终尖点指向底部的[日期/CTA]；高重心、压迫、紧张、有下坠感。避免底部信息拥挤，避免使用安稳温和的品牌语气。
Keywords: inverted triangle composition, top-heavy balance, downward convergence, suspended tension.
```

HTML/CSS 提示：

```text
顶部内容区宽 100%，中段 66%，底部焦点 20%；用 grid-template-rows 与 justify-self:center 逐级收窄，背景图形使用 clip-path:polygon(0 0,100% 0,50% 100%)。
```

## 09｜同心环绕 Concentric

多个中心相同的环、框或形状反复包围焦点，强调聚焦、回声与层层深入。

- 视线路径：外圈先建立场域，向内层层收束到中心。
- 适合：音乐、科技、声波、社区、周年、抽象主题。
- 避坑：中心文字与环线错开；环太多会产生噪声或眩晕。
- 标签：`同心` `环绕焦点` `层层收束` `concentric composition` `nested rings` `central convergence`

图像模型提示：

```text
竖版海报，围绕中央[主视觉]使用 4–7 层同心圆或同心框，尺寸与线宽有节奏变化；标题“[主标题]”置于中心或切入其中一层，辅助信息沿外环分布；形成聚焦、声波、回声感。避免环线遮挡文字、避免所有环同样粗。
Keywords: concentric composition, nested rings, central convergence, rhythmic intervals.
```

HTML/CSS 提示：

```text
海报使用 place-items:center；创建多个绝对定位元素，inset 按等比递增，border-radius:50% 或矩形；用 CSS 变量控制每环尺寸、线宽与透明度；文字层 z-index 最高。
```

## 10｜框中框 Frame within Frame

利用边框、窗口或嵌套容器反复限定观看范围，形成展陈感和纵深。

- 视线路径：先识别外框，再穿过内框进入主体，最后读取框外注释。
- 适合：艺术展、建筑、档案、摄影、复古印刷、画册封面。
- 避坑：框线层级不能与文字层级竞争；内框太小会像缩略图。
- 标签：`框中框` `嵌套边界` `窗口焦点` `frame within frame` `nested borders` `contained focal area`

图像模型提示：

```text
竖版文化海报，使用框中框构图：外层细边框定义安全区，中层偏置窗口裁切主视觉[对象]，内层小框锁定标题“[主标题]”；框外保留编号和注释，形成档案与展陈感。避免所有边框同粗、避免边框压迫正文。
Keywords: frame within frame, nested borders, contained focal area, gallery layout.
```

HTML/CSS 提示：

```text
使用三层嵌套容器，每层独立 padding 与 border；中层可偏离中心 4%–8%；主视觉 object-fit:cover；用 outline-offset 而非堆叠 box-shadow 保持边界清晰。
```

## 11｜对角线动势 Diagonal Tension

强斜轴贯穿画面，打破水平稳定，快速建立速度、冲突和方向。

- 视线路径：沿斜轴从一角穿向对角；信息依附或反向切断斜轴。
- 适合：体育、音乐、促销、科技、行动号召、青年文化。
- 避坑：正文不要全部倾斜；斜线必须指向有效信息而非画外。
- 标签：`对角线` `斜轴` `速度` `切割` `diagonal composition` `dynamic axis` `directional tension`

图像模型提示：

```text
竖版海报，以一条从左下穿向右上的强对角轴组织画面；主视觉[对象]压在线上，标题“[主标题]”沿斜轴或与其形成反向切割，次要信息保持水平以保证可读；高速度、高张力。避免所有文字同时旋转、避免斜轴把视线带离关键信息。
Keywords: diagonal composition, dynamic axis, directional tension, kinetic layout.
```

HTML/CSS 提示：

```text
用 ::before 创建宽色带，position:absolute，transform:rotate(-24deg) scale(1.4)；关键元素沿对角的多个锚点绝对定位；正文保持 transform:none。
```

## 12｜放射爆发 Radial Burst

线条、形状或人物从共同中心向外辐射，形成能量爆发与集中传播。

- 视线路径：中心先被锁定，随后向四周扩散，再回到中心文字。
- 适合：音乐节、促销、庆典、运动、宣言、强情绪活动。
- 避坑：放射中心必须唯一；细密射线会干扰小字号。
- 标签：`放射` `爆发中心` `向外扩散` `radial burst` `radiating lines` `explosive focal point`

图像模型提示：

```text
3:4 竖版海报，以[主视觉/标题]为唯一放射中心，使用粗细相间的射线、文字或图形向四周扩散；中心标题“[主标题]”高对比、紧凑，边缘信息较弱；能量强、节庆或宣言感。避免多个放射中心、避免射线穿过小字。
Keywords: radial burst composition, radiating lines, explosive focal point, centrifugal energy.
```

HTML/CSS 提示：

```text
背景使用 repeating-conic-gradient() 生成射线；中心内容用 place-items:center；射线层降低 opacity 并放在文字之下，给中心文字增加纯色底或 isolation:isolate。
```

## 13｜Z 型路径 Z-Path

用左上、右上、左下、右下四个锚点构成折线路径，适合少量信息的顺序阅读。

- 视线路径：左上标题 → 右上主视觉 → 左下说明 → 右下 CTA。
- 适合：活动、产品页首屏、课程、发布会、社交媒体广告。
- 避坑：Z 只是编排脚手架，不是所有观众固定的眼动规律。
- 标签：`Z 型路径` `四角锚点` `顺序阅读` `Z-pattern layout` `corner anchors` `sequential scan`

图像模型提示：

```text
竖版信息海报，使用清晰的 Z 型阅读路径：左上放主标题“[主标题]”，右上放主视觉[对象]，左下放简短说明，右下放日期/CTA；用对比、细线或视线方向暗示两条水平线与中间斜线，但不要画出字母 Z。避免四角同等强。
Keywords: Z-pattern composition, corner anchors, sequential visual path, diagonal handoff.
```

HTML/CSS 提示：

```text
使用 2 列 × 2 行网格，四个内容簇分别占四角；用伪元素绘制低对比折线或通过元素朝向建立交接；右下 CTA 设置最高色彩对比但不超过主标题尺度。
```

## 14｜S 型曲线 S-Curve

以柔和的反向曲线连接多个焦点，产生流动、优雅和渐进叙事。

- 视线路径：从上方一侧切入，沿曲线越过中央，最终到达下方另一侧。
- 适合：美妆、时尚、舞蹈、自然、饮品、优雅叙事。
- 避坑：不要让曲线成为无关装饰；每次转弯应有信息节点。
- 标签：`S 曲线` `蛇形路径` `柔性动势` `S-curve composition` `serpentine flow` `graceful movement`

图像模型提示：

```text
竖版海报，围绕一条隐性的 S 型曲线安排[主视觉]与 3 个信息节点；标题“[主标题]”在上方入口，中段元素跨越画面中心，日期/CTA 位于下方反侧终点；流动、优雅、连续，空间张弛交替。避免无节点的装饰曲线。
Keywords: S-curve composition, serpentine flow, graceful movement, sequential nodes.
```

HTML/CSS 提示：

```text
用 SVG path 或两个大圆弧伪元素构成 S 曲线；三个内容节点用 absolute + 百分比坐标附着路径；正文保持水平。窄屏或 200% 缩放时，只有在最小字号、裁切安全和交互尺寸仍满足时才整体等比缩放；否则按语义 DOM 顺序重排节点，同时保留入口、转折、终点及相邻关系，不强求同一几何曲线。
```

## 15｜L 型锚定 L-Shaped Anchor

一条竖向与一条横向信息带在角落相交，像支架一样托住主视觉。

- 视线路径：沿竖边下行，在角点转向横边，最后进入被框住的主视觉。
- 适合：建筑、展览、目录、理性品牌、空间与工业主题。
- 避坑：L 的两臂需要明显长短关系；四边都封闭会变成框中框。
- 标签：`L 型` `角落锚点` `边栏` `L-shaped layout` `corner anchor` `edge rails`

图像模型提示：

```text
竖版海报，使用 L 型构图：左侧竖向信息栏与底部横向标题带在左下角相交，形成视觉支架；主视觉[对象]占据右上开放区域，标题“[主标题]”沿底边展开；理性、建筑感、边缘对齐明确。避免四边全部封闭。
Keywords: L-shaped composition, corner anchor, edge rails, open field.
```

HTML/CSS 提示：

```text
容器 grid-template-columns:18% 1fr；grid-template-rows:1fr 18%；竖栏占第 1 列，底栏跨两列，主视觉占右上；文字可用 writing-mode:vertical-rl。
```

## 16｜左右分屏 Vertical Split

画面垂直分成两个对等或主次区域，用于对比、问答、前后或双主题。

- 视线路径：先读对比更强的一侧，再跨越中缝读取另一侧关系。
- 适合：对比型产品、双人、前后变化、观点碰撞、联名。
- 避坑：两侧必须有明确关系；50/50 分割可能缺乏主次。
- 标签：`左右分屏` `双主题` `并置对比` `vertical split` `dual-panel composition` `juxtaposition`

图像模型提示：

```text
竖版海报采用左右分屏构图，左侧呈现[对象 A]，右侧呈现[对象 B]，通过色彩/材质形成鲜明对比；标题“[主标题]”跨越中缝或分别拆分在两侧，设置 60/40 主次比例；强调并置关系。避免两侧毫无关联、避免完全同权导致无焦点。
Keywords: vertical split composition, dual-panel layout, juxtaposition, 60/40 balance.
```

HTML/CSS 提示：

```text
使用 5 列 CSS Grid，左区 3 列、右区 2 列；两侧独立背景与 content wrapper；跨缝标题设置 grid-column:2/5、z-index:2。窄屏时仅在两侧都满足最小单元宽度、最小字号和安全裁切时保留并排；否则可改为上下或顺序分区，但保持 A/B 的语义顺序、对比关系和 60/40 主次。
```

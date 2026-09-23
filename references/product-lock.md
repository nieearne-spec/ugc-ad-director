# 产品一致性约束

AI 生成带货视频最常见的失败模式不是画面难看，而是**产品在镜头之间变了样**。观众可能不会立刻说出哪里不对，但「这东西看起来不像同一个」会直接摧毁购买欲。

本文件定义怎么把产品锁死。

## 一、先提取外观清单

在写任何提示词之前，先对着产品图逐项提取。**只写观察到的，不要写推测的。**

| 维度 | 提取内容 | 示例 |
|---|---|---|
| 形状与比例 | 轮廓类型、长宽高关系 | 细长圆柱形，长度约为直径的 6 倍 |
| 主色与配色 | 主色、辅色、色块位置 | 管身为白色，字标区域为蓝色 |
| 字标位置 | **只写位置、大小、形状、颜色** | 字标位于管身上三分之一处，横向排列，蓝色 |
| 材质与表面 | 哑光 / 亮面 / 金属 / 磨砂 | 管身哑光，盖子为半反光塑料 |
| 结构细节 | 可动部件的数量与位置 | 一个螺旋盖，管口有环状凸起 |
| 尺寸参照 | 与手、桌面的相对大小 | 握在手中时长度约为手掌宽度的 1.5 倍 |

**不要写 logo 或包装上的文字内容。**文字渲染在生成模型里不稳定，写了反而会得到变形字符。只描述它的位置和形状。

## 二、约束语句怎么写

约束层里最没用的一句话是 `keep the product consistent`——它什么都没告诉模型。**必须落到具体特征。**

| 差 | 好 |
|---|---|
| `keep the product consistent` | `keep the same slim cylindrical white tube; cap diameter equal to body diameter; blue lettering on the upper third; matte finish with one narrow highlight running vertically` |
| `do not change the product` | `the cap stays closed in shots 1 and 4, and open only in shots 2 and 3; the bullet extends to the same length every time it is visible` |
| `keep colors the same` | `body stays white, lettering stays blue; no color shift between warm and cool shots` |

规则：**约束语句里的信息量，应该等于外观清单里提取到的项数。**漏掉哪一项，哪一项就会漂移。

## 三、按产品类型的锁定重点

不同产品的脆弱点不一样，约束要往对应方向上加重。

| 产品类型 | 最易漂移的项 | 约束重点 |
|---|---|---|
| 圆柱类（唇膏、口红、睫毛膏） | 盖与管的直径关系、膏体伸出长度 | 明确直径比例、每次出现时的膏体长度 |
| 瓶罐类（面霜、精华、洗发水） | 瓶盖比例、瓶身弧度、液面高度 | 瓶身轮廓线、盖子高度占比、液体位置 |
| 盒装类（收纳、包装、玩具） | 长宽高比例、开口结构 | 三边比例、拉链或卡扣的位置与数量 |
| 带屏类（3C 数码） | 屏幕占比、边框宽度、接口数量与位置 | 屏幕与机身的面积比、接口在侧边的具体位置 |
| 软性类（服饰、布艺、包袋） | 轮廓长度、图案密度、缝线 | 长度参照（到大腿中部）、图案的重复密度、五金件数量 |
| 食品与液体 | 包装形状、内容物颜色、液面 | 包装轮廓、内容物的颜色与质地 |

品类卡里的「常见翻车点」表可以与本表对照使用。

## 三点五、两类写不进提示词的产品事实（实战教训）

产品事实登记表里的每一项都成立，但不等于都能照写进 Seedance 提示词。以下两类**照写必翻车**（2026-09-23 吸盘支架实测）：

### 1. 精确计数 → 用拓扑关系代替

「24 颗吸盘、每组 4 列 × 3 行」这类数字约束，视频模型**数不对**——它不会按数量摆放，反而会在特写镜头下把细节铺满整个面来自圆其说。

| 照写（翻车） | 改写（可锁） |
|---|---|
| `two groups of twelve suction cups (four columns by three rows)` | `a smooth matte band across the middle cleanly separates the shallow cups into a top cluster and a bottom cluster; the middle band is always completely free of cups` |
| `show the two rows of suction cups on its back` | `tilts it briefly so the camera catches a glimpse of the cups on its back` |

规则：**可数的细节一律降级为结构关系（分隔、对称、边界的存在性），动态镜头里只给一瞥，不给静态整面展示。**

### 2. 绝对尺寸 → 用相对参照代替

「90 × 60 mm」对视频模型没有意义——它没有尺子的概念，画面里也没有第二把尺子可对。**可锁的只有画面内参照物之间的比例**：产品 vs 手机、产品 vs 手掌、产品 vs 脸。

| 照写（翻车） | 改写（可锁） |
|---|---|
| `about 90 by 60 millimeters` | `clearly smaller than the phone, about 60% of the phone's height; when mounted, almost completely hidden behind the phone with only a slim margin visible` |

规则：**尺寸锁必须绑定一个画面里真实存在的参照物。**写之前先问：这个比例的比较对象会在镜头里出现吗？

### 3. 镜头设计上的推论

需要精确细节的面（整面吸盘、细密纹理、文字排布）**永远不安排正面特写**——特写下模型被迫填充细节，必然自由发挥。替代手法：侧缘展示、一瞥而过、部分遮挡、让动作带过而非停顿展示。


## 四、产品图提示词的写法

方案包第 5 项的两条提示词，同样遵循「先特征、后场景」的顺序。

**5.1 产品干净图**（英文）

```text
Product reference photo, single object centered on a clean pure white background, 
soft even lighting with no harsh shadows, sharp focus on the whole object, 
true-to-life color, no props, no text overlays, no watermark.
[这里插入外观清单里的特征描述]
Negative: distorted shape, changed proportions, invented details, misaligned lettering, 
melted or warped surfaces, watermarks, text, extra objects, harsh reflections, blown-out highlights.
```

**5.2 产品使用场景图**（英文）

```text
Lifestyle product photo, the same product placed in a real [场景], 
natural indoor light, real surface with slight texture, shot at an everyday angle, 
realistic environment details, no studio staging.
[这里插入外观清单里的特征描述]
Negative: distorted product, invented packaging details, unrealistic scale, 
plastic-looking material, watermarks, text.
```

**先判断要不要做**：如果用户提供的产品图已经是干净白底三视图，直接告诉他可以跳过第 5 项。不要为了走完流程让他重复劳动。

## 五、校验

交付前对一遍：

- [ ] 外观清单里每一项都能在约束层里找到对应的表述
- [ ] 没有描述包装或 logo 上的文字内容
- [ ] **没有精确计数类约束（几颗、几行几列）——已降级为拓扑关系**
- [ ] **没有绝对尺寸（毫米/厘米）——比例已绑定画面内真实参照物**
- [ ] **没有把需要精确细节的面安排正面静态特写**
- [ ] 可动部件（盖子、卡扣、拉链）在哪个镜头是什么状态，已明确写出
- [ ] 产品在不同镜头里的尺寸参照一致（都是与手或桌面比）
- [ ] 约束层没有被删减——如果用户反馈产品变形，第一件事就是检查这一层

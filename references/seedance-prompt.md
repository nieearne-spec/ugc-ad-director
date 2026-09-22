# Seedance 提示词编译规则

第 9 项是整份方案包的最终交付物：**一段连续可复制的提示词**。

## 一、语言分层（最重要的一条）

提示词里同时存在两种语言，各司其职：

| 内容 | 语言 | 原因 |
|---|---|---|
| 风格定调 | **英文** | 模型对英文画面描述的解析更稳定 |
| 时间轴里的画面与动作描述 | **英文** | 同上。景别、运镜、光线、材质的英文描述还原度更高 |
| 台词 | **目标市场语言** | 口型同步与音频生成都依赖它 |
| 引用锚点说明 | **英文** | 属于技术描述 |
| 约束条件 | **英文** | 同上 |

**为什么必须分开**：做英语市场时，画面描述和台词碰巧同语言，看不出区别；一旦切到西语、葡语、日语市场，如果整段都写成目标语言，画面描述的质量会明显下降；如果整段都写成英文，生成的音频就会说英文。**所以两者必须显式分层。**

**台词的语言标注**：在提示词中不要只写语言名。目标市场语言必须在方案包第 1 项的确认表里写明变体（如 `es-MX`），并在使用说明里提示用户选择支持该语言口型同步的模型。

## 二、四层结构

### 第 1 层：风格定调（英文，2–3 句）

一次性定下整片质感。必须包含：

- 视频形态与比例（`vertical video`、`9:16`）
- 视角（`phone front-camera look` / `handheld selfie angle`）
- 光线（`soft natural indoor light`）
- 真实度锚点（`realistic skin texture with visible pores`、`no beauty filter`、`no studio lighting`）

UGC 的核心是「不像广告」。这一层就是用来压住广告感的第一道闸。

### 第 2 层：时间轴分镜（英文描述 + 目标语言台词）

按 `00:00-00:02 —` 的格式逐段写，**段与段之间用空行隔开**，便于阅读与修改。

每段包含：

- 时间段
- 景别 + 运镜
- 画面内容（人物动作、产品状态、环境）
- 单独一行 `Dialogue:` 后跟台词原文

时间轴必须与第 7 项脚本主版、第 8 项镜头清单**逐字对应**。三者不能有任何出入。

### 第 3 层：引用锚点（英文）

用两句话说明 `@image1`、`@image2` 分别指什么。写法：

```text
Reference anchors: @image1 is the woman (host reference). @image2 is the product 
(slim white tube with blue lettering).
```

**数量约定**：默认两个锚点——人物在前，产品在后。若用户要求只拍产品（不出现人物），则只保留产品锚点。

### 第 4 层：约束条件（英文）

这一层决定了成片能不能用。必须覆盖：

- **产品一致性**：外观、颜色、字标位置、结构、比例在全部镜头里不变
- **人物一致性**：面部、发型、服装在全部镜头里不变（服饰品类尤其关键）
- **手部完整性**：五指、比例自然
- **口型同步**：唇部与下颌运动与台词对齐
- **背景一致性**：空间布局、家具位置不变
- **禁止项**：文字叠加、水印、价格标签、额外物品
- **皮肤真实度**：不要平滑、不要美化

**这一层不要删减。**用户若反馈「产品变形」，第一处理方式就是让他确认约束层是否完整保留。

## 三、完整示例

以下为「墨西哥市场 + 美妆个护 + 15 秒 + 真人口播」的提示词成品。实际输出时按用户的产品替换即可。

```text
Authentic UGC-style vertical video, 9:16, phone front-camera look, handheld with slight
natural movement, soft natural indoor light, realistic skin texture with visible pores,
no beauty filter, no studio lighting, no glossy retouching.

00:00-00:02 — Medium close-up, static. A young Mexican woman in her late twenties sits at a
small vanity table in a softly lit bedroom. She holds a slim white lipstick tube with small
blue lettering, looks directly into the lens and speaks.
Dialogue: "Si tus labios siempre están secos, necesitas ver esto."

00:02-00:07 — Extreme close-up, slow push in. Her hand lifts the tube and opens it; the bullet
is fully extended and catches soft light. The tube shape and the blue lettering position stay
unchanged from the previous shot.
Dialogue: "Mira qué suave se ve la textura."

00:07-00:12 — Close-up, handheld with slight shake. She applies it once to her lower lip and
keeps the movement simple. Skin texture stays natural, no smoothing applied, no visible
retouching.
Dialogue: "Y no tienes que usar capas y capas para que se note."

00:12-00:15 — Medium close-up, static. She holds the tube up beside her face with the label
facing the camera, then lowers it slightly.
Dialogue: "Te dejo el link aquí abajo."

Reference anchors: @image1 is the woman (host reference). @image2 is the product (slim white
lipstick tube with blue lettering).

Consistency constraints: the product must remain identical in every shot — same slim white
tube, same blue lettering position, same cap and bullet proportions, no added details, no
logo changes, no color shift. Keep the woman's face, hairstyle and outfit identical across
all shots. Keep the bedroom background layout consistent. Hands must have exactly five
fingers with natural proportions. Keep lip and jaw movement in sync with the dialogue.
Do not add text overlays, watermarks, price tags, captions or extra objects. Do not smooth,
brighten or beautify the skin.
```

**这段提示词的设计说明**（供理解，不要写进交付的提示词里）：

- 台词共 34 词，15 秒内留有停顿空间，不会赶
- 第二个镜头的「开盖」动作是全片唯一的手部精细动作，风险集中在一处，便于排查
- 台词没有一句宣称使用效果——「质地看起来柔滑」是视觉观察，「不用涂很多层」是产品特性，都不涉及亲述体验
- 末镜产品正对镜头并带字标，保证观众记得住包装

## 四、使用说明（随提示词一起交付）

提示词本体之后另起一段，用中文写：

```text
使用说明：
1. 提示词中的 @image1 / @image2 是占位引用。上传素材时按「先人物、后产品」的顺序上传，
   顺序不同则必须手动改掉编号——否则模型不知道在指谁。
2. 台词为墨西哥西语（es-MX），请在生成工具中选择支持该语言口型同步的模型。
3. 若成片里产品出现变形，先检查第 4 层约束条件是否被完整保留，不要直接删减。
4. 建议先用最短时长试生成一个镜头，确认人物与产品无误后再生成全部镜头，避免整批返工。
```

## 五、常见问题与修复

| 现象 | 原因 | 修复方式 |
|---|---|---|
| 产品变形、结构改变 | 约束层不足或缺失 | 补全第 4 层的产品一致性描述，写明具体特征而非「保持一致」 |
| 人物换脸、发型变化 | 未使用参考图，或参考图权重不足 | 改用参考图生视频，不要纯文生视频 |
| 手部多指、畸形 | 手部精细动作过多 | 减少每个镜头内的手部操作数量；把复杂动作拆到不同镜头 |
| 画面太「干净」、像广告 | 风格定调层缺失真实度锚点 | 补 `no studio lighting`、`no beauty filter`、`visible pores` |
| 台词与口型不同步 | 模型不支持该语言，或台词过长 | 换支持该语言的模型；缩短单句台词 |
| 屏幕内容乱码（3C 品类） | 模型无法稳定渲染界面 | 不要描述屏幕具体内容，改为「屏幕亮起」或让屏幕转向侧面 |
| 颜色偏移（服饰品类） | 多场景色温不一致 | 全片使用同一光线描述，避免冷暖跳变 |
| 生成结果像定格动画 | 镜头切换过密 | 减少镜头数量，单镜时长拉到 4 秒以上 |

## 六、交付前的最后一道检查

- [ ] 整段提示词中间**没有夹中文注释**
- [ ] 画面描述全是英文
- [ ] 台词全是目标市场语言，且与脚本主版逐字一致
- [ ] 时间轴覆盖完整，没有时间缺口或重叠
- [ ] 第 3 层锚点数量与用户的上传方式一致
- [ ] 第 4 层包含产品、人物、手部、口型、背景、禁止项、皮肤七类约束
- [ ] 使用说明已附在提示词之后，且明确标注了「不属于提示词本体」

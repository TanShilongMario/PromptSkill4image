# Examples

以下示例展示这个 Skill 的核心用法：图像反推、粗糙提示词扩写、翻译转写与变量提炼、PromptFill 模板输出。

---

## 示例 1：极简提示词增强

用户输入：

```text
赛博朋克少女，雨夜街道，霓虹灯
```

建议输出：

```markdown
## Enhanced Prompt

### Minimal Version
A cyberpunk girl standing on a rainy night street, surrounded by neon signs and wet reflections, cinematic lighting, high-detail portrait, shallow depth of field.

### Advanced Version
Subject: a cyberpunk girl with a calm, mysterious expression, subtle tech accessories, and a futuristic streetwear outfit
Scene: rainy night street in a dense neon city, wet asphalt reflecting signs and holographic billboards
Composition: medium portrait, eye-level camera, subject centered with deep background perspective
Lighting: neon rim light, soft blue and magenta glow, cinematic contrast
Color: cyber purple-blue palette with small warm orange highlights
Style: photorealistic sci-fi editorial portrait
Quality: ultra-detailed, realistic skin texture, atmospheric rain, sharp focus

### Further Suggestions
- 如果想更像商业海报，可加入品牌字体、留白和竖版 9:16。
- 如果想更像电影剧照，可强化镜头语言，如 50mm lens、anamorphic bokeh。
```

---

## 示例 2：图像反推提示词

用户输入：

```text
请根据这张图反推一个可用于生图的英文提示词。
```

建议输出结构：

```markdown
## Image Prompt

### Minimal Version
A luxury perfume bottle on a clean cream-colored studio set, surrounded by soft geometric props and delicate flower petals, premium product photography, soft diffused lighting, elegant commercial poster style.

### Advanced Version
Subject: a luxury glass perfume bottle with a refined label and transparent golden liquid
Scene: clean cream-colored studio background with soft geometric display blocks
Composition: centered product hero shot, low front angle, balanced negative space for ad copy
Lighting: diffused studio softbox lighting with gentle rim highlights and subtle reflections
Color: warm cream, champagne gold, pale blush accents
Material: clear glass, glossy cap, soft paper label, smooth acrylic display props
Style: premium commercial product photography, minimalist luxury advertising
Quality: ultra-clean, high-resolution, realistic reflections, polished editorial finish

### Observed Elements
- Subject: perfume bottle
- Scene: studio product setup
- Lighting: soft commercial lighting
- Style: luxury minimalist product poster

### Further Suggestions
- 如果用于 Midjourney，可追加 `--ar 4:5` 或 `--ar 9:16`。
- 如果用于电商主图，建议保留更干净的背景和更少装饰。
```

注意：反推结果是对画面的实用重构，不声称还原原始生成参数。

---

## 示例 3：翻译转写并提炼变量

用户输入：

```text
把这段中文提示词转成英文，并提炼变量和词组建议：
古风仙女，穿着汉服广袖，站在仙境云海之上，丁达尔光线，梦幻飘渺氛围
```

建议输出：

```markdown
## Transwritten Prompt

### Chinese
古风仙女穿着汉服广袖，站在仙境般的云海之上，丁达尔光线穿透云层，营造梦幻飘渺的东方幻想氛围，电影感构图，高细节插画质感。

### English
An ancient fantasy fairy wearing wide-sleeved Hanfu, standing above a fairyland sea of clouds, Tyndall light rays breaking through the mist, creating a dreamy and ethereal Eastern fantasy atmosphere, cinematic composition, high-detail illustration quality.

## Variables

| Variable | Current Value | Category | Suggestions |
|---|---|---|---|
| `character_type` | 古风仙女 / ancient fantasy fairy | character | 剑修 / sword cultivator, 龙族王子 / dragon prince |
| `outfit` | 汉服广袖 / wide-sleeved Hanfu | item | 道袍仙衣 / Taoist robe, 晚礼服 / evening gown |
| `location` | 仙境云海 / fairyland sea of clouds | location | 江南水乡 / Jiangnan water town, 雪山之巅 / mountain summit |
| `lighting` | 丁达尔光线 / Tyndall light rays | visual | 黄金时刻光 / golden hour light, 月光冷光 / moonlight cold glow |
| `mood` | 梦幻飘渺 / dreamy and ethereal | visual | 神秘庄严 / mysterious and solemn, 温柔宁静 / gentle and serene |

## Further Suggestions
- 想保持简洁时，直接使用英文转写版即可。
- 想复用模板时，可改成 `{{character_type}} wearing {{outfit}}, standing in {{location}}, {{lighting}}, {{mood}} atmosphere`。
```

---

## 示例 4：PromptFill JSON

用户输入：

```text
把这段提示词做成 PromptFill 模板 JSON：
古风仙女，穿着汉服广袖，站在仙境云海之上，丁达尔光线，梦幻飘渺氛围
```

建议输出：

```json
{
  "id": "tpl_ancient_fantasy_fairy",
  "name": {
    "cn": "古风仙女",
    "en": "Ancient Fantasy Fairy"
  },
  "content": {
    "cn": "{{character_type: 古风仙女}}，穿着{{outfit: 汉服广袖}}，站在{{location: 仙境云海}}之上，{{lighting: 丁达尔光线}}，{{mood: 梦幻飘渺}}氛围，{{render_quality: 高细节插画质感}}",
    "en": "{{character_type: Ancient Fantasy Fairy}}, wearing {{outfit: Wide-Sleeved Hanfu}}, standing above {{location: Fairyland Sea of Clouds}}, {{lighting: Tyndall Light Rays}}, {{mood: Dreamy and Ethereal}} atmosphere, {{render_quality: High-Detail Illustration Quality}}"
  },
  "imageUrl": "https://placehold.co/600x400/png?text=Ancient+Fantasy+Fairy",
  "selections": {
    "character_type": { "cn": "古风仙女", "en": "Ancient Fantasy Fairy" },
    "outfit": { "cn": "汉服广袖", "en": "Wide-Sleeved Hanfu" },
    "location": { "cn": "仙境云海", "en": "Fairyland Sea of Clouds" },
    "lighting": { "cn": "丁达尔光线", "en": "Tyndall Light Rays" },
    "mood": { "cn": "梦幻飘渺", "en": "Dreamy and Ethereal" },
    "render_quality": { "cn": "高细节插画质感", "en": "High-Detail Illustration Quality" }
  },
  "tags": ["古风", "人物", "幻想"],
  "language": ["cn", "en"],
  "banks": {
    "character_type": {
      "label": { "cn": "角色类型", "en": "Character Type" },
      "category": "character",
      "options": [
        { "cn": "古风仙女", "en": "Ancient Fantasy Fairy" },
        { "cn": "剑修", "en": "Sword Cultivator" },
        { "cn": "精灵弓手", "en": "Elf Archer" },
        { "cn": "魔法师", "en": "Mage" },
        { "cn": "龙族王子", "en": "Dragon Prince" }
      ]
    },
    "lighting": {
      "label": { "cn": "光照效果", "en": "Lighting" },
      "category": "visual",
      "options": [
        { "cn": "丁达尔光线", "en": "Tyndall Light Rays" },
        { "cn": "黄金时刻光", "en": "Golden Hour Light" },
        { "cn": "月光冷光", "en": "Moonlight Cold Glow" },
        { "cn": "电影感打光", "en": "Cinematic Lighting" },
        { "cn": "体积光束", "en": "Volumetric Light Beams" }
      ]
    }
  }
}
```

实际输出时，应为所有重要变量补齐 `banks`，此处为节省篇幅仅展示部分。
# Examples - 使用示例

---

## 示例 1：简单人像提示词（模式 A：仅分析）

**用户输入**：
```
分析这个提示词：
"一个 [艺术风格] 的 [角色]，穿着 [服装]，在 [场景] 中，[光照] 光线"
```

**助手输出**：

```markdown
## 📋 提示词分析

| 变量名 | 当前占位 | 语义类别 | 词库状态 |
|--------|---------|---------|---------|
| art_style | [艺术风格] | 视觉 | ✓ 通用词库已有 |
| character_type | [角色] | 主体 | ✓ 通用词库已有 |
| outfit | [服装] | 道具 | ✓ 通用词库已有 |
| location | [场景] | 场景 | ✓ 通用词库已有 |
| lighting | [光照] | 视觉 | ✓ 通用词库已有 |

## 📐 结构分析
- ✅ 包含主体描述
- ✅ 包含场景环境
- ✅ 包含光照参数
- ⚠️ 建议补充：构图视角 `{{camera_angle}}`
- ⚠️ 建议补充：色彩方案 `{{color_scheme}}`
- ⚠️ 建议补充：渲染质量 `{{render_quality}}`

## 💡 质量评估
- 完整性：6/10 — 缺少技术参数
- 专业性：7/10 — 结构清晰
- 可变性：9/10 — 变量划分合理

## 🎯 改进建议
在末尾添加：`{{camera_angle}}，{{color_scheme}}色调，{{render_quality}}`
```

---

## 示例 2：复杂提示词拆分（模式 B：拆分 + 词库）

**用户输入**：
```
帮我拆分这段提示词并生成词库：

### 赛博朋克角色设计

由赛博朋克大师设计的机甲战士，战术皮衣风格，
手持光子步枪，站在废弃工业区中。

**细节**：
- 霓虹灯光照射
- 低角度仰拍
- 紫蓝赛博色调
```

**助手输出**：

```markdown
## 📋 变量拆分结果

| 变量名 | 当前值 | 类别 | 词库状态 |
|--------|-------|------|---------|
| art_style | 赛博朋克 | visual | ✓ 已有 |
| character_type | 机甲战士 | character | ✓ 已有 |
| outfit | 战术皮衣 | item | ✓ 已有 |
| weapon | 光子步枪 | item | ✓ 已有 |
| location | 废弃工业区 | location | ✓ 已有 |
| lighting | 霓虹灯光 | visual | ✓ 已有 |
| camera_angle | 低角度仰拍 | technical | ✓ 已有 |
| color_scheme | 紫蓝赛博色调 | visual | ✓ 已有 |

---

## 🔧 结构化提示词

### 中文版
```
### {{art_style}}角色设计

由{{art_style}}大师设计的{{character_type}}，{{outfit}}风格，
手持{{weapon}}，站在{{location}}中。

**细节**：
- {{lighting}}照射
- {{camera_angle}}
- {{color_scheme}}色调
```

### English Version
```
### {{art_style}} Character Design

A {{character_type}} designed by {{art_style}} master, {{outfit}} style,
holding {{weapon}}, standing in {{location}}.

**Details**:
- {{lighting}} illumination
- {{camera_angle}}
- {{color_scheme}} tones
```

---

## 📚 变量词库

### {{art_style}} — 艺术风格

| 中文 | English |
|------|---------|
| 赛博朋克 | Cyberpunk |
| 蒸汽朋克 | Steampunk |
| 水墨国风 | Chinese Ink Painting |
| 暗黑哥特 | Dark Gothic |
| 超现实主义 | Surrealism |
| 吉卜力风格 | Ghibli Studio Style |
| 赛博道家 | Cyber-Taoist |
| 新海诚风格 | Makoto Shinkai Style |

### {{character_type}} — 角色类型

| 中文 | English |
|------|---------|
| 机甲战士 | Mecha Warrior |
| 赛博少女 | Cyberpunk Girl |
| 暗影刺客 | Shadow Assassin |
| 星际探索者 | Interstellar Explorer |
| 古风仙子 | Ancient Fantasy Fairy |
| 末日幸存者 | Post-Apocalypse Survivor |
| 精灵弓手 | Elf Archer |
| 魔法师 | Mage |

### {{weapon}} — 武器

| 中文 | English |
|------|---------|
| 光子步枪 | Photon Rifle |
| 符文长剑 | Runic Longsword |
| 能量护盾 | Energy Shield |
| 魔法权杖 | Magic Staff |
| 双刃战斧 | Double-Bladed Battleaxe |
| 时空镰刀 | Time-Space Scythe |

[... 其余词库省略，实际输出完整 ...]

---

## 🎲 示例填充版本（随机生成，可直接测试）

```
### 蒸汽朋克角色设计

由蒸汽朋克大师设计的暗影刺客，战甲盔甲风格，
手持能量护盾，站在魔法学院中。

**细节**：
- 丁达尔光线照射
- 侧面轮廓构图
- 金红暖色调
```
```

---

## 示例 3：生成 PromptFill JSON（模式 C）

**用户输入**：
```
帮我把这个提示词生成 PromptFill 模板 JSON：

古风仙女，穿着汉服广袖，站在仙境云海之上，
丁达尔光线，俯视鸟瞰视角，梦幻飘渺氛围
```

**助手输出**：

````markdown
## ✅ PromptFill 模板 JSON

```json
{
  "id": "tpl_xianxia_fairy",
  "name": { "cn": "古风仙女", "en": "Ancient Fantasy Fairy" },
  "content": {
    "cn": "{{character_type: 古风仙女}}，穿着{{outfit: 汉服广袖}}，站在{{location: 仙境云海}}之上，\n{{lighting: 丁达尔光线}}，{{camera_angle: 俯视鸟瞰}}视角，{{mood: 梦幻飘渺}}氛围",
    "en": "{{character_type: Ancient Fantasy Fairy}}, wearing {{outfit: Hanfu Wide Sleeves}}, standing above {{location: Fairyland Cloud Sea}},\n{{lighting: Tyndall Effect Rays}}, {{camera_angle: Bird's Eye View}}, {{mood: Dreamy and Ethereal}} atmosphere"
  },
  "imageUrl": "https://placehold.co/600x400/png?text=Ancient+Fantasy+Fairy",
  "selections": {
    "character_type": { "cn": "古风仙女", "en": "Ancient Fantasy Fairy" },
    "outfit": { "cn": "汉服广袖", "en": "Hanfu Wide Sleeves" },
    "location": { "cn": "仙境云海", "en": "Fairyland Cloud Sea" },
    "lighting": { "cn": "丁达尔光线", "en": "Tyndall Effect Rays" },
    "camera_angle": { "cn": "俯视鸟瞰", "en": "Bird's Eye View" },
    "mood": { "cn": "梦幻飘渺", "en": "Dreamy and Ethereal" }
  },
  "tags": ["古风", "人物", "仙侠"],
  "language": ["cn", "en"],
  "banks": {
    "character_type": {
      "label": { "cn": "角色类型", "en": "Character Type" },
      "category": "character",
      "options": [
        { "cn": "古风仙女", "en": "Ancient Fantasy Fairy" },
        { "cn": "剑修", "en": "Sword Cultivator" },
        { "cn": "龙族王子", "en": "Dragon Prince" },
        { "cn": "精灵弓手", "en": "Elf Archer" },
        { "cn": "魔法师", "en": "Mage" }
      ]
    },
    "outfit": {
      "label": { "cn": "服装风格", "en": "Outfit Style" },
      "category": "item",
      "options": [
        { "cn": "汉服广袖", "en": "Hanfu Wide Sleeves" },
        { "cn": "道袍仙衣", "en": "Taoist Robe" },
        { "cn": "战甲盔甲", "en": "Battle Armor" },
        { "cn": "赛博皮衣", "en": "Cyberpunk Leather Jacket" },
        { "cn": "晚礼服", "en": "Evening Gown" }
      ]
    },
    "location": {
      "label": { "cn": "场景地点", "en": "Location" },
      "category": "location",
      "options": [
        { "cn": "仙境云海", "en": "Fairyland Cloud Sea" },
        { "cn": "古风江南水乡", "en": "Ancient Jiangnan Water Town" },
        { "cn": "魔法学院", "en": "Magic Academy" },
        { "cn": "雪山之巅", "en": "Mountain Summit" },
        { "cn": "樱花古道", "en": "Cherry Blossom Path" }
      ]
    },
    "lighting": {
      "label": { "cn": "光照效果", "en": "Lighting" },
      "category": "visual",
      "options": [
        { "cn": "丁达尔光线", "en": "Tyndall Effect Rays" },
        { "cn": "黄金时刻光", "en": "Golden Hour Light" },
        { "cn": "月光冷光", "en": "Moonlight Cold Glow" },
        { "cn": "逆光剪影", "en": "Backlight Silhouette" },
        { "cn": "荧光生物光", "en": "Bioluminescent Glow" }
      ]
    },
    "camera_angle": {
      "label": { "cn": "构图视角", "en": "Camera Angle" },
      "category": "technical",
      "options": [
        { "cn": "俯视鸟瞰", "en": "Bird's Eye View" },
        { "cn": "低角度仰拍", "en": "Low Angle Shot" },
        { "cn": "平视正面", "en": "Eye-Level Front" },
        { "cn": "侧面轮廓", "en": "Side Profile" },
        { "cn": "3/4 侧面", "en": "Three-Quarter View" }
      ]
    },
    "mood": {
      "label": { "cn": "情绪氛围", "en": "Mood" },
      "category": "visual",
      "options": [
        { "cn": "梦幻飘渺", "en": "Dreamy and Ethereal" },
        { "cn": "史诗宏大", "en": "Epic and Grand" },
        { "cn": "宁静祥和", "en": "Calm and Serene" },
        { "cn": "神秘莫测", "en": "Mysterious and Enigmatic" },
        { "cn": "忧郁诗意", "en": "Melancholic and Poetic" }
      ]
    }
  }
}
```

**使用说明**：
- 将此 JSON 导入 PromptFill 应用即可使用
- `imageUrl` 请替换为实际预览图
- `content` 中的 `{{key: 内联值}}` 表示该变量的当前默认值
````

---

## 示例 4：英文提示词处理

**用户输入**：
```
Split this prompt and generate vocabulary:

"A cyberpunk girl with [hair_style], wearing [outfit], 
in a [location], [lighting] light, shot with [camera_angle]"
```

**助手输出**：

```markdown
## 📋 Variable Extraction

| Variable | Current Value | Category | Bank Status |
|----------|--------------|---------|------------|
| hair_style | [hair_style] | character | ✓ Available |
| outfit | [outfit] | item | ✓ Available |
| location | [location] | location | ✓ Available |
| lighting | [lighting] | visual | ✓ Available |
| camera_angle | [camera_angle] | technical | ✓ Available |

## 🔧 Structured Prompt

### English
```
A cyberpunk girl with {{hair_style}}, wearing {{outfit}},
in a {{location}}, {{lighting}} light, shot with {{camera_angle}}
```

### 中文版
```
赛博朋克少女，{{hair_style}}发型，穿着{{outfit}}，
在{{location}}中，{{lighting}}光线，{{camera_angle}}构图
```

## 📚 Vocabulary Banks

### {{hair_style}} — Hair Style / 发型

| 中文 | English |
|------|---------|
| 银白短发 | Silver White Short Hair |
| 双马尾 | Twin Tails |
| 渐变染发 | Gradient Dyed Hair |
| 长直黑发 | Long Straight Black Hair |
| 蓬松卷发 | Fluffy Curly Hair |
| 半扎发 | Half-Up Half-Down |

[... 其余变量词库 ...]
```

---

## 提示：何时使用内联默认值

当提示词中某个位置有**明确推荐值**，但仍希望保留可替换性时，使用 `{{key: 具体值}}` 语法：

```
# 普通占位（完全可替换）
{{art_style}}风格的{{character_type}}

# 内联默认值（有推荐值，但可替换）
{{art_style: 赛博朋克}}风格的{{character_type: 赛博少女}}
```

**建议**：
- 有强烈设计意图的核心元素 → 使用内联默认值
- 完全开放的可变参数 → 使用普通占位
- 临时性、场景专属的词条 → 使用内联默认值（无需建词库）

复杂的结构化提示词示例：

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

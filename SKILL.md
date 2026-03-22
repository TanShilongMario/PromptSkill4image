---
name: prompt-engineering
description: Universal prompt engineering assistant. Deconstructs complex prompts into structured variables, generates bilingual vocabulary banks, identifies domain, evaluates quality, learns new terms, compares prompts, and optionally outputs PromptFill-compatible JSON. Use when a user wants to analyze, split, translate, learn from, compare, or template-ify any AI image/text prompt. Trigger phrases include: "analyze prompt", "split prompt", "extract variables", "create template", "learn vocabulary", "compare prompts", "词库", "提示词拆分", "提示词分析", "创建模板", "学习新词", "对比提示词".
---

# Prompt Engineering - 通用提示词工程助手

将任意复杂提示词拆解为结构化变量，提供配套词库与双语翻译，支持词库扩展学习、提示词对比、质量评估，可选生成 PromptFill 兼容的 JSON 模板。

---

## 工作模式

收到提示词后，**先询问用户需要哪种输出**（除非用户已明确指定）：

```
我可以提供以下服务，请选择：

A. 仅分析      — 提取变量、识别领域、评估质量、给出改进建议
B. 拆分 + 词库  — 提取变量，并为每个变量生成候选词库（推荐）
C. 完整模板输出 — 生成 PromptFill 兼容的 JSON 模板（含词库）
D. 学习新词    — 从提示词中提取新变量/词条，扩展词库
E. 对比分析    — 对比两段提示词的结构差异

（默认推荐 B）
```

---

## 核心工作流

### Step 1：解析提示词

识别所有可变部分，统一标注为 `{{variable_name}}` 格式：

**识别模式**：
- `[内容]` → `{{variable_name}}`
- `（内容）`/ `(内容)` → `{{variable_name}}`
- `__________` 下划线占位 → `{{variable_name}}`
- 语义上明显可替换的名词/短语 → `{{variable_name}}`
- 已有 `{{key}}` 或 `{{key: 内联默认值}}` → 保留并识别（`baseKey` 为冒号前部分）

**变量命名规则**：
- 小写英文 + 下划线：`art_style`、`character_type`
- 名称应描述变量语义，而非具体值
- 同一 baseKey 多处出现合并计数，不分裂
- 同义变量归并为同一 key

---

### Step 2：领域识别与结构分析报告

**领域分类**（根据主要描述对象判断）：

| 领域 | 触发特征 | 典型变量 |
|------|---------|---------|
| `portrait` | 人像、角色、人物 | character_type, expression, outfit |
| `product` | 产品、商品、物体 | product_type, material, background |
| `design` | 平面、海报、UI | layout, typography, color_scheme |
| `art` | 纯艺术、概念图 | art_style, mood, composition |
| `common` | 通用 / 无法判断 | — |

**输出标准化分析表格**：

```markdown
## 📋 提示词分析

- 总长度：XXX 字符 / 变量数量：X 个
- 主要领域：portrait / 次要领域：art

| 变量名 | 当前值/占位 | 内联值(若有) | 语义类别 | 词库状态 |
|--------|------------|------------|---------|---------|
| art_style | 赛博朋克 | — | visual | ✓ 通用词库已有 |
| character_type | 少女 | — | character | ✓ 通用词库已有 |
| weapon | 光剑 | — | item | ✎ 建议新建 |
| lighting | 霓虹灯光 | — | visual | ✓ 通用词库已有 |
```

**变量语义类别**（参考 [vocabulary-banks.md](vocabulary-banks.md)）：

| 类别 | 英文 key | 说明 |
|------|---------|------|
| 主体 | `character` | 人物、角色、生物 |
| 道具 | `item` | 物品、配饰、武器 |
| 动作 | `action` | 动词、姿势、行为 |
| 场景 | `location` | 地点、环境、背景 |
| 视觉 | `visual` | 风格、色彩、光影 |
| 技术 | `technical` | 镜头、渲染、构图 |
| 其他 | `other` | 不属于以上任何类别 |

---

### Step 3：词库生成（模式 B/C）

为每个变量生成 **5–12 个候选词条**：

```markdown
### {{art_style}} — 艺术风格

| 中文 | English |
|------|---------|
| 赛博朋克 | Cyberpunk |
| 蒸汽朋克 | Steampunk |
| 水墨国风 | Chinese Ink Painting |
| 吉卜力风格 | Ghibli Style |
| 超现实主义 | Surrealism |
| 暗黑哥特 | Dark Gothic |
| 新海诚风格 | Makoto Shinkai Style |
```

**词库生成规则**：
- 覆盖主流常见选项，兼顾小众精品
- 双语完整，英文为 AI 平台主流表达
- 选项之间语义具有明显差异（避免同义重复）
- 内联默认值（`{{key: 某值}}`）中的具体词自动纳入词库
- 若内联值与通用词库某选项一致，标注为"正式选项"；若不在词库中，标注为"临时词条，建议加入词库"

---

### Step 4：重构提示词

输出三个版本：

**结构化版本**（变量已标注）：
```
{{art_style}}风格的{{character_type}}，
手持{{weapon}}，站在{{location}}中。
{{lighting}}光线，{{camera_angle}}构图。
```

**内联默认值版本**（推荐值已内嵌，仍可替换）：
```
{{art_style: 赛博朋克}}风格的{{character_type: 少女}}，
手持{{weapon: 光剑}}，站在{{location: 废弃工厂}}中。
{{lighting: 霓虹灯光}}，{{camera_angle: 低角度仰拍}}构图。
```

**示例填充版本**（从词库随机抽取值填充，供直接测试）：
```
蒸汽朋克风格的机甲战士，
手持符文长剑，站在赛博朋克都市中。
体积光束照射，俯视鸟瞰构图。
```

> 示例填充版本可直接复制到 AI 平台测试效果，确认满意后再进行下一步。

---

### Step 5：PromptFill JSON 输出（仅模式 C）

> 完整示例见 [examples.md](examples.md)

生成完整 JSON：

```json
{
  "id": "tpl_<descriptive_name>",
  "name": { "cn": "模板中文名", "en": "Template English Name" },
  "content": {
    "cn": "{{art_style: 赛博朋克}}风格的{{character_type}}...",
    "en": "{{art_style: Cyberpunk}} style {{character_type}}..."
  },
  "imageUrl": "https://placehold.co/600x400/png?text=Template",
  "selections": {
    "art_style": { "cn": "赛博朋克", "en": "Cyberpunk" }
  },
  "tags": ["角色", "风格"],
  "language": ["cn", "en"],
  "banks": {
    "art_style": {
      "label": { "cn": "艺术风格", "en": "Art Style" },
      "category": "visual",
      "options": [
        { "cn": "赛博朋克", "en": "Cyberpunk" },
        { "cn": "蒸汽朋克", "en": "Steampunk" }
      ]
    }
  }
}
```

**字段说明**：
- `id`：唯一标识符，`tpl_` 前缀 + 英文描述
- `content`：双语提示词正文，支持 `{{variable}}` 和 `{{variable: 内联值}}` 两种写法
- `selections`：每个变量的默认选中值（各一个双语对象）
- `banks`：随模板附带的词库定义（含 label、category、options）
- `tags`：内容主题标签（不含"图片"、"视频"等类型词）

---

### Step 6：词库扩展学习（模式 D）

从提示词或用户提供的词条中学习新变量，**半自动模式**（生成建议后等待人工审核）：

**工作流程**：

```
输入提示词 / 词条列表
    ↓
对照 vocabulary-banks.md 检查现有词库
    ↓
标记新变量（不在词库中的 key）
    ↓
提取可复用的候选选项
    ↓
评估复用性（1-10 分）
    ↓
生成学习报告 → 等待用户审核
```

**学习报告格式**：

```markdown
## 元素学习报告

### ✨ 新变量：{{weapon_type}}
- 推荐类别：`item`
- 复用性评分：8/10（多种场景通用）
- 建议选项：

| 中文 | English |
|------|---------|
| 符文长剑 | Runic Longsword |
| 光子步枪 | Photon Rifle |
| 时空镰刀 | Time-Space Scythe |

**审核选项**：
- [ ] 批准加入词库
- [ ] 需要修改（请说明）
- [ ] 拒绝（理由）

---

### 🔄 现有词库扩展建议：{{lighting}} 新增选项
- 建议添加：`{ cn: "张艺谋电影灯光", en: "Zhang Yimou Cinematic Lighting" }`
- 原因：未在现有词库中，具有较高复用性

---

### 💡 内联词条升级建议
- `{{art_style: 赛博道家}}` — "赛博道家"不在词库中，建议升级为正式选项
- `{{lighting: 月光冷光}}` — "月光冷光"已在词库中，可改回 `{{lighting}}` 普通占位
```

**复用性评分标准**：

| 分数 | 含义 |
|------|------|
| 8–10 | 通用性强，多种场景复用，建议立即添加 |
| 5–7  | 有一定复用价值，可选添加 |
| 1–4  | 场景高度专属，建议保持为内联临时词条 |

---

### Step 7：对比分析（模式 E）

对比两段提示词的结构差异：

```markdown
## 对比分析报告

### 📊 基本对比

| 维度 | 提示词 A | 提示词 B |
|------|---------|---------|
| 总长度 | 120 字符 | 85 字符 |
| 变量数量 | 8 个 | 5 个 |
| 主要领域 | portrait | portrait |
| 结构层次 | 3 层 | 1 层 |

### 🔍 变量差异

| 变量名 | A | B | 说明 |
|--------|---|---|------|
| art_style | ✓ | ✓ | 两者均有 |
| lighting | ✓ | ✗ | A 有，B 缺失 |
| camera_angle | ✗ | ✓ | B 有，A 缺失 |
| render_quality | ✓ | ✗ | A 更专业 |

### 💡 结论
- A 更完整，适合精细控制
- B 更简洁，适合快速测试
- 建议：将 A 的 `{{lighting}}` 和 `{{render_quality}}` 补充到 B 中
```

---

## 质量评估

| 维度 | 评分 | 说明 |
|------|------|------|
| 完整性 | 8/10 | 主体、风格、场景均有描述 |
| 专业性 | 7/10 | 建议补充摄影参数 |
| 可用性 | 8/10 | 结构清晰，变量定义明确，易于理解 |
| 可变性 | 9/10 | 变量划分合理，覆盖面广 |
| 双语质量 | 8/10 | 英文表达地道 |

**常见改进建议**：
- 添加 `{{lighting}}` 光照参数
- 补充 `{{camera_angle}}` 构图视角
- 建议增加 `{{mood}}` 情绪/氛围描述
- 补充 `{{render_quality}}` 提升专业性

---

## 双语翻译规则

1. **英文优先用 AI 平台通用表达**，不直译中文（如"赛博朋克少女"→ "cyberpunk girl"，而非 "cyber punk young lady"）
2. **专有名词保留英文**（如 `Ghibli Style`）
3. **描述性短语使用地道英文表达**，避免机械翻译
4. **技术参数用专业术语**（如 `f/2.8 aperture`、`bokeh effect`）

---

## 快速参考：常用变量词库

通用变量列表见 [vocabulary-banks.md](vocabulary-banks.md)，涵盖：
- 艺术风格、角色类型、服装配饰
- 场景环境、光照效果、构图视角
- 色彩方案、摄影参数、情绪氛围

---

## 示例对话

完整示例见 [examples.md](examples.md)，包含：
- 简单人像提示词拆分（模式 A）
- 复杂多段提示词拆分 + 词库（模式 B）
- PromptFill JSON 生成（模式 C）
- 英文提示词双语处理

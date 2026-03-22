---
name: prompt-engineering
description: Universal prompt engineering assistant that deconstructs complex prompts into structured variables, provides vocabulary banks for each variable, offers bilingual (Chinese/English) translation, and optionally generates PromptFill-compatible JSON templates. Use when a user wants to analyze, split, translate, or template-ify any AI image/text prompt. Trigger phrases include: "analyze prompt", "split prompt", "extract variables", "create template", "prompt variables", "词库", "提示词拆分", "提示词分析", "创建模板".
---

# Prompt Engineering - 通用提示词工程助手

将任意复杂提示词拆解为结构化变量，提供配套词库与双语翻译，可选生成 PromptFill 兼容的 JSON 模板。

---

## 工作模式

收到提示词后，**先询问用户需要哪种输出**（除非用户已明确指定）：

```
我可以提供以下服务，请选择：

A. 仅拆分与分析 — 提取变量、识别结构、给出改进建议
B. 拆分 + 词库   — 提取变量，并为每个变量生成候选词库
C. 完整模板输出  — 生成 PromptFill 兼容的 JSON 模板（含词库）

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
- 已有 `{{key}}` 或 `{{key: 内联默认值}}` → 保留并识别

**变量命名规则**：
- 小写英文 + 下划线：`art_style`、`character_type`
- 名称应描述变量语义，而非具体值
- 同义变量归并为同一 key

---

### Step 2：结构分析报告

输出标准化分析表格：

```markdown
## 📋 提示词分析

| 变量名 | 当前值/占位 | 语义类别 | 建议词库 |
|--------|------------|---------|---------|
| art_style | 赛博朋克 | 风格 | ✓ 通用词库已有 |
| character_type | 少女 | 主体 | ✓ 通用词库已有 |
| weapon | 光剑 | 道具 | ✎ 建议新建 |
| lighting | 霓虹灯光 | 光影 | ✓ 通用词库已有 |
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

为每个变量生成 **5–12 个候选词条**，格式如下：

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
| 赛博道家 | Cyber-Taoist |
| 新海诚风格 | Makoto Shinkai Style |
```

**词库生成规则**：
- 覆盖主流常见选项，兼顾小众精品
- 双语完整，英文为 AI 平台主流表达
- 选项之间具有明显差异性（避免同义重复）
- 内联默认值（`{{key: 某值}}`）中的具体词自动纳入词库

---

### Step 4：重构提示词

输出两个版本：

**结构化版本**（变量已标注）：
```
{{art_style}}风格的{{character_type}}，
手持{{weapon}}，站在{{location}}中。
{{lighting}}光线，{{camera_angle}}构图。
```

**示例填充版本**（随机词库值填充，供直接测试）：
```
赛博朋克风格的少女，
手持光剑，站在废弃工厂中。
霓虹灯光，低角度仰拍构图。
```

---

### Step 5：PromptFill JSON 输出（仅模式 C）

> 关于 PromptFill 模板格式，详见 [examples.md](examples.md)

生成完整 JSON：

```json
{
  "id": "tpl_<descriptive_name>",
  "name": { "cn": "模板中文名", "en": "Template English Name" },
  "content": {
    "cn": "{{art_style}}风格的{{character_type}}...",
    "en": "{{art_style}} style {{character_type}}..."
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
- `content`：双语提示词正文，使用 `{{variable}}` 占位
- `selections`：每个变量的默认选中值（各一个）
- `banks`：随模板附带的词库定义
- `tags`：内容主题标签（不含"图片"、"视频"等类型词）

---

## 双语翻译规则

所有输出均同时提供中英文，遵循以下原则：

1. **英文优先用 AI 平台通用表达**，不直译中文（如"赛博朋克少女"→ "cyberpunk girl"，而非 "cyber punk young lady"）
2. **专有名词保留英文**（如 `Ghibli Style`，而非"吉卜力风格"的硬译）
3. **描述性短语使用地道英文表达**，避免机械翻译
4. **技术参数用专业术语**（如 `f/2.8 aperture`、`bokeh effect`）

---

## 质量评估（可选输出）

| 维度 | 评分 | 说明 |
|------|------|------|
| 完整性 | 8/10 | 主体、风格、场景均有描述 |
| 专业性 | 7/10 | 建议补充摄影参数 |
| 可变性 | 9/10 | 变量划分合理，覆盖面广 |
| 双语质量 | 8/10 | 英文表达地道 |

**改进建议**（如有）：
- 添加 `{{lighting}}` 光照参数
- 补充 `{{camera_angle}}` 构图视角
- 建议增加情绪/氛围描述

---

## 快速参考：常用变量词库

通用变量列表见 [vocabulary-banks.md](vocabulary-banks.md)，涵盖：
- 艺术风格、角色类型、服装配饰
- 场景环境、光照效果、构图视角
- 色彩方案、摄影参数、情绪氛围

---

## 示例对话

完整示例见 [examples.md](examples.md)，包含：
- 简单人像提示词拆分
- 复杂多段提示词处理
- PromptFill JSON 生成示例

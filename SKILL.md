---
name: prompt-engineering
description: Advanced image prompt engineering assistant for Chinese-first users. Turns any input into high-quality image-generation prompts: reverse-engineers prompts from images, expands rough prompts into structured prompts, translates/transwrites prompts, extracts reusable variables, suggests phrase banks, supports minimal prompts, and optionally exports PromptFill-compatible JSON.
---

# Prompt Engineering - 高级图像提示词工程 Skill

这是一个**中文优先**的 AI 图像提示词工程 Skill。它可以把图片、粗糙想法、短关键词、中文提示词、英文提示词或中英混合输入，转成可直接用于 AI 生图工具的高质量提示词。

This is a Chinese-first image prompt engineering skill. It turns images, rough ideas, keywords, Chinese/English prompts, and mixed drafts into high-quality image-generation prompts.

核心目标不是把所有内容都强行变成复杂模板，而是先理解用户真正想要什么，再输出最适合当前场景的提示词版本。

---

## 语言与沟通规则

- 默认使用中文回答，除非用户明确要求英文或双语。
- 面向中文用户时，优先给出中文解释，同时提供可直接用于生图模型的英文提示词。
- 如果输出双语，中文用于说明和结构理解，英文用于模型执行。
- 不做机械直译，英文提示词应使用 Midjourney、Stable Diffusion、GPT Image 等图像模型常见表达。
- 用户只想要极简提示词时，不要强迫输出复杂结构。

---

## 核心原则

在写最终提示词前，先判断用户需求。

如果用户意图明确，直接执行；如果缺少的信息会显著影响结果，只问一个简短澄清问题。

默认判断：
- 用户提供图片、图片 URL 或本地图片路径时，优先按“图像反推提示词”处理。
- 用户提供短句、关键词或粗糙想法时，优先扩写成更强的图像提示词。
- 用户要求翻译、转英文、转中文或中英双语时，执行“翻译转写”，不是逐字直译。
- 用户要求变量、词组、模板、PromptFill、JSON、填空版时，提炼 `{{variable_name}}` 变量并提供词组建议。
- 用户输入极短且没有要求结构化时，先输出“极简增强版”，再可选给出“高级结构化版”。

---

## 用户需求路由

把用户请求归入以下一个或多个任务类型。

### A. 图像反推提示词 / Image To Prompt

适用场景：
- 用户上传、链接或引用一张图片。
- 用户说“反推提示词”“图生文”“看图写提示词”“img2prompt”“根据这张图写 Midjourney/SD 提示词”等。
- 用户只有参考图，但想生成提示词或可复用模板。

处理流程：
1. 观察画面要素：主体、环境、构图、镜头、光影、色彩、材质、风格、文字、氛围。
2. 区分高置信观察和推测性风格词。
3. 至少输出一段可直接使用的生图提示词。
4. 如果用户还需要模板或变量，再进入“粗糙提示词扩写”或“变量提炼”流程。

注意：不要声称可以还原图片的原始隐藏参数。应说明这是对画面的实用重构。

### B. 粗糙提示词扩写 / Rough Prompt Expansion

适用场景：
- 用户提供一句话、关键词、草稿提示词或半成品提示词。
- 用户要求“优化”“扩写”“变高级”“变专业”“结构化”“适合生图”等。

处理流程：
1. 识别主体和目标图像类型。
2. 补充真正有帮助的维度：主体细节、场景、风格、构图、光影、色彩、材质、情绪、技术质量、画幅比例、必要的负面约束。
3. 判断应该保持极简，还是升级为结构化。
4. 先输出可复制结果，再补充变量和建议。

### C. 翻译转写与变量提炼 / Translation, Transwriting, And Variables

适用场景：
- 用户要求把提示词翻译成英文或中文。
- 用户需要中英双语版本。
- 用户要求提炼变量词、填空词、候选词、词组建议、模板结构。

处理流程：
1. 保留原意，重写成更适合图像模型理解的表达。
2. 保留专有名词、风格名、品牌名、镜头术语、画幅比例和技术参数。
3. 将可复用部分提炼为 `{{variable_name}}`。
4. 为重要变量提供 5-12 个有区分度的候选词组。
5. 如果原提示词已经很好，做轻量润色，不要过度改写。

---

## 输出策略

优先输出结果，再解释原因。推荐顺序：

1. 最终提示词
2. 可选结构化版本
3. 变量与词组建议
4. 进一步优化建议

大多数情况下输出两个版本：
- **极简增强版**：简洁、直接、适合快速复制试图。
- **高级结构化版**：维度更完整，适合精细控制和复用。

如果用户明确说“只要一句”“简单点”“不要结构化”，只输出极简增强版，加一条简短建议即可。

如果用户要求 PromptFill、模板、变量、JSON 或可导入格式，才输出结构化变量和 PromptFill JSON。

---

## 提示词复杂度

### Level 1：极简版

适合短提示词、快速试图、不喜欢复杂结构的用户。

格式：

```text
[主体]，[核心风格]，[主要场景/构图]，[光影或氛围]，[质量/风格收尾]
```

示例：

```text
A cyberpunk girl in a rainy neon alley, cinematic lighting, high-detail portrait, shallow depth of field.
```

### Level 2：平衡版

默认推荐给大多数用户。

格式：

```text
[主体和关键特征], [环境], [动作或姿态], [构图/镜头], [光影], [色彩], [风格], [质量细节], [必要时加入画幅比例]
```

### Level 3：结构化版

适合用户要求高级、模板化、商业级、可复用、PromptFill-ready 的场景。

格式：

```markdown
主体：...
场景：...
构图：...
光影：...
色彩：...
风格：...
细节：...
质量：...
负面约束：...
```

---

## 图像提示词维度

扩写或反推时，从以下维度中选择必要项，不要机械塞满所有维度。

核心维度：
- `subject`：主体，人物、产品、物体、生物、地点或概念。
- `action`：动作、姿势、行为、互动。
- `scene`：场景、背景、环境。
- `style`：视觉风格、艺术流派、渲染风格、设计语言。

视觉控制：
- `composition`：版式、构图、画面布局。
- `camera_angle`：平视、低角度、俯视、特写、广角等。
- `lighting`：影棚柔光、电影感打光、霓虹灯光、黄金时刻、体积光。
- `color_scheme`：莫兰迪色、马卡龙、金红暖色、黑白高对比等。
- `mood`：宁静、戏剧化、奢华、未来感、可爱、神秘等。
- `material`：玻璃、金属、布料、木材、陶瓷、皮肤纹理、纸张颗粒等。
- `render_quality`：照片写实、超高细节、编辑大片、3D 渲染、概念艺术。
- `aspect_ratio`：1:1、16:9、9:16、4:3、3:2、21:9。

常见图像类型：
- 人像与角色设定
- 产品摄影与商业海报
- 品牌概念单品
- 建筑与城市海报
- 信息图与博物馆图鉴
- UI、图标与平面设计
- 时尚大片与杂志封面
- 漫画、插画、绘本风格
- 微缩场景与创意物体摄影
- 3D 渲染与工业设计
- 宠物、游戏、幻想、科幻与概念艺术

---

## 变量规则

只有当用户需要复用、替换、选择或做模板时，才使用变量。

语法：
- 标准变量：`{{variable_name}}`
- 内联默认值：`{{variable_name: 默认值}}`

命名规则：
- 使用小写英文和下划线。
- 变量名描述语义角色，而不是具体值。
- 好例子：`art_style`、`character_type`、`lighting`、`camera_angle`、`product_type`
- 坏例子：`cyberpunk`、`beautifulGirl`、`camera-angle`

分类：
- `character`：人物、角色、生物、身体特征、表情。
- `item`：服装、道具、配饰、产品、材质。
- `action`：动作、姿势、手势、互动。
- `location`：地点、场景、背景环境。
- `visual`：风格、色彩、光影、构图、氛围。
- `technical`：镜头、相机、画幅比例、质量、渲染参数。
- `other`：其他无法归类的内容。

提炼变量时，为重要变量提供 5-12 个候选词组。中文用户场景下，候选词组应尽量中英双语。

---

## 标准输出模板

### A. 图像反推输出

```markdown
## 图像提示词

### 极简增强版
...

### 高级结构化版
...

### 画面要素
- 主体：...
- 场景：...
- 构图：...
- 光影：...
- 色彩：...
- 风格：...

### 进一步建议
- ...
```

### B. 粗糙提示词扩写输出

```markdown
## 优化后的提示词

### 极简增强版
...

### 高级结构化版
...

### 为什么这样优化
- ...

### 进一步建议
- ...
```

### C. 翻译转写与变量输出

```markdown
## 翻译转写结果

### 中文润色版
...

### 英文生图版
...

## 变量提炼
| 变量 | 当前值 | 类别 | 候选词组 |
|---|---|---|---|
| `art_style` | ... | visual | ... |

## 词组建议
### `{{art_style}}`
- 中文 / English

## 进一步建议
- ...
```

---

## PromptFill JSON 输出

仅当用户明确要求 PromptFill、JSON、模板导出或可导入格式时输出。

结构如下：

```json
{
  "id": "tpl_descriptive_name",
  "name": { "cn": "中文模板名", "en": "English Template Name" },
  "content": {
    "cn": "{{art_style: 赛博朋克}}风格的{{character_type}}...",
    "en": "{{art_style: Cyberpunk}} style {{character_type}}..."
  },
  "imageUrl": "https://placehold.co/600x400/png?text=Template",
  "selections": {
    "art_style": { "cn": "赛博朋克", "en": "Cyberpunk" }
  },
  "tags": ["人物", "摄影"],
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

规则：
- `id` 使用 `tpl_` 前缀。
- `content` 支持 `{{variable}}` 和 `{{variable: 默认值}}`。
- `selections` 为每个变量提供一个默认值。
- `banks` 为变量提供可选词库，包含 `label`、`category`、`options`。
- `tags` 描述内容主题，不要把“图片”“视频”当作主题标签。

---

## 质量检查清单

最终输出前检查：
- 提示词是否有清晰主体。
- 输出复杂度是否符合用户真实需求。
- 风格词和技术词是否有用，而不是堆砌。
- 英文是否是自然的图像模型表达。
- 变量是否可复用，没有过度拆碎。
- 极简用户没有被迫使用复杂模板。
- 进一步建议是否具体、可执行。

---

## 进一步建议策略

在有帮助时，用简短建议结尾：
- 询问是否需要针对 Midjourney、Stable Diffusion、GPT Image、即梦、可灵等平台微调。
- 仅在平台适合时建议负面提示词。
- 建议画幅比例、风格变体或镜头变体。
- 当用户需要复用或批量迭代时，建议转换为 PromptFill 模板。
---
name: prompt-engineering
description: Advanced image prompt engineering assistant. Turns any input into high-quality image-generation prompts: reverse-engineers prompts from images, expands rough prompts into structured prompts, translates/transwrites prompts, extracts reusable variables, suggests phrase banks, supports minimal prompts, and optionally exports PromptFill-compatible JSON.
---

# Prompt Engineering - Advanced Image Prompt Skill

This skill turns almost any user input into a usable image-generation prompt. It supports image-to-prompt, rough-prompt expansion, prompt translation/transwriting, variable extraction, phrase suggestions, minimal prompts, structured prompts, and optional PromptFill JSON.

The primary goal is not to force every prompt into a complex template. The goal is to understand what the user wants, then output the strongest useful image prompt at the right level of complexity.

---

## Core Principle

Always identify the user's actual need before writing the final prompt.

If the user's intent is clear, proceed directly. If the intent is ambiguous and the missing choice changes the output significantly, ask one short clarification question.

Default assumptions:
- If the user provides an image or image URL/path, treat it as image-to-prompt unless they clearly ask for another task.
- If the user provides a short or rough idea, expand it into a stronger prompt.
- If the user provides a prompt in one language and asks for another language, translate and transwrite it for image-generation models.
- If the user asks for variables, template, PromptFill, word bank, or options, produce structured variables and phrase suggestions.
- If the input is extremely short and the user does not ask for structure, preserve a minimal version first, then offer a structured upgrade.

---

## User Need Router

Classify the request into one or more of these tracks.

### Track A: Image To Prompt

Use when:
- The user uploads, links, or references an image.
- The user asks for reverse prompt, img2prompt, image prompt, "look at this image", "describe this as a prompt", or similar.
- The user only has a reference image and wants a prompt or template.

Process:
1. Observe visible elements: subject, environment, composition, camera, lighting, color, material, style, text, mood.
2. Separate high-confidence observations from inferred style terms.
3. Output at least one directly usable image prompt.
4. If the user wants a reusable template, continue into Track B after generating the text prompt.

Do not claim to recover the original hidden generation parameters. Say the result is a practical reconstruction.

### Track B: Rough Prompt Expansion

Use when:
- The user provides a rough phrase, concept, draft prompt, keywords, or incomplete prompt.
- The user asks to optimize, expand, improve, make advanced, make professional, or make structured.

Process:
1. Identify the subject and intended image type.
2. Add only useful missing dimensions: subject details, scene, style, composition, lighting, color, material, mood, technical quality, aspect ratio, and optional negative constraints.
3. Decide whether the prompt should stay minimal or become structured.
4. Output the usable prompt first, then explain variables and options if helpful.

### Track C: Translation, Transwriting, And Variables

Use when:
- The user asks to translate a prompt.
- The user wants Chinese/English versions.
- The user wants variable words, fill-in words, alternatives, phrase suggestions, or a prompt template.

Process:
1. Translate meaning, not word order. Use natural image-model English.
2. Preserve named styles, brand names, camera terms, aspect ratios, and technical parameters when appropriate.
3. Extract reusable variables with `{{variable_name}}`.
4. Provide phrase suggestions for important variables.
5. If the original prompt is already good, keep the refined version close rather than rewriting aggressively.

---

## Output Policy

Always output the result before long analysis. Prefer this order:

1. Final prompt
2. Optional structured version
3. Variables and phrase suggestions
4. Further suggestions

For most users, produce two prompt versions:
- Minimal enhanced version: concise, direct, suitable for users who dislike complex prompts.
- Advanced structured version: richer, grouped by visual dimensions, suitable for precision control.

If the user explicitly asks for only a short prompt, output only the minimal enhanced version plus one short improvement note.

If the user asks for PromptFill, template, variables, or JSON, include structured variables and optional PromptFill-compatible JSON.

---

## Prompt Complexity Levels

### Level 1: Minimal

Use for extremely short prompts, fast ideation, or users who prefer simple prompts.

Format:

```text
[subject], [core style], [main scene/composition], [lighting or mood], [quality/style finish]
```

Example:

```text
A cyberpunk girl in a rainy neon alley, cinematic lighting, high-detail portrait, shallow depth of field.
```

### Level 2: Balanced

Use as the default for most users.

Format:

```text
[subject with key traits], [environment], [action or pose], [composition/camera], [lighting], [color palette], [style], [quality details], [aspect ratio if relevant]
```

### Level 3: Structured

Use when the user asks for advanced, template, repeatable, professional, commercial, or PromptFill-ready output.

Format:

```markdown
Subject: ...
Scene: ...
Composition: ...
Lighting: ...
Color: ...
Style: ...
Details: ...
Quality: ...
Negative constraints: ...
```

---

## Image Prompt Dimensions

When expanding or reverse-engineering prompts, choose relevant dimensions from this list. Do not force all dimensions into every prompt.

Core:
- `subject`: main person, product, object, creature, place, or concept
- `action`: pose, motion, behavior, interaction
- `scene`: location, background, environment
- `style`: visual style, art movement, rendering style, design language

Visual control:
- `composition`: layout, framing, spatial arrangement
- `camera_angle`: eye-level, low angle, bird's-eye view, close-up, wide shot
- `lighting`: studio soft light, cinematic lighting, neon lighting, golden hour, volumetric light
- `color_scheme`: muted tones, pastel palette, gold-red warm tones, black-and-white contrast
- `mood`: serene, dramatic, luxurious, futuristic, playful, mysterious
- `material`: glass, metal, fabric, wood, ceramic, skin texture, paper grain
- `render_quality`: photorealistic, ultra-detailed, editorial, 3D render, concept art
- `aspect_ratio`: 1:1, 16:9, 9:16, 4:3, 3:2, 21:9

Common image types, inspired by PromptFill templates:
- portrait and character design
- product photography and commercial poster
- brand concept object
- architecture and city poster
- infographic and museum-style diagram
- UI, icon, and graphic design
- editorial fashion and magazine cover
- comic, manga, illustration, and storybook style
- miniature scene and creative object photography
- 3D render and industrial design
- pet, game, fantasy, sci-fi, and conceptual art

---

## Variable Rules

Use variables only when the user benefits from reuse, selection, or customization.

Syntax:
- Standard variable: `{{variable_name}}`
- Inline default: `{{variable_name: default value}}`

Naming:
- Use lowercase English and underscores.
- Name the semantic role, not the specific value.
- Good: `art_style`, `character_type`, `lighting`, `camera_angle`, `product_type`
- Bad: `cyberpunk`, `beautifulGirl`, `camera-angle`

Categories:
- `character`: people, roles, creatures, body traits, expressions
- `item`: clothing, props, accessories, products, materials
- `action`: actions, poses, gestures, interactions
- `location`: places, environments, background settings
- `visual`: style, color, lighting, composition, mood
- `technical`: camera, lens, aspect ratio, quality, render settings
- `other`: anything that does not fit above

When extracting variables, include 5-12 phrase suggestions for important variables when useful. Suggestions should be meaningfully different, bilingual when the user works in Chinese and English.

---

## Standard Output Templates

### A. Image To Prompt Output

```markdown
## Image Prompt

### Minimal Version
...

### Advanced Version
...

### Observed Elements
- Subject: ...
- Scene: ...
- Composition: ...
- Lighting: ...
- Color: ...
- Style: ...

### Further Suggestions
- ...
```

### B. Rough Prompt Expansion Output

```markdown
## Enhanced Prompt

### Minimal Version
...

### Advanced Version
...

### Why This Works
- ...

### Further Suggestions
- ...
```

### C. Translation And Variables Output

```markdown
## Transwritten Prompt

### Chinese
...

### English
...

## Variables
| Variable | Current Value | Category | Suggestions |
|---|---|---|---|
| `art_style` | ... | visual | ... |

## Phrase Suggestions
### `{{art_style}}`
- 中文 / English

## Further Suggestions
- ...
```

---

## PromptFill JSON Output

Only include this when the user asks for PromptFill, JSON, template export, or importable format.

Use this shape:

```json
{
  "id": "tpl_descriptive_name",
  "name": { "cn": "中文模板名", "en": "English Template Name" },
  "content": {
    "cn": "{{art_style: 赛博朋克}}风格的{{character_type}}...",
    "en": "{{art_style: Cyberpunk}} style {{character_type}}..."
  },
  "imageUrl": "https://placehold.co/600x400/png?text=Template",
  "selections": {
    "art_style": { "cn": "赛博朋克", "en": "Cyberpunk" }
  },
  "tags": ["人物", "摄影"],
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

Rules:
- `id` starts with `tpl_`.
- `content` may use `{{variable}}` or `{{variable: inline default}}`.
- `selections` contains one default value per variable.
- `banks` contains reusable options with `label`, `category`, and `options`.
- Tags describe content themes, not media types. Do not use "image" or "video" as content tags.

---

## Quality Checklist

Before finalizing, check:
- The prompt has a clear subject.
- The output matches the user's requested complexity.
- The style and technical terms are useful, not decorative filler.
- Translation reads naturally for image-generation models.
- Variables are reusable and not over-fragmented.
- Minimal users are not forced into a heavy template.
- Further suggestions are concrete and actionable.

---

## Further Suggestions Policy

End with practical next steps when helpful:
- ask whether the user wants Midjourney, Stable Diffusion, GPT Image, or another platform tuning;
- suggest adding negative prompts only when the platform benefits from them;
- suggest aspect ratio and style variants;
- suggest turning the prompt into a PromptFill template when the user is iterating or reusing it.

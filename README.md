# Prompt Engineering Skill

面向中文用户的 AI 图像提示词工程 Skill。它可以把图片、粗糙想法、短关键词、中文提示词、英文提示词或中英混合输入，转成可直接用于生图工具的高质量提示词。

It turns images, rough ideas, keywords, Chinese/English prompts, and mixed drafts into high-quality image-generation prompts.

---

## 适合做什么

这个 Skill 重点解决三类需求：

1. **图像反推提示词**：根据参考图生成可复现画面的提示词草稿。
2. **粗糙提示词扩写**：把一句话、关键词或半成品提示词扩写成高级版本。
3. **翻译转写与变量提炼**：把提示词翻译成更适合图像模型的表达，同时提炼 `{{variable}}` 变量并提供词组建议。

输出时会优先给出可直接使用的结果，再给出变量、词组建议和后续优化方向。默认中文说明，必要时提供英文生图提示词。

---

## 设计原则

- 中文优先，适合中文用户直接使用。
- 先明确用户需求，再决定输出形式。
- 不把所有输入都强行变成复杂模板。
- 对极简提示词友好：先输出短而强的版本，再提供高级结构化版本。
- 英文提示词采用图像模型常用表达，不做机械直译。
- 支持 PromptFill 的 `{{variable}}` 与 `{{variable: 默认值}}` 语法。
- 可选导出 PromptFill 兼容 JSON。

---

## 文件结构

```text
prompt-engineering/
├── SKILL.md              # 主技能指令，安装时必须包含
├── vocabulary-banks.md   # 常用变量与双语词组参考
├── examples.md           # 使用示例
├── README.md             # 安装和使用说明
└── LICENSE               # MIT 许可证
```

公开安装时建议复制根目录这几个文件。`skills/` 目录是 Prompt Fill 内部流程沉淀，不是外部 Agent 安装必需文件。

---

## 快速安装

```bash
git clone https://github.com/TanShilongMario/PromptSkill4image.git
```

后续根据你使用的 Agent，把仓库根目录文件复制到对应的 Skill、Rule、Memory 或 Instructions 目录。

---

## 主流 Agent 安装方式

### Cursor

Cursor 原生支持 `~/.cursor/skills/<skill-name>/SKILL.md` 结构，推荐使用这种方式。

全局安装，所有项目可用：

```bash
mkdir -p ~/.cursor/skills/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md PromptSkill4image/README.md PromptSkill4image/LICENSE ~/.cursor/skills/prompt-engineering/
```

项目级安装，仅当前项目可用：

```bash
mkdir -p /your-project/.cursor/skills/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md PromptSkill4image/README.md PromptSkill4image/LICENSE /your-project/.cursor/skills/prompt-engineering/
```

使用示例：

```text
优化这个图像提示词：赛博朋克少女，雨夜街道，霓虹灯
```

### Claude Code

Claude Code 可使用项目级 `.claude/skills/<skill-name>/SKILL.md` 结构。

```bash
mkdir -p /your-project/.claude/skills/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md PromptSkill4image/README.md PromptSkill4image/LICENSE /your-project/.claude/skills/prompt-engineering/
```

如果你的 Claude Code 环境没有启用 Skills，也可以把 `SKILL.md` 放入项目文档目录，并在对话中明确引用：

```text
请阅读 prompt-engineering/SKILL.md，并按这个 Skill 帮我优化下面的图像提示词：...
```

### Windsurf

Windsurf 更常见的方式是通过项目规则或上下文文档引用。

推荐做法：

```bash
mkdir -p /your-project/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md /your-project/prompt-engineering/
```

然后在 Windsurf Rules、项目说明或对话中引用：

```text
请始终按 prompt-engineering/SKILL.md 的流程处理图像提示词需求。
```

### Aider

Aider 可通过约定文档或只读上下文文件使用。

推荐做法：

```bash
mkdir -p /your-project/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md /your-project/prompt-engineering/
```

使用时在提示中引用：

```text
Read prompt-engineering/SKILL.md and follow it when improving this image prompt:
赛博朋克少女，雨夜街道，霓虹灯
```

### GitHub Copilot / Copilot Chat

Copilot 不同环境对自定义 Skill 的支持不完全一致，最稳妥的方式是将 `SKILL.md` 放入项目文档，并在 Chat 中引用。

```bash
mkdir -p /your-project/docs/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md /your-project/docs/prompt-engineering/
```

使用时：

```text
请根据 docs/prompt-engineering/SKILL.md 的规则，帮我把这段中文提示词转成英文生图提示词，并提炼变量。
```

### ChatGPT / Claude / Gemini 等网页端

网页端通常没有文件式 Skill 安装。可以使用以下方式：

1. 打开 `SKILL.md`。
2. 将内容粘贴到自定义 GPT、Project Instructions、Claude Project Knowledge、Gemini Gems 或对话开头。
3. 再输入你的提示词需求。

推荐开场：

```text
请把下面这份 SKILL.md 作为图像提示词工程规则。之后所有“图像反推、提示词扩写、翻译转写、变量提炼”任务都按它执行。
```

### Continue / Cline / Roo Code / 其他 VS Code Agent

这些工具通常支持 Rules、Memory、Instructions 或项目上下文文件。建议采用通用目录：

```bash
mkdir -p /your-project/prompt-engineering
cp PromptSkill4image/SKILL.md PromptSkill4image/vocabulary-banks.md PromptSkill4image/examples.md /your-project/prompt-engineering/
```

然后在对应 Agent 的规则文件中加入：

```text
When the user asks for image prompt engineering, image-to-prompt, prompt expansion, translation/transwriting, variable extraction, or PromptFill JSON, read and follow prompt-engineering/SKILL.md.
```

---

## 安装兼容性说明

这个 Skill 本质上是纯 Markdown 指令，不依赖 npm、Python 包或外部服务。因此它可以被大多数 Agent 通过以下任一方式使用：

- 原生 Skills 目录：如 Cursor、Claude Code。
- 项目规则或记忆：如 Windsurf、Continue、Cline、Roo Code。
- 文档上下文引用：如 Aider、Copilot Chat。
- 自定义系统提示词：如 ChatGPT、Claude、Gemini 网页端。

最低要求：

- Agent 能读取或粘贴 `SKILL.md`。
- 如需词库建议，最好同时提供 `vocabulary-banks.md`。
- 如需示例学习，最好同时提供 `examples.md`。

---

## 使用示例

### 图像反推提示词

```text
请根据这张参考图反推一段可用于 Midjourney 的英文提示词，并给一个简洁版和高级版。
```

输出将包含：

- 画面摘要
- 极简增强版提示词
- 高级结构化提示词
- 可选变量与风格建议

### 粗糙提示词扩写

```text
帮我优化：赛博朋克少女，雨夜街道，霓虹灯
```

输出将包含：

- 可直接复制的短提示词
- 更专业的结构化提示词
- 可以继续调整的维度，如镜头、光影、色彩、材质、比例

### 翻译转写并提炼变量

```text
把这段中文提示词转成英文，并提炼可替换变量和词组建议：
古风仙女，站在云海中，丁达尔光线，梦幻氛围
```

输出将包含：

- 中文润色版
- 英文生图版
- `{{character_type}}`、`{{location}}`、`{{lighting}}`、`{{mood}}` 等变量
- 每个变量的中英文候选词组

### PromptFill 模板

```text
把这段提示词做成 PromptFill 模板 JSON：
古风仙女，站在云海中，丁达尔光线，梦幻氛围
```

输出将包含可导入 PromptFill 的 JSON，含 `content`、`selections`、`banks` 和双语词库。

---

## 输出模式

默认情况下，Skill 会根据输入自动选择合适复杂度：

- **极简增强版**：适合快速复制、试图、社交平台和不想使用复杂结构的用户。
- **平衡增强版**：适合大多数图像生成需求，包含主体、场景、构图、光影、风格和质量描述。
- **高级结构化版**：适合商业海报、产品摄影、角色设定、建筑图、信息图、可复用模板等精细控制场景。
- **PromptFill JSON**：仅在用户明确要求模板、JSON、PromptFill 或可导入格式时输出。

---

## 参考变量类别

`vocabulary-banks.md` 收录常用变量和双语词组，覆盖：

- 视觉风格：`art_style`、`color_scheme`、`lighting`、`mood`
- 主体角色：`character_type`、`hair_style`、`expression`
- 服装道具：`outfit`、`accessory`、`weapon`
- 场景环境：`location`、`background`
- 摄影技术：`camera_angle`、`shot_type`、`render_quality`、`aspect_ratio`

---

## PromptFill 兼容性

支持以下变量语法：

```text
{{variable_name}}
{{variable_name: 默认值}}
```

PromptFill JSON 输出遵循：

- `id` 使用 `tpl_` 前缀。
- `content` 支持中英文双语。
- `selections` 为每个变量提供默认选中值。
- `banks` 为每个变量提供 `label`、`category`、`options`。
- `tags` 描述内容主题，不使用“图片”“视频”这类媒介类型词。

---

## 发布建议

如果你准备把本仓库作为公开 Skill 发布，建议至少包含：

- `SKILL.md`
- `README.md`
- `vocabulary-banks.md`
- `examples.md`
- `LICENSE`

不建议默认发布内部开发资料：

- `skills/`
- 临时测试文件
- 与 Prompt Fill 主应用内部实现强绑定的示例或草稿

---

## 许可证

MIT License. 详见 `LICENSE`。

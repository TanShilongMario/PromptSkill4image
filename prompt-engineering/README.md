# Prompt Engineering Skill

**通用 AI 提示词工程助手 Skill** | A universal AI prompt engineering assistant skill

将复杂提示词拆解为结构化变量，生成双语词库，可选输出 PromptFill 兼容的 JSON 模板。

Deconstruct complex prompts into structured variables, generate bilingual vocabulary banks, and optionally output PromptFill-compatible JSON templates.

---

## 功能介绍 | Features

| 功能 | Feature | 说明 |
|------|---------|------|
| 提示词拆分 | Prompt Deconstruction | 识别所有可变部分，统一标注为 `{{variable}}` |
| 变量分类 | Variable Analysis | 按语义分类（主体/道具/场景/视觉/技术参数） |
| 词库生成 | Vocabulary Banks | 每个变量生成 5–12 个双语候选词条 |
| 双语翻译 | Bilingual Output | 同时输出中文和英文版本的结构化提示词 |
| 模板导出 | PromptFill JSON | 可选生成可导入 [PromptFill](https://www.aipromptfill.com) 的完整 JSON |
| 质量评估 | Quality Evaluation | 从完整性、专业性、可变性三个维度评分 |

---

## 安装方式 | Installation

本 Skill 以 Markdown 文件形式提供，**兼容所有支持外部 Skill/规则加载的 AI Agent**，无需额外依赖。

This skill is provided as plain Markdown files and is **compatible with any AI agent that supports external skill/rule loading**.

---

### Cursor

**个人全局安装**（所有项目均可使用）：

```bash
# 克隆仓库
git clone https://github.com/TanShilongMario/PromptSkill4image.git

# 复制到 Cursor 全局 skills 目录
cp -r PromptSkill4image/prompt-engineering ~/.cursor/skills/
```

**项目级安装**（仅当前项目，可随仓库共享）：

```bash
cp -r PromptSkill4image/prompt-engineering /your-project/.cursor/skills/
```

> 注意：请勿放入 `~/.cursor/skills-cursor/`，该目录为 Cursor 内置保留目录。

---

### Claude Code (claude.ai/code)

将 Skill 文件放入项目的 `.claude/skills/` 目录，Claude Code 会自动识别：

```bash
git clone https://github.com/TanShilongMario/PromptSkill4image.git

# 进入你的项目
cd /your-project

# 创建 skills 目录（如不存在）
mkdir -p .claude/skills

# 复制 skill
cp -r PromptSkill4image/prompt-engineering .claude/skills/
```

之后在对话中提及"分析提示词"或"prompt engineering"即可触发。

---

### 其他 AI Agent（通用方式）

对于 **Windsurf、Copilot Workspace、Aider** 等其他 AI 编码工具，将整个 `prompt-engineering/` 文件夹放入项目中，并在需要时手动引用：

```bash
cp -r PromptSkill4image/prompt-engineering /your-project/
```

然后在对话中直接引用文件：
```
请阅读 prompt-engineering/SKILL.md 并按其指引帮我分析这段提示词：
[你的提示词]
```

---

### 直接使用（无需安装）

你也可以直接将 `SKILL.md` 的内容粘贴到任何 AI 对话窗口的系统提示词或上下文中使用，无需安装任何工具。

You can also paste the contents of `SKILL.md` directly into any AI chat as a system prompt — no installation required.

---

## 使用方式 | Usage

安装后，当你说出以下内容时会自动触发：

- `分析提示词` / `提示词拆分` / `拆分这段提示词`
- `analyze this prompt` / `split this prompt` / `extract variables`
- `生成词库` / `create template` / `提示词变量`

### 三种输出模式

首次使用时，助手会询问你需要哪种输出（也可直接指定）：

**模式 A — 仅分析**

```
分析这段提示词：[你的提示词]
```
输出：变量列表、结构分析、改进建议

**模式 B — 拆分 + 词库**（推荐）

```
拆分这段提示词并生成词库：[你的提示词]
```
输出：结构化提示词（含 `{{变量}}`）、每个变量的双语词库、示例填充版本

**模式 C — 完整 PromptFill 模板**

```
帮我把这段提示词生成 PromptFill 模板：[你的提示词]
```
输出：模式 B 的全部内容 + 可导入 PromptFill 的完整 JSON

---

## 文件结构 | File Structure

```
prompt-engineering/
├── SKILL.md              # 主技能指令（AI 读取此文件）
├── vocabulary-banks.md   # 15+ 常用变量类别的通用词库参考
├── examples.md           # 4 个完整使用示例
├── README.md             # 本文件
└── LICENSE               # MIT 许可证
```

---

## 内置词库覆盖 | Vocabulary Coverage

详见 [`vocabulary-banks.md`](vocabulary-banks.md)，涵盖：

| 类别 | 变量示例 |
|------|---------|
| 视觉风格 | 艺术风格、色彩方案、光照效果、情绪氛围 |
| 主体角色 | 角色类型、发型、表情神态 |
| 服装道具 | 服装风格、配饰、武器 |
| 场景环境 | 场景地点、背景环境 |
| 摄影技术 | 构图视角、景别、渲染质量、画幅比例 |

---

## PromptFill 兼容性

模式 C 输出的 JSON 遵循 [PromptFill](https://www.aipromptfill.com) 模板格式：

- `{{variable}}` — 标准占位符
- `{{variable: 默认值}}` — 带内联默认值（V1.1 语法）
- 所有用户可见文本使用 `{ cn: "中文", en: "English" }` 双语格式
- JSON 中内嵌 `banks` 字段定义完整词库

---

## 示例 | Examples

见 [`examples.md`](examples.md)，包含：

1. 简单人像提示词分析（模式 A）
2. 复杂多元素提示词拆分与词库生成（模式 B）
3. 完整 PromptFill JSON 生成（模式 C）
4. 英文提示词处理与双语输出

---

## 贡献 | Contributing

欢迎贡献新词库或示例！

1. Fork 本仓库
2. 在 `vocabulary-banks.md` 中添加新变量词库
3. 在 `examples.md` 中补充使用示例
4. 提交 Pull Request

词库格式要求：每个变量 5–12 个选项，中英文均完整，选项之间语义差异明显。

---

## 许可证 | License

MIT License — 详见 [LICENSE](LICENSE)

---

## 致谢 | Acknowledgments

本 Skill 基于 AI 图像生成提示词的实践经验构建，适用于 [Midjourney](https://midjourney.com)、[Stable Diffusion](https://stability.ai)、[即梦](https://jimeng.jianying.com)、[可灵](https://kling.kuaishou.com) 等主流 AI 创作平台。

PromptFill JSON 格式专为 [PromptFill](https://www.aipromptfill.com) 应用设计。

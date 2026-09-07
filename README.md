# 大数据与人工智能 · 个人课程仓库

> 「大数据与人工智能」课程的**个人学习仓库**：作业、笔记、可复用 Skill 三位一体。
> 仓库地址：<https://github.com/zhiji-bigdate/bigdata-ai-course>

---

## 一、仓库用途

这个仓库是我的**个人学习作品集 + 课程作业提交点**，承担三个角色：

1. **作业归档**：每个作业放在 `assignments/` 下，子目录里有独立的 `README.md` 说明本次作业的目标、交付物、核查记录。
2. **学习笔记**：课程中遇到的概念、对比、复习材料放在 `notes/` 下，配 Markdown + HTML 双版本。
3. **可复用 Skill**：把"做某类任务的最佳实践"沉淀到 `.workbuddy/skills/`，下次直接由 AI 自动加载。

---

## 二、目录结构

```
bigdata-ai-course/
├── README.md                          # 本文件（仓库总览）
├── .gitignore                         # 已排除：__pycache__、.env、敏感凭据等
├── .workbuddy/
│   └── skills/
│       └── concept-learning/          # 项目级 Skill
│           └── SKILL.md               # 概念学习资料生成 Skill
├── assignments/                       # 作业
│   └── assignment-01/                 # 作业 01：概念学习 Skill 创建 + 3 概念学习
│       ├── README.md                  # 本次作业说明
│       └── ai-usage-audit.md          # AI 使用核查报告（学术规范要求）
└── notes/                             # 课程笔记
        └── concepts/                   # 概念学习材料
            ├── agent.md + agent.html              # Agent 学习资料
            ├── llm-context.md + llm-context.html  # 大模型的上下文 学习资料
            ├── skill.md + skill.html              # Skill 学习资料
            └── concept-relationship.md            # 三者关系图（含 Mermaid）
```

---

## 三、项目级 Skill（核心资产）

| 项 | 值 |
|----|----|
| **名称** | `concept-learning` |
| **存放路径** | [`.workbuddy/skills/concept-learning/SKILL.md`](./.workbuddy/skills/concept-learning/SKILL.md) |
| **类型** | 项目级 Skill（随仓库 clone 走，换机器依然可用） |
| **作用** | 接收任意**单个概念名**，按固定 8 节结构生成完整学习资料（Markdown + 单文件 HTML 预览） |

### 8 节结构规范

1. **学习目标** — 3-5 条动作型目标（理解/能举例/能区分……）
2. **核心问题** — 3-5 个引导性问题，带着问题读
3. **概念地图** — 30 秒看懂的全局图
4. **结构化解释** — 定义 + 类比 + 机制 + 边界误区（4 小节）
5. **应用案例** — 1 个真实场景 + 1 个动手练习
6. **概念辨析** — 与相邻概念的对比
7. **自测问题** — 3 道可点开看提示的复盘题
8. **参考来源** — 3-5 条带"为什么推荐"的延伸阅读（要求真实可核查）

### 在 WorkBuddy 中调用

打开这个仓库所在的 WorkBuddy 对话，直接说：

| 自然语言 | Skill 行为 |
|----------|-----------|
| 「学一下 RAG」 | 生成 `notes/concepts/rag.md` + `rag.html` |
| 「给我讲讲 Transformer」 | 生成 `notes/concepts/transformer.md` + `transformer.html` |
| 「学习 Embedding 这个概念」 | 生成 `notes/concepts/embedding.md` + `embedding.html` |
| 「整理 Fine-tuning 的学习材料」 | 生成 `notes/concepts/fine-tuning.md` + `fine-tuning.html` |

工作原理：WorkBuddy 检测到触发关键词 → 自动加载 `.workbuddy/skills/concept-learning/SKILL.md` → 按 8 节规范生成文件。

### Skill 不能做的事（边界）

- ❌ 不做**跨概念对比**（用单独的对比文档 `concept-relationship.md`）
- ❌ 不写代码、不调外部 API
- ❌ 不生成图片或视频
- ❌ 不伪造引用（不存在的论文标题、URL 必须标 `[未验证]`）

---

## 四、已生成的学习资料

放在 [`notes/concepts/`](./notes/concepts/) 下，每份含**个人解释 + 核心机制 + 应用场景 + 混淆问题 + 可核查来源**：

| 概念 | Markdown | HTML 预览 | 8 节齐全 |
|------|----------|-----------|----------|
| Agent | [agent.md](./notes/concepts/agent.md) | [agent.html](./notes/concepts/agent.html) | ✅ |
| 大模型的上下文 | [llm-context.md](./notes/concepts/llm-context.md) | [llm-context.html](./notes/concepts/llm-context.html) | ✅ |
| Skill | [skill.md](./notes/concepts/skill.md) | [skill.html](./notes/concepts/skill.html) | ✅ |

**附加**：3 者关系图（含 Mermaid）→ [concept-relationship.md](./notes/concepts/concept-relationship.md)

---

## 五、AI 使用核查（学术规范要求）

本次所有内容均使用 AI 协助起草，但**不是整段照搬 AI 对话**。我做了以下人工核查：

| 核查项 | 结果 |
|--------|------|
| 3 份概念资料的"结构化解释" | ✅ 原创整理 + 重新组织 + 日常生活类比，非整段照搬 |
| 15 条引用资料链接 | ✅ 10 条已通过搜索引擎核实，5 条已改为更宽泛的概称 |
| 2 条不准确引用（Simon Willison 标题、Lilian Weng 标题） | ❌ → ✅ 已替换为已验证存在的标题 |
| 不能核实的内容 | ⚠️ 明确标注 `[未验证]`，不伪造 |

**完整核查报告**：[`assignments/assignment-01/ai-usage-audit.md`](./assignments/assignment-01/ai-usage-audit.md)（5003 字节，含每条核查记录）

> 这份报告按作业规范 "**可以使用 AI 协助设计 Skill、编写 Git 命令和整理资料，但必须阅读、理解并核查 AI 生成的内容；资料来源不得伪造，概念解释不得整段照搬 AI 对话结果**" 撰写。

---

## 六、安全与版本控制

| 项 | 措施 |
|----|------|
| 远程版本控制 | 已通过 `git push` 推送到 GitHub（commit `cd0bce7`） |
| **API Key / 密码** | ❌ **绝不上传** |
| **个人隐私** | ❌ **绝不上传** |
| 敏感文件排除规则 | 见 [`.gitignore`](./.gitignore)（已排除 `.env`、`*.key`、`*.pem`、私钥等） |
| Token 处理 | GitHub PAT 仅用于本机 git 推送，已提醒可随时到 GitHub 撤销 |

---

## 七、克隆 & 本地体验

```bash
# 克隆
git clone https://github.com/zhiji-bigdate/bigdata-ai-course.git
cd bigdata-ai-course

# 打开任意概念 HTML 预览（双击或命令行）
start notes/concepts/agent.html        # Windows
open notes/concepts/agent.html         # macOS

# 在 WorkBuddy 中体验 Skill
# → 在这个仓库所在的对话里说："学一下 RAG"
```

---

## 八、作业列表

- ✅ **作业 01**：项目级 Skill 创建 + Agent / 大模型的上下文 / Skill 三概念学习
  - 详见 [`assignments/assignment-01/`](./assignments/assignment-01/)

---

_最后更新：2026-09-07_
# 作业 01：AI 基础概念学习资料生成

## 作业信息

- **作业名称**：概念学习资料生成 Skill 创建 + 应用
- **课程**：大数据与人工智能
- **GitHub 仓库**：https://github.com/zhiji-bigdate/bigdata-ai-course
- **完成时间**：2026-09-07

## 作业目标

1. 在 GitHub 上创建个人课程仓库（已完成，见上）
2. 在 WorkBuddy 中打开本地仓库
3. 在仓库内创建一个「概念学习资料生成 Skill」
4. 将该 Skill 建立为**项目级 Skill**，保存到 `.workbuddy/skills/`
5. 调用 Skill 学习 3 个概念：**Agent**、**大模型的上下文**、**Skill**
6. 提交 Skill + 学习资料 + 作业说明到 GitHub

## 交付物

本作业的交付物都已经在 GitHub 仓库的 `main` 分支上，可以直接访问查看：

### 1. 项目级 Skill

**路径**：[`.workbuddy/skills/concept-learning/SKILL.md`](../../.workbuddy/skills/concept-learning/SKILL.md)

这是一个「概念学习资料生成」Skill。它接受任意一个概念名（"Agent"、"RAG"、"Transformer" 之类），自动生成一份 **8 节结构**的完整学习资料：

| 章节 | 作用 |
|------|------|
| 1. 学习目标 | 告诉读者"学完能做什么" |
| 2. 核心问题 | 引导读者带着问题阅读 |
| 3. 概念地图 | 一张 30 秒看懂的全局图 |
| 4. 结构化解释 | 定义 + 类比 + 机制 + 误区 |
| 5. 应用案例 | 真实场景 + 课堂练习 |
| 6. 概念辨析 | 与相邻概念的对比表 |
| 7. 自测问题 | 3 道可点开查看提示的复盘题 |
| 8. 参考来源 | 3-5 个带"为什么推荐"的延伸阅读 |

**Skill 的设计要点**（写在 SKILL.md 里）：
- 自动按概念名生成 slug（如 `Agent` → `agent`）
- 输出到 `notes/concepts/<slug>.md`（Markdown）+ `<slug>.html`（单文件 HTML 预览）
- 自带 9 项自检清单，缺项必须补齐才能交付
- YAML 描述里写明触发关键词（"学一下 X"、"讲讲 X"），AI 看到会自动加载

### 2. 三个概念的学习资料

都放在 [`notes/concepts/`](../../notes/concepts/) 下，每个概念包含一份 Markdown 和一份配套 HTML 预览：

| 概念 | Markdown | HTML 预览 |
|------|----------|-----------|
| Agent | [agent.md](../../notes/concepts/agent.md) | [agent.html](../../notes/concepts/agent.html) |
| 大模型的上下文 | [llm-context.md](../../notes/concepts/llm-context.md) | [llm-context.html](../../notes/concepts/llm-context.html) |
| Skill | [skill.md](../../notes/concepts/skill.md) | [skill.html](../../notes/concepts/skill.html) |

**每份资料都严格遵循 8 节结构**，并通过 Skill 内置的 9 项自检清单。

### 3. 本目录（作业说明）

**本文件**（`assignments/assignment-01/README.md`）就是作业说明，告诉老师/同学：
- 这次作业做了什么
- 关键交付物的位置
- 怎么打开看

## 如何查看作业

1. **打开 GitHub 仓库**：https://github.com/zhiji-bigdate/bigdata-ai-course
2. **进入目录**：
   - 概念学习资料：`notes/concepts/`（双击 `xxx.html` 即可预览）
   - Skill 定义：`.workbuddy/skills/concept-learning/SKILL.md`
3. **本地体验 Skill**：
   - 克隆仓库后用 VS Code 打开
   - 在 WorkBuddy 里说"学一下 RAG" → Skill 自动触发 → 生成新概念资料

## 心得小结

这次作业让我体会到 3 个关键点：

1. **Skill 不只是 prompt**：一个好的 Skill 应该像产品文档一样，把"什么时候用、怎么用、不能做什么"讲清楚。
2. **8 节结构 vs 5 节结构的差别**：多出的"学习目标、核心问题、应用案例、概念辨析、参考来源"5 节，把一份笔记从"说明文"升级成了"完整学习材料"。
3. **元数据是 Skill 的灵魂**：`description` 字段决定了 AI 何时会"想到"这个 Skill。写得模糊就会触发不到，写得太宽又会被滥用。

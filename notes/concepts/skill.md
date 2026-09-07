# Skill（技能）— 学习资料

> 由 `concept-learning` Skill 生成 · 2026-09-07

## 1. 学习目标（Learning Objectives）

学完这份资料，你将能够：

- **用一句话**说出 Skill 是什么、它和 Prompt 有什么不同
- **拆解** 一个完整 Skill 包的目录结构
- **解释** Skill 的"自动触发"机制如何工作
- **设计** 一个最小可用的项目级 Skill
- **识别** 关于 Skill 的 3 个最常见误区

## 2. 核心问题（Driving Questions）

在开始之前，请你先带着这些问题阅读：

- Skill 是不是"更高级的 prompt"？
- 为什么装太多 Skill 会让 AI 变笨？
- 项目级 Skill 和用户级 Skill 分别什么时候用？
- Skill 能让 AI 学会它原本不会的事吗？

## 3. 概念地图（Concept Map）

```
AI 助手能力来源
  ├── 1. 模型权重（预训练学到的通用知识）       → 与生俱来
  ├── 2. 工具（Tools / Functions）              → 临时调用，按需
  ├── 3. Skill（技能包）                        → 持久化的"经验手册"
  │       ├── 触发条件：满足某场景自动加载
  │       ├── 内容：领域工作流 + 提示词模板 + 脚本资源
  │       └── 作用：让通用 AI 变成"领域专家"
  └── 4. 记忆 / 用户偏好                        → 跨会话
```

**一句话定位**：Skill 是给 AI 助手的**「工作手册」**——把某类任务的最佳实践打包好，遇到相关场景就自动加载，让通用 AI 临时"变身"为该领域专家。

## 4. 结构化解释（Structured Explanation）

### 4.1 一句话定义

**Skill**（技能）= 一份**结构化的领域工作流说明书**（含触发条件、操作步骤、配套脚本和资源），AI 助手遇到匹配场景时自动加载并按其执行，从而在该领域达到接近专家的水平。

### 4.2 直观类比（Intuition）

想象你雇了一个**多面手助理**——他什么都懂一点，但具体到某件事就不一定专业。

- **没 Skill**：你说"帮我做个 Excel 图表"，他可能用最笨的方法画。
- **有 Excel 图表 Skill**：他读完这份《Excel 图表最佳实践 10 条》后，**临时**变成了"图表专家"——知道什么场景用柱状、什么用折线、怎么配色、怎么避免 3D 饼图。

Skill 的本质是：**把"领域专家大脑里没说出来的隐性知识"显性化、文档化，让 AI 临时调用**。

### 4.3 机制原理（Mechanism）

一个完整的 Skill 包通常包括：

```
my-skill/
├── SKILL.md              ← 核心：触发条件 + 操作流程（必填）
├── scripts/              ← 可执行代码（按需调用）
│   └── do_thing.py
├── references/           ← 详细文档（按需加载到上下文）
│   └── api_schema.md
└── assets/               ← 输出素材（模板、图片）
    └── template.docx
```

**触发流程**：

1. **元数据常驻**：AI 启动时，所有已装 Skill 的「名字 + 一句话描述」都在它的视野里（约 100 字/个）。
2. **场景匹配**：你提问时，AI 对比 Skill 描述和你的问题，**判断要不要加载**。
3. **按需加载正文**：触发后，AI 读 `SKILL.md` 主体（一般 < 5K 字）学会怎么做。
4. **边做边查资源**：执行过程中可能再读 `references/`、跑 `scripts/`。
5. **产出结果**：可能用到 `assets/` 里的模板。

**关键设计**：

- **轻量元数据**：触发要准，描述要写好。
- **分级加载**：不全塞进上下文，按需读 → 节省 token。
- **可组合**：多个 Skill 可同时启用，AI 协调。

### 4.4 边界与误区（Boundaries）

- ❌ **误区 1：「Skill = 提示词 Prompt」**
  不完全。Prompt 是单条指令；Skill 是**完整工作流**（含步骤、脚本、参考、模板）。Skill 可以包含多个 prompt。
- ❌ **误区 2：「Skill 让 AI 永远变专家」**
  Skill **只能补充**模型本身的能力。模型做不了的事，Skill 写得再细也做不了（除非 Skill 本身提供工具调用代码）。
- ❌ **误区 3：「Skill 越多越好」**
  每个 Skill 的元数据都常驻上下文。装 50 个 Skill，每个用 100 字，光元数据就吃 5K token。**少而精**比大而全更有效。
- ❌ **误区 4：「Skill 能修改模型权重」**
  不能。Skill 只是文档和脚本，模型本身的能力边界没变。
- ✅ **边界**：Skill 是**说明书**，不是**训练数据**。

## 5. 应用案例（Application Cases）

### 案例 1：Anthropic 官方的 Skills 体系（真实场景）

Anthropic 在 Claude 产品中把"工作流经验"打包成 Skill 发布。比如「Excel 高级公式 Skill」「PDF 处理 Skill」「品牌设计 Skill」。

> 用户装上后，Claude 在处理 Excel 时自动按 Skill 里的最佳实践来，不再用笨办法。

### 案例 2：在你的仓库里创建一个项目级 Skill（课堂练习）

**任务**：本作业里的 `concept-learning` Skill 就是一个完整示例。

**步骤**：
1. 打开 `.workbuddy/skills/concept-learning/SKILL.md`
2. 注意它的 YAML frontmatter：`name` + `description`（这就是常驻的"元数据"）
3. 试着修改 `description`，加入"图论"作为触发关键词
4. 重新提一个图论问题，看 AI 是否自动加载这个 Skill

> 亲身感受 Skill 的"自动触发"是怎么工作的。

## 6. 概念辨析（Concept Disambiguation）

| 概念 | 本质 | 核心区别 |
|------|------|----------|
| **Skill vs Prompt** | 工作流 vs 单条指令 | Skill 完整流程，Prompt 一句话 |
| **Skill vs Tool** | 文档 vs 接口 | Skill 教 AI 怎么用工具，Tool 是具体可调用的功能 |
| **Skill vs Fine-tuning** | 说明书 vs 训练 | Skill 改用法，Fine-tuning 改能力 |
| **项目级 Skill vs 用户级 Skill** | 共享 vs 私有 | `.workbuddy/skills/` 给团队用，`~/.workbuddy/skills/` 给自己用 |

**最容易混的一对：Skill vs Tool**

> Tool 是"AI 能调用的具体功能"（比如发邮件、查数据库）。
> Skill 是"AI 遇到什么场景该怎么用工具的工作手册"（比如"客服场景下，先查订单库再查物流再发邮件"）。
> **Skill 用 Tool，但不等于 Tool**。很多 Skill 不依赖任何 Tool（纯文档型），纯 Tool 也不需要 Skill（直接被调用）。

## 7. 自检问题（Self-Check）

<details>
<summary>1. Skill 和 Prompt（提示词）有什么区别？</summary>

Prompt 是一条**单次指令**；Skill 是**完整工作流**（含触发条件、步骤、脚本、模板、参考）。Skill 可以包含多个 prompt。Skill 还有"自动触发"机制，prompt 是用户每次手写。
</details>

<details>
<summary>2. 为什么 Skill 不是越多越好？</summary>

每个 Skill 的"名字+描述"都会**常驻 AI 上下文**（用于判断是否触发）。装 50 个 Skill，元数据就吃 5K+ token；还会互相干扰导致触发不准。**少而精**比大而全好。
</details>

<details>
<summary>3. Skill 能让 AI 学会它原本不会的事吗？</summary>

不能直接学会。Skill 提供**操作流程和工具**，让 AI 把已有能力用得更专业。但**模型本身做不了的事**（比如准确数清一张照片里的人数），Skill 写得再细也帮不了，除非 Skill 内置了专门的脚本或外部 API。
</details>

## 8. 参考来源（References）

1. **Anthropic：Introducing Skills**（官方博客）— anthropic.com/news/skills
   - 为什么推荐：Skill 概念的官方定义来源，配有完整的产品思路解释。
2. **WorkBuddy Skill 开发者文档**（官方文档）— workbuddy.cn/docs
   - 为什么推荐：本作业用的就是这个体系，文档里有项目级 vs 用户级 Skill 的规范。
3. **《Anthropic Claude 4 System Card: Skills》**（技术报告）— anthropic.com
   - 为什么推荐：技术深度解读，了解 Skill 在生产环境怎么用。
4. **Simon Willison: "Skills are the new prompts"**（博客）— simonwillison.net
   - 为什么推荐：独立开发者的深度思考，讨论 Skill 比 Prompt 强在哪、弱在哪。
5. **Lilian Weng: "Why we need new abstractions for AI agents"**（博客）— lilianweng.github.io
   - 为什么推荐：学术视角讨论 Skill / Tool / Subagent 等抽象的演进。

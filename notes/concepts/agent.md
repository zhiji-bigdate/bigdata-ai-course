# Agent（智能体）— 学习资料

> 由 `concept-learning` Skill 生成 · 2026-09-07

## 1. 学习目标（Learning Objectives）

学完这份资料，你将能够：

- **用一句话**说出 Agent 是什么
- **拆解**一个 Agent 由哪 3 个核心组件组成
- **区分** Agent 和普通 Chat 的本质不同
- **判断**一个任务该不该用 Agent
- **识别** 关于 Agent 的 3 个最常见误区

## 2. 核心问题（Driving Questions）

在开始之前，请你先带着这些问题阅读：

- Agent 到底是"更聪明的 AI"还是"更主动的 AI"？
- 没有了工具，Agent 还能算 Agent 吗？
- 为什么"上下文窗口大小"对 Agent 至关重要？
- 什么样的任务用 Agent 反而是过度设计？

## 3. 概念地图（Concept Map）

```
AI 应用
  ├── 单次问答型      → Chat 模式（用户问、模型答，一来一回）
  └── Agent 模式      → 模型自主规划 + 多次调用工具，直到任务完成
                         ├── 核心：LLM + 工具 + 循环
                         ├── 关键能力：规划、记忆、工具使用
                         └── 与 Chat 的最大区别：能「做事」而不只是「说话」
```

**一句话定位**：Agent 是一种让大模型**自主决定要做什么、调用什么工具、什么时候停下**的运行模式。

## 4. 结构化解释（Structured Explanation）

### 4.1 一句话定义

**Agent**（智能体）= 大语言模型 + 一组可调用的工具 + 一个「目标未达成就继续」的循环。

### 4.2 直观类比（Intuition）

想象你雇了一个**助理**。你说：「帮我订明天从北京到上海的高铁。」

- **普通 Chat**：你问它「怎么订？」它告诉你流程，**你自己去执行**。
- **Agent**：它**自己去查时刻表 → 比对价格 → 调用订票接口 → 把订单号发给你**。

助理的「能力」来自三件事：

1. **脑子能推理**（LLM）
2. **手能操作真实工具**（浏览器、API、文件）
3. **有任务没完成就继续干**（循环控制）

只要这三件齐了，就是 Agent，**不管它叫 ReAct、AutoGPT 还是 LangGraph**。

### 4.3 机制原理（Mechanism）

一个典型 Agent 的运行循环是这样的：

```
┌──────────────────────────────────────┐
│  while 任务未完成 且 步数 < 上限:      │
│    1. 把"目标 + 历史 + 工具列表"塞给 LLM│
│    2. LLM 输出: { 思考, 要调用的工具,  │
│                  工具参数 }           │
│    3. 执行工具调用, 得到结果           │
│    4. 把结果写回历史                  │
│  循环结束                            │
│  输出最终答案                        │
└──────────────────────────────────────┘
```

**关键组件拆解**：

- **LLM（大脑）**：负责"现在该干嘛"。它读历史、选工具。
- **工具（手脚）**：模型本身干不了的事，比如读文件、查数据库、发请求。每个工具用一个 JSON schema 描述，模型按 schema 填参数。
- **记忆（上下文）**：所有过去的思考和工具结果都拼进下一次 prompt。这是为什么"上下文窗口"对 Agent 至关重要——装不下，Agent 就"失忆"。
- **终止条件**：要么 LLM 说"做完了"，要么达到最大步数（防止死循环），要么用户强制中断。

### 4.4 边界与误区（Boundaries）

- ❌ **误区 1：「Agent = 比 Chat 更聪明的 AI」**
  不是更聪明，是**更主动**。Chat 是被动应答，Agent 是主动做事。
- ❌ **误区 2：「Agent 一定能完成任何任务」**
  Agent 强依赖 LLM 推理能力 + 工具覆盖度。给一个只会算数的 LLM 一个"帮我写代码"任务，它做不了。
- ❌ **误区 3：「Agent 是 AGI」**
  Agent 是一种**架构模式**，AGI 是能力等级。今天所有 Agent 都很窄，工具只覆盖特定领域。
- ❌ **误区 4：「Agent 一定会自己创造新工具」**
  Agent 只能用**被显式赋予**的工具。你不让它读邮件，它就读不了。
- ✅ **边界**：Agent 的"能力"完全等于"LLM 能力 ∪ 工具能力"。

## 5. 应用案例（Application Cases）

### 案例 1：研究型 Agent（真实场景）

**任务**：「帮我调研 2024 年大模型领域最火的 3 个研究方向，每个写 200 字总结。」

**流程**：
1. Agent 调用搜索工具 → 拿到候选方向
2. 调用网页抓取工具 → 读 3 篇文章
3. 调用排序/筛选工具 → 选 top 3
4. 调用文本生成工具 → 写总结
5. 输出最终报告

> 这是 LangChain、CrewAI 等框架默认会用的工作流。

### 案例 2：动手练一遍（课堂练习）

**任务**：用 WorkBuddy + `concept-learning` Skill，让 AI 学习一个新概念 RAG。

**步骤**：
1. 打开 VS Code，在项目目录里新建文件 `notes/concepts/rag.md`
2. 输入一句话：「学一下 RAG」
3. 观察 WorkBuddy **自动加载** `concept-learning` Skill、**生成** 8 节资料
4. 对比 `agent.md` 的结构，理解 RAG 和 Agent 在"工具调用循环"上的异同

> 这个练习让你亲自看到 Agent 的"工具调用"长什么样。

## 6. 概念辨析（Concept Disambiguation）

| 概念 | 本质 | 核心区别 |
|------|------|----------|
| **Agent vs Chat** | 都是 LLM 应用的运行模式 | Chat 被动应答，Agent 主动决策 |
| **Agent vs RAG** | 都用工具，机制不同 | Agent 多次决策，RAG 一次检索增强 |
| **Agent vs Workflow** | 都是任务流 | Agent 步骤由 LLM 决定，Workflow 步骤由代码写死 |
| **Agent vs Fine-tuning** | 都是提升 LLM 能力 | Agent 扩能力（工具），Fine-tuning 改能力（参数） |

**最容易混的一对：Agent vs RAG**

> RAG（检索增强生成）是 Agent 工具箱里**最常用的一把工具**——让 LLM 能查最新/私域知识。一个"客服 Agent"通常会包含 RAG。

## 7. 自测问题（Self-Check）

<details>
<summary>1. Agent 和普通 Chat 的根本区别是什么？</summary>

**关键不在聪明程度，而在主动性**。Chat 被动应答；Agent 能自主决定调什么工具、什么时候停。但 Agent 也要靠 LLM 推理，模型差它也差。
</details>

<details>
<summary>2. Agent 的三个核心组件是什么？</summary>

LLM（大脑，决定下一步）+ 工具（手脚，访问外部世界）+ 循环（目标未达成继续）。少任何一个都不算真正的 Agent。
</details>

<details>
<summary>3. 什么任务适合用 Agent，什么不适合？</summary>

适合：需要**多步、跨工具、自主决策**的任务（调研、代码生成、复杂工作流）。
不适合：一句话就能答的事实问答、纯创意写作（无工具调用需求）。
判断标准：**能否一步搞定？能→Chat；不能→Agent**。
</details>

## 8. 参考来源（References）

1. **OpenAI 官方介绍：Introduction to LLM Agents**（官方文档）
   - 为什么推荐：最权威的"什么是 Agent"定义来源，避免被各种营销稿带偏。
2. **LangChain Agents 文档**（官方文档）— https://python.langchain.com/docs/modules/agents/
   - 为什么推荐：当前最主流的 Agent 开发框架，文档里有大量可运行的 ReAct 示例。
3. **《ReAct: Synergizing Reasoning and Acting in Language Models》**（论文，Yao et al., 2022）— arXiv:2210.03629
   - 为什么推荐：Agent "思考-行动-观察"循环的经典论文，理解机制的必读。
4. **吴恩达《AI Agent 系列短课》**（公开课）— deeplearning.ai
   - 为什么推荐：用最直白的方式讲清 Agent 设计模式，适合初学者。
5. **Lilian Weng "LLM Powered Autonomous Agents"**（博客）— lilianweng.github.io
   - 为什么推荐：技术综述类博客里写得最系统的一篇，涵盖规划、记忆、工具三大块。

---
name: li-bojie-ai-agent
description: "Knowledge base from \"深入理解 AI Agent — 设计原理与工程实践\" by 李博杰 (v1.4, 2026-08-06). Use when applying Li's frameworks for Agent 架构设计、上下文工程、工具设计、Coding Agent、评估方法论、后训练、持续进化、多模态交互、多 Agent 协作, studying the book, or referencing its concepts."
---

<!-- argument-hint: [topic, framework name, or chapter number] -->

# 深入理解 AI Agent — 设计原理与工程实践
**Author**: 李博杰 | **Pages**: ~314 | **Chapters**: 10 | **Generated**: 2026-08-12

## How to Use This Skill

- **Without arguments** — load core frameworks for reference
- **With a topic** — ask about `上下文工程`, `KV Cache`, `评估`, `SFT vs RL`, or another indexed topic; I find and read the relevant chapter
- **With chapter** — ask for `ch05`; I load that specific chapter
- **Browse** — ask "what chapters do you have?" to see the full index

When you ask about a topic not covered in Core Frameworks below, I will read
the relevant chapter file before answering.

---

## Core Frameworks & Mental Models

### Agent 核心公式(两层视角)
- **Demo/概念层**: `Agent = LLM + 上下文 + 工具` = 大脑 + 眼睛 + 手脚 = Policy + Observation Space + Action Space
- **生产/Harness 层**: `Agent = Model + [上下文 + 工具 + 约束 + 验证 + 纠正]`
- 上下文/工具让 Agent "能做事";约束/验证/纠正让 Agent "不做错事"
- **核心判据**: 大多数"需要更聪明模型"的问题,其实是观察/动作空间接口问题或 Harness 缺陷

### ReAct 循环与轨迹
- Reasoning + Acting(思考→行动→观察三环节循环)
- **上下文 = 静态前缀(系统提示词 + 工具定义) + 轨迹(动态消息历史)**
- 前缀不能动(利于 KV Cache),轨迹可压缩
- 轨迹是 Agent 能力的体现:可分析、可优化、可训练

### KV Cache 友好的上下文设计(三铁律)
1. 系统提示词一旦定下来就不要改(改一个字节该位置后全废)
2. 工具定义顺序固定、内容稳定
3. 动态信息(时间戳/状态/余额)一律追加到末尾,绝不嵌入前缀
- KV Cache(模型内部,单次推理)vs Prompt Cache(推理引擎,跨请求)是两个层级
- 缓存读取成本约首次计算的 1/10;**缓存即架构**——当 Prompt Cache 经济效益显著时,缓存一致性主导系统架构

### 上下文工程的本质:检索而非推理
- 注意力机制是检索引擎,擅长"查找"不擅长"归纳统计"
- Agent 状态栏和压缩都在给"只有一半"的检索引擎补上"提炼"
- **上下文腐化(Context Rot)**: 装得下但找不到,比溢出更隐蔽
- 与其期望模型从冗长上下文自动学习,不如主动显式提炼

### Agent Skills 渐进式披露(三层)
- 元数据(name + description,~300 tokens,常驻 system prompt)
- SKILL.md 核心流程(~2K tokens,按需加载)
- 子文档(选择性深入)
- description 必须写成路由条件("Use when / Don't use when"),**反例不是可选项**

### 工具五分类与致命三要素
- 感知 / 执行 / 协作 / 用户沟通 / 事件触发(按调用方向 × 作用对象)
- **致命三要素**(同时满足即高风险):非交互 + 不可逆 + 跨信任边界 → 必须人工确认
- Proposer-Reviewer vs Sidecar:前者重校验,后者重检索
- 通用工具用于组合探索;专用工具用于约束高风险

### Coding Agent + 文件系统 = 通用 Agent 基础
- 七工具最小集:Code Interpreter / Bash / 读 / 写 / 编辑 / Glob / Grep
- 代码是元能力:能在运行时动态创造新工具(自举)
- 代码即证明:执行结果提供客观对错标准

### 评估:对象是模型 + Harness 组合体
- **Model Swap 决策**:换强模型分数不涨=瓶颈在 Harness;换弱分数大跌=主要由模型决定
- 消融实验定位 Harness 内部哪个部件重要
- 评估首要价值不是打分,而是**让你快速跟上模型演进**

### 后训练两条主线
1. **"SFT 记忆,RL 泛化"**(实验结果,非普遍属性)
2. **"数据和环境比算法更重要"**——现成算法会用就行,精力投到仿真环境真实度和数据质量
- 三阶段:预训练(读万卷书)→ SFT(老师手把手教)→ RL(自己试错提升)

### 持续进化:保存经历 ≠ 从经历中学习
- 把轨迹塞进向量库不会自动完成跨案例比较
- 四种更新载体按可逆性优先级:知识文档 > Prompt/Skills > 程序/Harness > 模型参数
- 所有修改先形成候选版本,经回归/灰度/回滚验证才能改变下一轮运行

### 多模态实时三场景的共同架构挑战
- 语音 / Computer Use / 机器人:延迟敏感 + 多模态持续输入
- 共同方向:从串行流水线走向端到端模型
- 快慢解耦:快模型实时响应,慢模型后台当"军师"

### 多 Agent 协作分类
- 两维度:上下文是否共享 × 组织结构(对等/管理者/去中心化)
- 三通信机制:工具参数 / 共享文件系统(共享内存) / 消息总线(消息传递)
- **新信息判据**: 多 Agent 真正优于单 Agent 当且仅当能带来新信息

---

## Chapter Index

| # | Title | Key Frameworks |
|---|-------|----------------|
| [ch01](chapters/ch01-agent-fundamentals.md) | AI Agent 入门 | Agent 核心公式, ReAct, Harness 五功能, 上下文消融实验 |
| [ch02](chapters/ch02-context-engineering.md) | 上下文工程 | API 结构, KV Cache, 提示工程, Agent Skills, 状态栏, 压缩策略 |
| [ch03](chapters/ch03-memory-knowledge-base.md) | 用户记忆和知识库 | 记忆三层次, RAG, 混合检索, 智能体化 RAG, 双层记忆架构 |
| [ch04](chapters/ch04-tools.md) | 工具 | 五类工具, MCP, 安全机制, 事件驱动架构, 主动工具发现 |
| [ch05](chapters/ch05-coding-agent.md) | Coding Agent 与通用 Agent | 七工具, OpenClaw, 元能力, 自举, Proposer-Reviewer |
| [ch06](chapters/ch06-evaluation.md) | Agent 的评估 | 三层评估体系, Model Swap, LLM-as-a-Judge, 仿真环境 |
| [ch07](chapters/ch07-post-training.md) | 模型后训练 | 三阶段, SFT vs RL, 奖励信号设计, PPO/GRPO, RLVP |
| [ch08](chapters/ch08-continuous-evolution.md) | Agent 的持续进化 | 学习信号, 四种更新载体, 候选版本验证, 灰度发布 |
| [ch09](chapters/ch09-multimodal-realtime.md) | 多模态与实时交互 | 语音三范式, 快慢解耦, Computer Use, VLA, Sim2Real |
| [ch10](chapters/ch10-multi-agent.md) | 多 Agent 协作 | 两维度分类, 三通信机制, A2A 协议, Agent 社会与经济 |

## Topic Index

- **A2A 协议** → ch10
- **Action Space / 动作空间** → ch01
- **Agent Skills** → ch02, ch04
- **Agent 状态栏 / Context Distillation** → ch02
- **Agentic RAG / 智能体化 RAG** → ch03
- **BM25 / 稀疏检索** → ch03
- **Chat Template** → ch02
- **Circuit Breaker / 熔断** → ch01
- **Computer Use** → ch09
- **Context Rot / 上下文腐化** → ch02
- **DPO** → ch07
- **Embedding / 稠密嵌入** → ch03
- **GraphRAG** → ch03
- **GRPO** → ch07
- **Harness / 驾驭** → ch01, ch02
- **Hybrid Retrieval / 混合检索** → ch03
- **KV Cache / Prompt Cache** → ch02
- **Latent Bridge / 快慢接口** → ch09
- **LLM-as-a-Judge** → ch06
- **Loop Engineering / Graph Engineering** → ch01
- **LoRA** → ch07
- **Manus / OpenClaw** → ch01, ch05
- **MCP** → ch04
- **Mem0 / Memobase** → ch03
- **Model Swap / 消融实验** → ch06
- **Observation Space / 观察空间** → ch01
- **On-Policy Distillation** → ch07
- **PRM / 过程奖励** → ch07
- **Prompt Injection / 提示注入** → ch02, ch04
- **Proposer-Reviewer** → ch04, ch05
- **RAPTOR** → ch03
- **ReAct 循环** → ch01, ch02
- **RLHF / RLVP** → ch07
- **Sandbox / 沙盒** → ch04
- **SFT vs RL** → ch07
- **Sidecar / 旁路查询** → ch04
- **Sim2Real** → ch09
- **Sliding Window / 滑动窗口** → ch02
- **Spawn Subagent** → ch04, ch10
- **Trajectory / 轨迹** → ch01, ch02
- **User as Code** → ch03
- **VAD / 全双工** → ch09
- **VLA** → ch09
- **元能力 / 自举** → ch05
- **持续进化闭环** → ch08
- **消息总线** → ch10
- **致命三要素** → ch04

## Supporting Files

- [glossary.md](glossary.md) — 全部关键术语(按字母/拼音排序)
- [patterns.md](patterns.md) — 具体技巧、设计模式、算法(When/How/Trade-offs)
- [cheatsheet.md](cheatsheet.md) — 一页纸决策指南、决策树、阈值、Smells

---

## Scope & Limits

This skill covers the book content only. For hands-on implementation in your codebase,
combine with project-specific tools. For topics beyond this book, check related skills
or ask the agent directly.

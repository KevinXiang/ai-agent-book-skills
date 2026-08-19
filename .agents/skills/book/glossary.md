# Glossary — 深入理解 AI Agent

按拼音/字母排序。格式: **术语** — 定义 (Ch N)

## A

- **A2A 协议 (Agent-to-Agent Protocol)** — Agent 之间互相发现、协商、委托任务的开放协议 (Ch 10)
- **Action Space (动作空间)** — Agent 能做的所有事情的集合,对应 RL 学术概念;现代 Agent 用开放式动作空间(可生成任意自然语言和代码) (Ch 1)
- **ACI (Agent-Computer Interface)** — 工具接口设计的演进框架,关注模型与工具的交互体验,与 HCI 对应 (Ch 4)
- **Agentic RAG (智能体化 RAG)** — 让 Agent 自主决定何时检索、检索什么的范式转变,而非被动触发检索 (Ch 3)
- **Agent Skills** — 领域能力的可组合、可按需加载的知识包;采用渐进式披露设计哲学 (Ch 2)
- **Agent 状态栏 (Agent Status Bar)** — 把分散在上下文各处的隐式状态提炼为显式知识,作为 user 消息插入末尾;Context Distillation 的日常形态 (Ch 2)
- **Ablation Study (消融实验)** — 系统性去除某个组件观察影响,用于证明各组件的真实贡献 (Ch 1, 6)
- **ANN (近似最近邻)** — 高维向量检索算法的总称,如 HNSW、IVF-PQ,用于稠密嵌入检索 (Ch 3)
- **AOI (Area of Interest)** — 机器人/视觉场景中,Agent 的观察接口,限定模型能看到的屏幕区域 (Ch 9)

## B

- **BM25** — 经典稀疏检索算法,基于词频和逆文档频率的关键词匹配 (Ch 3)
- **半监督微调** — 见 SFT

## C

- **Chat Template** — API 服务端将结构化 JSON 消息转为模型实际处理的线性 token 流;用 `<|im_start|>`/`<|im_end|>` 等特殊 token 标识边界 (Ch 2)
- **Channel (PineClaw)** — 事件驱动架构中,接收外部事件的通道抽象 (Ch 4)
- **Chunking (文档分块)** — 把长文档切分为可检索单元的技术,粒度影响召回率与精度 (Ch 3)
- **Circuit Breaker (熔断器)** — 错误连续发生时自动"断电"停止重试的机制,类比家用保险丝 (Ch 1)
- **Computer Use** — 让 AI 像人一样操作电脑图形界面的能力 (Ch 9)
- **Context Distillation (上下文蒸馏)** — 提前算好结论加进上下文,或把臃肿原始记录换成算好的结论 (Ch 2)
- **Context Engineering (上下文工程)** — 系统性设计、组织、提供 AI 完成任务所需的全部背景知识 (Ch 2)
- **Context Rot (上下文腐化)** — 与溢出不同:溢出是"装不下",腐化是"装得下但找不到";决策质量悄然下降 (Ch 2)

## D

- **DPO (Direct Preference Optimization)** — 不需显式奖励模型,直接从偏好对训练的 RL 简化算法 (Ch 7)
- **Deep Research** — Agent 反复搜索阅读、综合出完整报告的产品形态 (Ch 1)

## E

- **Embedding (嵌入)** — 把文本/图像映射为高维稠密向量,使语义相近的内容向量也相近 (Ch 3)
- **Event-Driven Agent (事件驱动 Agent)** — 由外部事件(邮件、定时、Webhook)触发执行的异步架构 (Ch 4)

## F

- **Few-shot 示例** — 在提示词中给出少量输入-输出示范,引导模型按期望格式/风格生成 (Ch 2)

## G

- **GRPO (Group Relative Policy Optimization)** — DeepSeek 推广的 RL 算法,用组内相对优势替代独立 critic (Ch 7)
- **Graph Engineering (图工程)** — 2026 年提出的高层编排视角,把 Agent 循环、确定性程序和人工审批组织成显式执行图 (Ch 1)
- **GraphRAG** — 利用知识图谱构建实体关系索引,支持跨文档推理的 RAG 变体 (Ch 3)

## H

- **Harness (马具/驾驭)** — 模型之外的全部基础设施:上下文 + 工具 + 约束 + 验证 + 纠正 (Ch 1)
- **Harness Engineering** — 设计和优化模型之外基础设施的工程实践;Agent 系统的核心竞争力 (Ch 1)
- **Heartbeat** — OpenClaw 的事件机制之一,周期性唤醒 Agent 检查状态 (Ch 4)
- **Hooks (OpenClaw)** — 注册回调,在特定事件发生时触发 Agent (Ch 4)
- **Hybrid Retrieval (混合检索)** — 稠密嵌入 + 稀疏关键词的组合,兼顾语义理解和精确匹配 (Ch 3)

## K

- **KV Cache** — 模型内部机制,缓存已计算 token 的键值对避免重复计算;前缀不变才能复用 (Ch 2)

## L

- **Latent Bridge (快慢接口)** — 快慢模型之间除文字外还能传递的隐式表征通道 (Ch 9)
- **LLM-as-a-Judge** — 用大语言模型充当评委自动化评判 Agent 输出质量 (Ch 6)
- **Loop Engineering (Loop 工程)** — 把视野从单次运行扩展到跨轮次持续自主运转的工程范式 (Ch 1)
- **LoRA (Low-Rank Adaptation)** — 只训练低秩矩阵的轻量微调方法 (Ch 7)

## M

- **Manus** — 合并 Deep Research + Coding + Computer Use 的生产级通用 Agent (Ch 1)
- **MAST (Multi-Agent System Failure Taxonomy)** — 多 Agent 系统失败模式的分类法 (Ch 10)
- **MCP (Model Context Protocol)** — 标准化 Agent 与工具互操作的协议,类似 USB-C for tools (Ch 4)
- **Mem0 / Memobase** — 用户记忆框架,自动提取、压缩、组织用户偏好 (Ch 3)
- **Message Bus (消息总线)** — Agent 之间传递消息的中转站,对应 IPC 的消息传递范式 (Ch 10)
- **Model Swap (模型替换实验)** — 固定 Harness 只换模型,用于区分"模型能力不足" vs "Harness 设计缺陷" (Ch 6)
- **Moshi / GPT-Live** — 全双工语音 Agent,可边听边说 (Ch 9)

## O

- **Observation Space (观察空间)** — Agent 能看到的全部信息;没有进入观察空间的信息对模型等于不存在 (Ch 1)
- **Omni (端到端全模态)** — 单模型处理语音输入输出但仍轮流说话的范式 (Ch 9)
- **On-Policy Distillation** — 用模型自身生成的样本做蒸馏训练,RL 样本效率优化方法 (Ch 7)
- **OpenClaw** — 本地优先、常驻的个人 Agent,通过消息渠道接收任务、Gateway 连接云应用与本地 (Ch 1, 5)

## P

- **Pass@k / Pass^k** — 评估指标:前者采样 k 次至少一次通过,后者同一上下文下 k 次都通过 (Ch 6)
- **Policy (策略)** — Agent 决定"下一步做什么"的决策逻辑,对应 LLM (Ch 1)
- **Policy Drift (策略漂移)** — 模型在持续学习中偏离原有良好行为的现象 (Ch 8)
- **Prefill 阶段** — 模型生成回复前处理输入全部 token 的阶段;无缓存时计算量随长度平方增长 (Ch 2)
- **PRM (Process Reward Model)** — 过程奖励模型,对推理每一步打分,而非只对最终结果 (Ch 7)
- **Prompt Cache** — 推理引擎优化,跨多次 API 请求复用相同前缀的计算结果;读取成本约首次 1/10 (Ch 2)
- **Prompt Engineering (提示工程)** — 通过优化输入给模型的自然语言指令提升输出质量 (Ch 2)
- **Prompt Injection (提示注入)** — 攻击者通过外部内容注入恶意指令劫持 Agent 行为 (Ch 2)
- **Progressive Disclosure (渐进式披露)** — Skills 的设计哲学:先给目录,需要时再加载完整内容 (Ch 2)

## R

- **RAPTOR** — 层次化文档摘要的 RAG 索引结构,支持不同粒度检索 (Ch 3)
- **ReAct** — Reasoning + Acting,Agent 执行任务的核心循环:思考→行动→观察 (Ch 1)
- **Reinforcement Learning (RL, 强化学习)** — 让模型反复尝试、根据结果好坏给奖惩来改进行为 (Ch 7)
- **RLHF (RL from Human Feedback)** — 用人类偏好训练奖励模型再 RL 的经典方法 (Ch 7)
- **RLVP (RL from Verifiable Problems)** — 用可验证问题(代码、数学)的客观奖励做 RL (Ch 7)
- **RoPE (旋转位置编码)** — Transformer 中的位置编码方法;在 KV Cache 组合研究中用于重定位缓存块 (Ch 2, 9)

## S

- **Sandbox (沙盒)** — 与主系统隔离的安全运行空间,代码执行即使出错也不影响宿主机 (Ch 4)
- **Self-Bootstrapping (自举)** — Agent 用代码动态创造新工具和新 Agent 的能力 (Ch 5)
- **Sidecar (旁路查询)** — 不污染主上下文,通过独立查询获取辅助信息的模式 (Ch 4)
- **Sim2Real** — 把仿真环境中训练的策略迁移到真实机器人的技术 (Ch 9)
- **Sliding Window (滑动窗口)** — 只保留最近几条对话历史的策略,会破坏 KV Cache 一致性 (Ch 2)
- **SFT (Supervised Fine-Tuning)** — 用标注好的"输入—输出"对训练模型,类似老师给标准答案 (Ch 7)
- **Spawn Subagent (创建子 Agent)** — 派生独立上下文子 Agent 的协作原语 (Ch 4, 10)
- **Step-Audio R1** — 把思考内化进单一模型实现"边想边说"的语音架构 (Ch 9)

## T

- **Tool Calling / Function Calling** — 模型通过结构化方式调用外部工具的能力 (Ch 1)
- **Tool Definitions (工具定义)** — 静态前缀的一部分,告诉模型有哪些工具可用及参数规范 (Ch 2)
- **Tool Results (工具结果)** — 闭环控制的关键,缺失会导致 Agent 盲目循环 (Ch 1)
- **Trajectory (轨迹)** — Agent 执行过程中动态增长的消息历史;每次 LLM 调用都看到完整轨迹 (Ch 1, 2)

## U

- **User as Code (可执行代码记忆)** — 把用户偏好表示为可执行代码而非文本,具备精确语义 (Ch 3)

## V

- **VAD (Voice Activity Detection)** — 语音活动检测,判断用户是否在说话;级联架构的轮次假设源头 (Ch 9)
- **VLA (Vision-Language-Action)** — 视觉-语言-动作模型,用于机器人控制 (Ch 9)

## W

- **Worktree (工作目录)** — Agent 在多次执行间保存计划、中间结果、日志和产物的受控目录 (Ch 1, 5)

## 数字/符号

- **τ²-bench / τ-bench** — 工具调用型 Agent 的评估基准 (Ch 6)

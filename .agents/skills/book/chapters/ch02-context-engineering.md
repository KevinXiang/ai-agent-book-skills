# Chapter 2: 上下文工程

## Core Idea
上下文是 Agent 的"眼睛"——**Agent 只能基于它看到的信息做决策**。本章是全书最关键一章:从 API 消息结构建立"上下文 = 静态前缀 + 轨迹"的地基,围绕 KV Cache 友好性、提示工程、提示注入、Agent Skills、状态栏、压缩策略六个层面,系统讲解如何设计、组织、供给 Agent 在每个决策点所需的信息。

## Frameworks Introduced

- **上下文五组成**(对应 API 结构): system 系统提示词 + tools 工具定义(静态前缀) + user/assistant/tool 消息(动态轨迹)
  - When to use: 讨论任何 Agent API 调用结构
  - How: 静态前缀不能动(利于 KV Cache),轨迹可压缩

- **KV Cache 友好设计三原则**:
  1. 系统提示词一旦定下来就不要改
  2. 工具定义顺序固定、内容稳定
  3. 动态信息(时间戳、状态)追加到上下文末尾,不要嵌入前缀
  - When to use: 任何对延迟/成本敏感的 Agent 系统

- **KV Cache vs Prompt Cache**(两个层级): KV Cache 是模型内部机制(单次推理内缓存已计算 token 的 K/V);Prompt Cache 是推理引擎优化(跨请求复用相同前缀)。缓存读取成本约首次计算的 1/10

- **缓存作为架构约束**(Claude Code 实践):
  - 提示词排列顺序首先由缓存边界决定,其次才是语义逻辑
  - 子 Agent 必须与父 Agent 字节级对齐才能命中 Prompt Cache
  - 工具结果的替换字符串首次出现时即被冻结
  - N 个二值条件若放在缓存边界前会产生 2^N 种缓存键

- **Agent Skills 渐进式披露**(三层):
  - 第 1 层:元数据(name + description,~300 tokens,会话启动时加载到 system prompt)
  - 第 2 层:SKILL.md 核心流程(~2K tokens,按需加载)
  - 第 3 层:子文档(选择性深入)
  - description 字段是路由决策关键,应写成路由条件("Use when / Don't use when")而非功能介绍;**反例不是可选项**
  - When to use: 系统提示词膨胀时,按需加载领域能力

- **Agent 状态栏**(Context Distillation 的日常形态): 把分散在上下文各处的隐式状态提炼为可直接使用的显式知识,作为 user 角色消息插入上下文末尾
  - 构成:任务规划(TODO)、事件侧信道信息(时间/位置)、环境当前状态、工具调用计数
  - 效果:弱模型补准确率(可涨 40-54 个百分点)、强模型省效率(思考量降一个数量级);不带状态栏时思考量随上下文增长,带上后基本恒定
  - When to use: 长程任务、有明确约束(如"不超过 3 次呼叫")、需要追踪多步骤状态

- **上下文压缩两动机**:
  1. 解决长度约束和成本约束(直观)
  2. **提升思考质量**——总结后的知识比原始形式更利于模型使用(更深层,易被忽视)

- **检索而非推理**(注意力的本质): 注意力擅长在已有内容里"查找",不擅长在一次前向传播里"归纳统计"。压缩和状态栏都在给这台"只有一半"的检索引擎补上缺失的"提炼"

- **上下文腐化(Context Rot)**: 与上下文溢出不同——溢出是"装不下了",腐化是"装得下但找不到了"。Agent 表面正常工作,决策质量悄然下降。无关内容一旦占到上下文大头,决策质量明显下滑

## Key Concepts

- **Context Engineering(上下文工程)**: 系统性设计、组织和提供 AI 完成任务所需的全部背景知识
- **Trajectory(轨迹)**: Agent 执行过程中动态增长的消息历史;每次 LLM 调用都看到完整轨迹
- **Chat Template**: API 服务端将结构化 JSON 消息转为模型实际处理的线性 token 流;用 `<|im_start|>`/`<|im_end|>` 等特殊 token 标识角色边界
- **Prefill 阶段**: 模型生成回复之前处理输入全部 token 的阶段;无缓存时计算量随上下文长度平方级增长
- **Prompt Injection(提示注入)**: 攻击者通过外部内容(网页、文档、工具结果)注入恶意指令劫持 Agent 行为
- **Progressive Disclosure(渐进式披露)**: Skills 的设计哲学——先给目录摘要,需要时再加载完整内容
- **Context Distillation(上下文蒸馏)**: 提前算好结论加进上下文,或把臃肿原始记录换成算好的结论

## Mental Models

- **AI Agent 是永远的新员工**: 给足背景信息能干得很好,什么都不告诉再聪明也白搭。构建 AI 原生团队首先是文档化运动,而非部署新工具
- **对远程友好的团队也对 AI 友好**: 信息透明、文档驱动(Linux 内核模式)天然创造对 AI 友好的环境
- **缓存即架构**: 当 Prompt Cache 经济效益足够显著时,缓存一致性会反过来主导系统架构选择,而非事后优化
- **状态栏是"页边笔记"**: 模型不会每改一个事实就从头重读,而靠页边笔记;状态栏把分散的隐式状态提炼为显式知识
- **压缩是给"只有一半"的引擎补"提炼"**: 注意力机制是检索引擎不是归纳引擎,压缩把需要思考才能得到的结论变成可直接检索的知识

## Anti-patterns

- **动态系统提示词**: 在 system prompt 嵌入时间戳/余额等动态信息 → 每次请求破坏整个前缀缓存。正确做法:作为 user 消息追加到末尾,或通过工具获取
- **工具定义动态排序**: 按使用频率调整工具顺序 → 改变 token 序列破坏缓存。固定顺序对模型选择能力几乎无影响,但对性能提升显著
- **滑动窗口对话历史**: 只保留最近几条消息 → 破坏前缀一致性 + 丢失关键工具调用结果 → Agent 经常陷入循环重复执行相同调用
- **文本格式化消息**: 把 role-content 转为"USER: ⋯ASSISTANT: ⋯"纯文本流 → 偏离模型训练时使用的标准消息格式,模型需额外消耗注意力推断角色边界
- **让 LLM 维护状态栏**: 让大模型批量统计长历史吐出状态栏 → 把"扫描整段上下文"难题原封不动搬了个家。应用代码(20 行正则)维护,或 LLM 逐条抽取再由代码汇总
- **只留状态栏删原始上下文**: 状态栏是有损投影,只覆盖预想维度。一旦问题落到没算过的维度,准确率断崖式崩塌
- **孤立修改字段而无 CoT**: 没有 CoT 时孤立改字段会被忽略——结论已烘焙进下游状态却无思考路径更新它

## Code Examples

**Agent 核心循环的 messages 管理**(章节核心,所有上下文工程技术本质都在优化这个列表):

```python
def execute_tool(name, arguments):
    if name == "get_current_time":
        return '{"datetime": "2025-09-13T05:18:47", "day_of_week": "Saturday"}'
    elif name == "get_weather":
        return '{"temperature": 13.2, "unit": "celsius", "conditions": "clear"}'

messages = [
    {"role": "system", "content": "You are a helpful assistant. Use tools when needed."},
    {"role": "user", "content": "What's the current time and weather in Vancouver?"},
]

while True:  # Production: 需要 max_iterations 上限防止死循环
    response = client.chat.completions.create(
        model="Qwen3-0.6B", messages=messages, tools=tools)
    assistant_message = response.choices[0].message
    messages.append(assistant_message)
    if not assistant_message.tool_calls:
        print(assistant_message.content); break
    for tool_call in assistant_message.tool_calls:
        result = execute_tool(tool_call.function.name, tool_call.function.arguments)
        messages.append({"role": "tool", "tool_call_id": tool_call.id, "content": result})
```

- **What it demonstrates**: Agent 框架的核心工作就是管理 messages 列表——在合适时机追加消息,然后把整个列表送给模型

**Agent 状态栏注入**(作为 user 消息插入末尾,而非改 system):

```python
messages: [
   { role: "system", content: "You are a customer service assistant..." },     # Fixed
   { role: "user",   content: "Help me cancel my Xfinity plan" },
   { role: "assistant", tool_calls: [...] },                                    # Round 1
   { role: "tool",   content: "Call log..." },
   ...(more rounds)
   { role: "user",   content: "<agent_status>\n"                                # ← 状态栏
      "Current State:\n - phone_call invoked 3 times (Xfinity: 3/3 max)\n"
      "TODO: 取消套餐(进行中)</agent_status>" },
]
```

- **What it demonstrates**: 状态栏作为 user 消息插入末尾,紧邻模型生成位置获得最高注意力权重,且不破坏前缀缓存

## Reference Tables

**上下文压缩策略对比**(实验 2-X,基于一个失败任务的 token 用量):

| 策略 | Token 用量 | 压缩率 | 迭代次数 | 结果 |
|---|---|---|---|---|
| 无压缩 | 166,043 | 102.1% | 5 | ✗ 失败 |
| 个体摘要 | 276,608 | 10.9% | 12 | ✓ 成功 |
| 组合摘要 | 93,449 | 4.3% | 10 | ✓ 成功 |
| **上下文感知** | **40,157** | **3.0%** | **7** | **✓ 成功(最优)** |
| 感知+引用 | 222,992 | 4.1% | 10 | ✓ 成功 |
| 自适应窗口 | 174,601 | 102.4% | 7 | ✓ 成功 |

**上下文五组成 ↔ API 结构对应**:

| 第一章五组成 | API 消息角色 | 性质 |
|---|---|---|
| 系统提示词 | `system` 消息 | 静态前缀(KV Cache 缓存) |
| 工具定义 | `tools` 字段(顶层) | 静态前缀 |
| 用户消息 | `user` 消息 | 动态轨迹 |
| 模型回复 | `assistant` 消息(含 tool_calls) | 动态轨迹 |
| 工具执行结果 | `tool` 消息(通过 tool_call_id 关联) | 动态轨迹 |

## Worked Example

**实验 2-3 KV Cache 常见错误模式**(★☆☆): 系统性测试了五种常见但有害的上下文管理模式,所有解法最终收敛回三条核心结论(系统提示词不动、工具顺序固定、动态信息追加到末尾)。其中"动态系统提示词"是最常见错误——在 system prompt 嵌入时间戳让 token 序列每次都不同,正确做法是把时间作为 user 消息追加或通过工具获取。

**实验 2-7 Agent 状态栏注意力可视化**(★★☆): 客服 Agent 已拨打 Xfinity 3 次电话,用户追问"能不能再打电话催促":
- **对照组 A(无状态栏)**: 注意力高度分散,在三次电话调用区域形成"聚焦点",思考 token 体现数数统计过程。Qwen3-0.6B 经常违反约束继续拨打
- **对照组 B(有状态栏)**: 在轨迹末尾添加 `<agent_status>phone_call 已被调用 3 次(Xfinity: 3/3 max)</agent_status>`,注意力高度集中在状态栏,思考过程直接使用已提炼信息,稳定遵从约束

**量化基准结论**(作者与合作者的 Context Distillation 研究):
- 弱模型:准确率涨 40-54 个百分点,2B 本地小模型追平不带状态栏的前沿大模型
- 强模型:思考量/延迟/花费各降约一个数量级(思考 token 砍掉八九成)
- 不带状态栏:思考量随上下文变长持续增长;带上后:基本恒定

## Key Takeaways

1. **上下文 = 静态前缀 + 轨迹**: 这是所有上下文工程技术的地基。前缀不能动(利于 KV Cache),轨迹可压缩
2. **缓存即架构**: 当 Prompt Cache 经济效益足够大,缓存一致性会反过来主导系统架构选择,而非事后优化。Claude Code 大部分 Harness 代码都围绕这个约束
3. **系统提示词一旦定下来就不要改**: 改一个字节会让该位置及之后的缓存全废;动态信息一律追加到末尾
4. **检索而非推理**: 注意力机制是检索引擎,不擅长归纳统计。压缩和状态栏的本质都是补上"提炼"这一半
5. **Skills 渐进式披露 = 上下文经济的可组合单元**: 元数据常驻(数百 token)+ 正文按需加载(数 K token);description 必须写成路由条件,反例不是可选项
6. **状态栏三条经验**: 用代码维护别让 LLM 维护、不要删原始上下文、把准确率当一线生产指标盯
7. **压缩有两动机**: 解决长度/成本是表象,**提升思考质量**才是深层动机——总结后的知识比原始形式更利于模型使用
8. **上下文腐化比溢出更隐蔽**: Agent 表面正常但决策质量悄然下降;与其期望模型从冗长上下文自动学习,不如主动显式提炼
9. **选 Agent 交互模式要对齐模型厂商训练方法论**: 用 Claude 就充分利用 Skills + 结构化提示;偏离厂商优化过的模式等于给自己挖坑

## Connects To

- **Ch 1**: 上下文是 Agent = LLM + 上下文 + 工具 的"眼睛"组件;本章深化第一章的"上下文五组成"
- **Ch 3**: 记忆和知识库把上下文管理延伸到跨会话持久化
- **Ch 4**: 工具定义设计(4.6)、提示注入防御(4.x 安全)
- **Ch 5**: Coding Agent 的状态管理、上下文压缩实现
- **Ch 8**: 持续进化判断"经验应写成知识、Prompt、Skill、程序还是模型参数"

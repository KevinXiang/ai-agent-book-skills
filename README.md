# ai-agent-book-skills

> 把优秀 AI Agent 书籍转化为可被 Coding Agent(Claude Code / ZCode / Cursor 等)按需调用的 **Skill 知识库**。
>
> 让你在写 Agent 代码时,LLM 能直接调用书中沉淀的框架、决策规则与反模式,而不是凭直觉。

本项目当前收录的第一本书:

## 📚 深入理解 AI Agent — 设计原理与工程实践

**作者**:李博杰 · **版本**:v1.4 (2026-08-06) · **页数**:~314 · **章节**:10

一本系统讲解 AI Agent 设计与工程实践的中文技术书。从 Agent 核心公式出发,沿"构建 → 评估与进化 → 交互与协作"四个层次展开,覆盖上下文工程、工具设计、Coding Agent、评估方法论、模型后训练、持续进化、多模态交互、多 Agent 协作等核心主题。

### 全书结构

```
                  第1章  Agent 基础知识
                 统一概念 · ReAct · Harness · 学习机制
                           │
       ┌───────────────────┴────────────────────┐
       │              构建 Agent                  │
       │                                         │
   第2章 上下文工程    第3章 记忆与知识库    第4章 工具       第5章 Coding Agent
   KV Cache·状态栏·压缩  RAG·智能体化 RAG       MCP·安全·异步    元能力·自举
       │                                         │                │
       └───────────────────┬────────────────────┘                │
                           │           评估与进化                  │
                           │                                      │
                    第6章 Agent 评估   第7章 模型后训练   第8章 持续进化
                    Model Swap·Judge    SFT vs RL         四种更新载体
                           │                                      │
                           └───────────────────┬──────────────────┘
                                               │  交互与协作
                                    第9章 多模态与实时交互   第10章 多 Agent 协作
                                    语音·Computer Use·机器人   消息总线·Agent 社会
```

## 📁 仓库结构

```
ai-agent-book-skills/
└── .agents/skills/li-bojie-ai-agent/      # Skill 根目录(Agent 自动发现)
    ├── SKILL.md                            # 主入口:核心框架 + 章节/主题索引
    ├── chapters/                           # 10 章摘要(按需加载,不占常驻上下文)
    │   ├── ch01-agent-fundamentals.md
    │   ├── ch02-context-engineering.md     # 全书最关键一章
    │   ├── ch03-memory-knowledge-base.md
    │   ├── ch04-tools.md
    │   ├── ch05-coding-agent.md
    │   ├── ch06-evaluation.md
    │   ├── ch07-post-training.md
    │   ├── ch08-continuous-evolution.md
    │   ├── ch09-multimodal-realtime.md
    │   └── ch10-multi-agent.md
    ├── glossary.md                         # 全部关键术语(按字母/拼音排序)
    ├── patterns.md                         # 具体技巧、设计模式(When/How/Trade-offs)
    └── cheatsheet.md                       # 一页纸决策指南、决策树、Smells
```

**设计原则**(遵循 Agent Skills 渐进式披露):
- `SKILL.md` 元数据常驻(~3K tokens),Agent 知道自己拥有哪些能力
- `chapters/*` 按需加载 —— 问到某章才读,不占常驻上下文
- `glossary` / `patterns` / `cheatsheet` 是查阅层,密度优先于完整性

## 🚀 如何使用

### 方式一:在支持 Skills 的 Agent 中直接调用

把本仓库 clone 到工作目录,或复制 `.agents/skills/li-bojie-ai-agent/` 到你自己项目的 `.agents/skills/` 下:

```bash
git clone https://github.com/KevinXiang/ai-agent-book-skills.git
```

然后在 Claude Code / ZCode / Cursor 等支持 Agent Skills 的工具里:

```
# 不带参数 → 加载核心框架做参考
li-bojie-ai-agent

# 带主题 → 查某个概念
li-bojie-ai-agent 讲讲 KV Cache 友好的上下文设计
li-bojie-ai-agent SFT 和 RL 怎么选?
li-bojie-ai-agent 工具的致命三要素是什么?

# 带章节号 → 深入某章
li-bojie-ai-agent 的 ch07
```

### 方式二:直接读 Markdown

每个文件都是自包含的中文文档,可以脱离 Agent 直接阅读 —— 当作学习笔记或速查手册。

## 🎯 适合谁用

- **Agent 应用开发者**:写代码时让 LLM 直接套用作者的决策框架(如"瓶颈在模型还是 Harness"、"SFT 还是 RL"、"经验沉淀到哪种载体")
- **本书读者**:把读过的内容沉淀为可查询的知识库,而不是读完即忘
- **AI Agent 学习者**:作为系统化的概念地图,快速建立对 Agent 工程的全局认识

## 🔑 核心框架速览

| 框架 | 一句话 |
|---|---|
| Agent 核心公式 | `Agent = LLM + 上下文 + 工具`(Demo) / `= Model + Harness`(生产) |
| 上下文 = 静态前缀 + 轨迹 | 前缀不能动(利于 KV Cache),轨迹可压缩 |
| KV Cache 三铁律 | 系统提示词不动 · 工具顺序固定 · 动态信息追加到末尾 |
| 检索而非推理 | 注意力擅长"查找"不擅长"归纳" → 状态栏和压缩补上"提炼" |
| 工具致命三要素 | 非交互 + 不可逆 + 跨信任边界 → 必须人工确认 |
| SFT vs RL | 先 SFT 立"形",再 RL 求"神";"SFT 记忆,RL 泛化"是实验结果非普遍规律 |
| 数据 > 环境 > 算法 | 真正决定成败的是仿真环境真实度和训练数据质量 |
| 四种更新载体 | 知识文档 > Prompt/Skills > 程序/Harness > 模型参数(按可逆性优先) |
| 多 Agent 新信息判据 | 多 Agent 真正优于单 Agent 当且仅当能带来新信息 |

> 完整决策表见 [`cheatsheet.md`](.agents/skills/li-bojie-ai-agent/cheatsheet.md)。

## 🛠️ 这个 Skill 是怎么生成的

使用 [book-to-skill](https://github.com/virgiliojr94/book-to-skill) 工具(配合 ZCode agent)从 PDF 自动转换:

```bash
/book-to-skill /path/to/AI-Agents-in-Depth-zh-CN.pdf
```

**流程**:
1. PDF 文本抽取(技术模式,Docling 结构感知;沙箱无网时 fallback 到 pdftotext)
2. 全书结构分析(标题、作者、10 章定位)
3. 每章生成 2-3K tokens 摘要,保留作者精确命名(如"上下文腐化""智能体化 RAG""致命三要素")
4. 生成 glossary(110+ 术语)、patterns(40+ 设计模式)、cheatsheet(12 决策表)
5. 生成主 SKILL.md(< 4K tokens,核心框架 + 章节索引 + 主题索引)
6. 安全扫描 → 清理临时文件

**核心原则**:提取结构,而非生成摘要 —— 捕获命名框架、可执行原则、技巧与反模式,而非章节内容回顾。

## 📝 许可与致谢

- 本 Skill 中的**框架、术语、决策规则**归原书作者 **李博杰** 所有,本项目仅做结构化提炼以便个人学习与 Agent 辅助开发
- 原书版权属于原作者,如需完整内容请支持正版
- Skill 生成工具:[book-to-skill](https://github.com/virgiliojr94/book-to-skill) by Virgilio Jr

---

> 💡 **计划收录更多书籍**。如果你有推荐的 AI Agent 相关书籍,欢迎提 Issue。

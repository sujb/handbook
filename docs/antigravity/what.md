# 第一部分 · 它是什么

---

## §01 定位：Google Antigravity 到底是什么

### 一句话

Antigravity 是 Google 在 2026 年推出的 **agent-first 开发生态**，不是 IDE，不是 Copilot，是一套让 Agent 代替人执行任务的系统。

### 和同类工具的本质差异

| 工具 | 定位 | 你的角色 |
|------|------|----------|
| Antigravity | Agent 代替人执行 | 设计师 / 审核员 |
| Hermes Agent | 可定制的个人 Agent | 使用者 / Skill 作者 |
| Claude Code | 代码级 pair programmer | 驾驶员 |
| Cursor | IDE 内嵌 AI | 用户在 IDE 里 |

关键区别：**Antigravity 的默认状态是 Agent 自己干活，你回头看结果；Hermes 的默认状态是你发指令，它执行一段。**

### 为什么 Google 在 2026 年推它

三个现实推动：

1. **模型能力过了一个阈值**：Gemini 的上下文窗口和 tool use 质量，已经能支撑"代替人执行"而不只是"辅助写代码"
2. **用户行为变了**：原 Antigravity IDE 上线后，大量非开发者用户也在用 Agent Manager 做非开发任务
3. **竞争倒逼**：Claude Code、OpenCode、Hermes 等工具已经证明了 agent-first 的市场，Google 需要一个统一的产品束

### 底层逻辑

Antigravity 的核心命题是：

> 你不是在帮 AI 写代码，你是在**设计一套让 AI 自己干活的系统**。

这和 Loop Engineering 的说法完全一致：你的工作从"一句句 prompt"变成"设计那个自动发 prompt 的系统"。

---

## §02 四层产品：Desktop / IDE / CLI / SDK

### 为什么是四层，不是一层

Antigravity 不是一个 App，是一个**生态**。四层产品共享同一套 agent harness，但服务于不同场景：

| 产品 | 形态 | 适合谁 | 核心能力 |
|------|------|--------|----------|
| **Antigravity (standalone)** | 桌面 App | 个人开发者、非开发者 | 管理多个本地 Agent、Schedule、Artifact 审查 |
| **Antigravity IDE** | IDE（VS Code 衍生） | 专业开发者 | 深度代码上下文、代码库感知、IDE 内 Agent |
| **Antigravity CLI** | 命令行 | 资深开发者、DevOps | 终端内 Agent、Subagents、Skills |
| **Antigravity SDK** | Python SDK | 工程师、平台团队 | 程序化构建自定义 Agent |

### 选型决策树

```
你是开发者吗？
├── 否 → Desktop App
└── 是
    ├── 需要 IDE 深度集成？ → IDE
    ├── 只在终端工作？ → CLI
    └── 要嵌入自己的系统？ → SDK
```

### 一件要注意的事

从 **2026 年 5 月 19 日** 起，Google 把原来的 **Gemini CLI** 合并进了 **Antigravity CLI**。Gemini CLI 将于 **2026 年 6 月 18 日** 停止对个人用户服务。现在安装 Antigravity CLI 就是安装未来。

---

**第二部分（§03-§05）**：讲运行机制，是理解后面所有实战的前提。

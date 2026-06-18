# 第二部分 · 它怎么运行

---

## §03 核心架构：共享 agent harness + Gemini 生态

### 什么是 agent harness

harness 是一个英文词，原意是"挽具 /  harness"。在 AI Agent 语境下：

> **Agent Harness = 一整套让 Agent 能干活的底座**，包括：接受指令、调用工具、管理上下文、执行子任务、返回结果。

Antigravity 的桌上 App、CLI、SDK 都是**同一个 harness 的不同外壳**。这意味着：

- 你在 Desktop App 里教的 Skills，CLI 里能用
- CLI 里修的 Hooks，SDK 里也能读到
- Google 改进 harness 的核心能力，四层产品同时受益

### Gemini 绑定逻辑

- Antigravity 深度绑定 **Gemini 模型**（尤其是 3.5 Flash / Flash Lite）
- 支持 **多模型**：可以通过 SDK 切其他模型，但桌面端和 CLI 默认走 Gemini
- 1M token 原生上下文（Gemini 3 级别），这是 Subagents 不被主上下文污染的前提

---

## §04 项目机制（Project）而不是工作区（Workspace）

### 为什么改名字

原来的 Antigravity IDE 按 **workspace**（一个仓库）组织对话。2.0 改成 **Project**，原因是：

1. 真实项目往往是多仓库（前端 + 后端），不能割裂
2. Workspace 的命名暗示了"一个目录"，但 Agent 实际需要的是**一个权限边界**

### Project 的定义

> **Project = 一组本地文件夹 + 一套独立权限配置 + 一组对话历史**

特性：
- 一个 Project 可以映射**多个本地文件夹**
- 每个 Project 有独立的 Security Preset
- 对话按 Project 分组，左侧导航栏可切换

### 什么时候开多个 Project

| 场景 | 建议 |
|------|------|
| 一个仓库的日常开发 | 1 个 Project |
| 前后端两个仓库，Agent 需要同时看两边 | 1 个 Project，映射两个文件夹 |
| 团队任务和个人项目混用 | 分开，避免权限混乱 |
| 不同类型的工作（写代码 vs 写文档） | 分开，不同 Security Preset |

---

## §05 记忆与上下文

### 三层记忆机制

**第一层：对话上下文**
- 当前对话的完整历史
- Antigravity 会自动把**之前对话的摘要**一起发给模型，不只是最后一条
- 这是默认行为，不需要你手动管理

**第二层：项目级 Knowledge**
- 项目文件夹里的特定文件会被 Agent 自动读取
- 通常是一个 `KNOWLEDGE.md` 或类似命名的文件
- 作用：给 Agent "项目背景知识"，比如架构决策、编码规范
- 类比：Hermes 的 `MEMORY.md`，但 Antigravity 没有显式的分层结构

**第三层：Hooks**
- 用户可以写 **JSON 格式的 hooks**
- 作用：拦截 Agent 的某些行为，注入自定义逻辑
- 不是长期记忆，是**行为规则**

### 主 Agent 和子代理隔离

- 主 Agent 的上下文窗口不会被子代理的中间结果污染
- 子代理完成任务后，只把**摘要**返回给主 Agent
- 这是 Antigravity 能处理复杂任务的关键设计

### 没有显式记忆API

目前没有发现类似 Hermes 的 `memory` 工具，也没有 `USER.md` / `MEMORY.md` 的官方规范。记忆主要通过：
- 项目文件（被动读取）
- Hooks（主动拦截）
- 对话历史（自动摘要）

---

**第三部分（§06-§08）**：你怎么上手。

# Antigravity 实战手册 · 大纲（v0.2）

> 结构：它是什么 → 它怎么跑 → 你怎么用 → 你怎么管控 → 边界在哪
> 每一层都是下一层的前提

---

## 第一部分 · 它是什么

### §01 定位：Google Antigravity 到底是什么
- 一句话定义
- 和 Hermes Agent / Claude Code / Cursor 的本质差异
- Google 为什么在 2026 年推它

### §02 四层产品：Desktop / IDE / CLI / SDK
- 各自扮演什么角色
- 为什么不是一套东西，而是四套
- 选型：个人用哪个入口，团队用哪个入口

---

## 第二部分 · 它怎么运行

### §03 核心架构：共享 agent harness + Gemini 生态
- harness 是什么
- Gemini 模型绑定逻辑
- Desktop App 和 CLI 为什么共用同一套后端

### §04 项目机制（Project）而不是工作区（Workspace）
- Project 的定义：一组本地文件夹 + 独立权限配置
- 为什么从 workspace 改成 project
- 多项目并行的适用场景

### §05 记忆与上下文
- 项目级 Knowledge 文件：Agent 自动读取的背景文档
- 对话上下文：主 Agent 和子代理隔离
- Hooks：JSON 格式拦截行为
- 没有显式的 USER.md / MEMORY.md，记忆主要靠项目文件

---

## 第三部分 · 你怎么开始用

### §06 十分钟启动
- Desktop App 安装
- CLI 安装
- 登录 + 主题 + 插件
- 第一个能跑通的任务

### §07 四种工作模式
- 模式 A：当聊天机器人用（TUI / 桌面端）
- 模式 B：当群里的助手用（Gateway）
- 模式 C：让它自己跑（Schedule / Cron）
- 模式 D：嵌进代码（SDK）

### §08 Slash Commands 实战
- `/browser`
- `/schedule`
- `/goal`
- `/grill-me`
- 其他内置命令

---

## 第四部分 · 你怎么管控它

### §09 权限与审核三层
- Default：继承全局
- Require manual review：所有命令和文件访问都需点头
- Secure Mode + Auto Execution 关闭：最严组合

### §10 沙箱边界
- Secure Mode 的三条红线
- 沙箱逃逸漏洞历史与修复
- 本地开发的注意事项

### §11 第一个完整项目：代码审查助手
- Case：给一份 PR，自动 review
- Follow：配置规则、跑 review、输出报告
- 适合用来练手的 3 个变量：规则粒度、上下文长度、输出格式

---

## 第五部分 · 怎么创作

### §12 创建你的第一个 Plugin
- 目录结构
- manifest 写法
- 本地调试

### §13 精通 Antigravity SDK
- Python SDK 调用
- 构建自定义 Agent
- 多模型切换

### §14 设计循环：让 Antigravity 自己干活
- Schedule / Hooks / Subagents 的组合
- 验证与回退机制

---

## 第六部分 · 边界与成本

### §15 成本：Token、延迟、Gemini API 配额
- 不同任务的实际消耗
- 免费额度的边界
- 怎么估算成本

### §16 常见坑与排错
- 安装失败 / 认证失败
- Context 超限
- 插件冲突

---

## 附录

### A 术语表
### B 官方资源索引
### C 所有支持的 Slash Commands 速查
### D 所有支持的 Tool / Native Tools 速查
### E 推荐 Skills / Plugins 清单

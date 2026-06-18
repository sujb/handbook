# §01-§02 十分钟启动 Hermes / 选对你的入口

### 前提

- 一台 Linux / macOS / WSL2
- Python 3.11+
- 最少 8GB 内存（本机模型权重另算）
- 需要 64K 上下文窗口的模型

安装本体
```bash
pip install hermes-agent
```

---

## §01 十分钟启动 Hermes

### Case

你只有一个终端窗口。`pip install` 是你能接受的最大成本。  
目标：5 分钟内跑出第一次有效对话。

### Follow

```bash
# 1. 安装
pip install hermes-agent

# 2. 启动 TUI
hermes --tui
```

选 Provider：
```bash
hermes model
```

推荐顺序：
1. Nous Portal（OAuth，体验最完整）
2. OpenRouter（API Key，模型选多）
3. 任何你账户里已有的 Chat Completions API Key

第一次验证 prompt：
```
列出我当前目录的文件
```

如果你看到真实文件列表，而不是 `401` 或 `context length exceeded`，说明 OK。

常见卡住：
- `40K context`：`hermes config set model <model-with-64k>`
- `pip 太慢`：换国内镜像
- `权限不足`：`pip install --user` 或开 venv

### 原理（一句）

Hermes 的入口层（CLI / TUI / Messaging Gateway / Cron / Batch API）  
都是对同一个 `AIAgent` 类的不同封装。装好本体，任何入口都能用。

### 进阶

- 多 Provider 兜底：主 Provider 挂了可以切备用
- `/model` 切模型：不用重新登录

---

## §02 选对你的入口

### Case

Hermes 有好几种入口。你刚装好，不该全装。  
只装最贴近你当前痛点的那一个。

### 入口对比

| 入口 | 适合 | 成本 |
|------|------|------|
| `hermes` (CLI) | 快速调通、看日志 | 低 |
| `hermes --tui` | 复杂任务、反复 prompt | 低 |
| Messaging Gateway（Telegram / Discord / Slack / Feishu） | 群里被 @ | 中（要 Bot / App / 域名 / 验证） |
| Cron | 无人值守 | 低 |
| Batch / API | 嵌入自己的脚本或服务 | 中（要写代码） |

### Follow：按场景选择

| 你现在想做的事 | 先装这个 | 理由 |
|----------------|----------|------|
| 只有一台本机，想写代码看输出 | TUI | 最完整，能看清每步 |
| 要值守一个群，群里 @ 它 | Gateway → 选 Feishu / Telegram | 一次配好，后面不用开终端 |
| 让它每天自动跑 | Cron | 不用装额外客户端 |
| 要嵌入你自己的脚本或服务 | Batch / API | 直接 import |

### 原理（一句）

Profile 隔离。每个入口可以绑一个独立 `~/.hermes/` 目录，技能、记忆、cron 互不污染。

### 进阶

- 一台 VPS 上：TUI 跑开发任务，Cron 跑定时任务
- Gateway 后面接多个 Profile，不同群对应不同助手人格

---

**下一步（§03）**：从第一个真实项目开始，看 Hermes 真正是怎么调用工具的。

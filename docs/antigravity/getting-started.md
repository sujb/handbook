# 第三部分 · 你怎么开始用

---

## §06 十分钟启动

### 安装 Desktop App

1. 打开 https://antigravity.google/download
2. 选你的系统（macOS / Linux / Windows）
3. 下载安装包，运行安装程序
4. 用 Google 账号登录
5. 接受 Security and Data Use policy
6. 选主题，装不装 Google Developer Tools 插件（可选）
7. 点 Finish

### 安装 CLI

```bash
# macOS (Homebrew)
brew install antigravity/tap/antigravity

# 或直接下载
# https://antigravity.google/download
```

**注意**：原来的 `gemini` 命令已被废弃，2026 年 6 月 18 日后停止服务。现在统一用 `antigravity` 命令。

### 第一个能跑通的任务

打开 Terminal，运行：

```bash
antigravity
```

登录后，直接在输入框里写：

```
看看我当前目录里有什么文件
```

如果你看到真实文件列表，而不是报错，说明 OK。

常见卡住：
- `Command not found`：PATH 没加对，重启 Terminal 或手动加
- `登录失败`：检查网络，Antigravity 需要访问 Google 服务
- `模型不可用`：部分地区 Gemini 3 还没全量，换成 Flash 或 Flash Lite

---

## §07 四种工作模式

Antigravity 的入口虽然多，但核心使用模式只有四种：

| 模式 | 入口 | 适合 | 一句话说明 |
|------|------|------|-----------|
| A：聊天 | Desktop / IDE | 单任务、反复调整 | 和 Agent 实时对话 |
| B：群聊 | Gateway | 团队共享 | 在群里被 @ |
| C：自动跑 | Schedule | 无人值守 | 定时触发 |
| D：嵌入 | SDK | 自己的系统 | 代码调用 |

#### 模式 A：聊天（最常用）

在 Desktop App 里直接输入框对话。适合：
- 问代码问题
- 让它改一个文件
- 看 Agent 的工作过程

#### 模式 B：群聊

Antigravity 支持作为群里的 bot（类似 Telegram / Discord Bot）。适合：
- 团队共享一个 Agent
- 群内 @ 它处理工单

#### 模式 C：自动跑

用 `/schedule` 或 UI 里的 Schedule 功能，可以：
- 定时触发一次
- 设置循环任务（每天 / 每周）

#### 模式 D：嵌入

用 Python SDK 把 Antigravity 的能力嵌进你自己的服务。适合：
- 自动化流水线
- 定制 Agent 行为

---

## §08 Slash Commands 实战

Antigravity 内置了一批 Slash Commands，直接打在输入框里，以 `/` 开头。

### 已确认的内置命令

| 命令 | 作用 | 例子 |
|------|------|------|
| `/browser` | 启动一个浏览器子代理 | `/browser 去查 GitHub issues` |
| `/schedule` | 定时或循环触发任务 | `/schedule 每天早上 9 点` |
| `/goal` | 持续执行直到任务完成，不打断 | `/goal 把这个项目重构完` |
| `/grill-me` | Agent 先追问你，澄清需求再动手 | `/grill-me 我要做一个用户系统` |

### 使用方式

1. 在输入框输入 `/`，会弹出命令列表
2. 用 tab 或方向键选择
3. 按回车执行

### 注意事项

- Slash commands 是**直接发给 Agent 的指令**，不是 shell 命令
- 自定义命令目前建议做成 **Skills** 或 **Plugins**，而不是 inline commands（v0.x 的 inline commands 正在被逐步移除）
- CLI 的 tab completion 也支持 Slash commands

---

**第四部分（§09-§11）**：你怎么管控它——权限、沙箱、审核。

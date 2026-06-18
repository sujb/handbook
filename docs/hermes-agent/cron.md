# §06 让它在你睡觉时干活：Cron 与 Loop

Cron 让 Hermes 在无人值守时自动跑任务。  
本章的“干活”不只是一次性命令，而是一个会自己判断、自己续跑的循环。

---

## Case

每天早上 9 点，自动整理 AI 新闻，摘要写进 `daily.md`，然后推送到你的飞书。

---

## Follow

### 1. 先写好 Skill

新建 `~/.hermes/skills/daily-brief/SKILL.md`：

```markdown
---
name: daily-brief
description: "每日 AI 新闻精简"
metadata:
  hermes:
    tags: [news, research]
---

# Daily Brief

## Procedure
1. web_search 搜索 "AI 新闻 昨天"
2. 选 3 条最重要的
3. 写 /tmp/daily.md
4. 用 send_message 推送到飞书
```

### 2. 创建 Cron

```bash
cronjob action='create'
  name="morning-ai-brief"
  schedule="0 9 * * *"
  prompt="执行 daily-brief 技能"
  deliver="origin"
```

### 3. 验证

```
cronjob action='list'
```

确认状态是 `enabled`。  
到时间后看飞书 / 本地输出目录。

---

## Loop 的五步

| 动作 | 在这个 Cron 里的表现 |
|------|----------------------|
| 发现 | 定时触发 |
| 交付 | 调用 daily-brief Skill |
| 验证 | 返回摘要质量 |
| 持久化 | write_file |
| 调度 | cron 自动 |

---

## 注意点

- Cron 会话**完全隔离**，不会继承你之前的对话上下文
- 别让 Hermes 在 Cron 里做需要人工确认的步骤
- 失败的任务不会自动重投；失败时常见原因是网络 / 模型额度不够

---

**下一步（§07）**：接入飞书，让 Hermes 变成群里能被 @ 的机器人。

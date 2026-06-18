# §05 让它记得你：记忆系统

Hermes 会忘。不是故障，而是默认设计。  
本节把它变成**你不想让它忘的东西都记得住**。

---

## Case

你昨天才告诉它偏好，今天重启它又忘了。  
为什么？因为记忆不是存在上下文里的。

---

## Follow

### 1. USER.md —— 它在每次启动时都读

编辑 `~/.hermes/USER.md`：

```markdown
## 基本信息
- 时区：Asia/Shanghai（UTC+8）
- 语言：中文优先

## 沟通偏好
- bullet list
- 代码示例优先 Python
- 一次性给 5 条以内的建议
```

### 2. MEMORY.md —— 项目状态

```markdown
## 当前项目
- 名称：Hermes Agent 实战手册
- 进度：§01–§04 已完成

## 环境事实
- 模型：Claude Code
- Provider：OpenAI
```

### 3. 对话中让它自己写

```
记住：后续所有中文回复优先 bullet，不需要问候语和客套。
```

Hermes 会通过 `memory` 工具自动写入当前 Profile 的 `USER.md`。

### 4. 验证

```bash
rm -rf ~/.hermes/sessions/*
hermes --tui
```

重新问同一个问题，看它是否像记得住。

---

## 原理（一句）

上下文是“这一刻它能看到的东西”，对话一结束就清空。  
`USER.md` / `MEMORY.md` 是写在磁盘上的，跨 session 存活。

---

## 注意点

- **读得快，写得慢**：建好 USER.md 是最高效的记忆手段
- **MEMORY.md 会变旧**：定期清理，不然反而干扰
- **上下文 ≠ 记忆**：上下文越大越贵；该沉到磁盘的沉下去

---

**下一步（§06）**：让它在你睡觉时也主动干活。

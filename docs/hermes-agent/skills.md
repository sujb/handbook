# §04 把手工流程变成 Skill

你已经会用 Hermes 整理资料了。  
那接下来最值钱的一件事：**把这套流程变成 Skill**，下次直接触发，不用 each time 重发一遍需求。

---

## Case

你每周都整理同一批来源。每次重新描述任务太费劲。  
目标：把 §03 的过程固化成一条斜杠命令。

---

## Follow

### 1. 新建 Skill

```bash
mkdir -p ~/.hermes/skills/research-brief/{references,templates}
```

### 2. 写 SKILL.md

把下面内容完整复制：

```markdown
---
name: research-brief
description: "把一组 URL 整理成入门 / 进阶 / 坑三栏 Markdown 笔记"
version: 0.1.0
metadata:
  hermes:
    tags: [research, writing, markdown]
    category: productivity
    requires_toolsets: [web, file]
---

# Research Brief

## When to Use
用户给了 3+ 个 URL，要求整理成笔记。

## Procedure
1. 用 web_extract 读取每篇
2. 按「入门 / 进阶 / 坑」分类
3. 写 notes.md
4. 附件：notes.html（同内容，简单 CSS）

## Defaults
- 输出目录：/tmp
- 语言：如果用户输入主要是中文，输出中文
- 每条附带 1 句「印象」

## Verification
确认 notes.md 和 notes.html 都存在，且尺寸 > 0。
```

### 3. 测试

```bash
hermes
/research-brief
```

Hermes 应该会展示这个 Skill 的说明，然后你可以直接发 URL 让它执行。

---

## 原理（一句）

Skill = 把一次做对的事，变成每次都能复用的文档。  
Hermes 每次只读摘要（Level 0），需要时才读完整内容（Level 1）。你不用担心 token 浪费。

---

## 进阶

- 给 Skill 加模板：`templates/research-brief.md`
- 放到 GitHub 仓库，团队共用
- 用 Cron 触发：每周一早上自动跑

---

**下一步（§05）**：让 Hermes 跨 session 记得你。

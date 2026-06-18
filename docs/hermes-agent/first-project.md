# §03 第一个完整项目：资料整理机器人

不是 Hello World。  
你要的是**给 Hermes 一堆乱链接，它输出结构化 Markdown**。  
这一节你走一遍，之后一年里这个模式都在复用。

---

## Case

你今天需要整理 5 篇关于 Hermes Agent 的博客。  
交给它：抓取、分类、润色、输出 `notes.md`。

---

## Step 1 选择入口

```bash
cd ~/projects/hermes-handbook
hermes --tui
```

> 为什么不是 CLI 不是 Gateway：第一次做项目，TUI 可见性最高，能看清每一步工具调用。

---

## Step 2 给第一轮 Prompt

把下面这段原样贴进去：

```
我在 /tmp/input.md 里放了 5 个 URL。请按下面要求整理：
1. web_search / web_extract 抓取每一篇
2. 按「入门 / 进阶 / 注意事项」三类归纳
3. 写 /tmp/notes.md，格式：
   # 标题
   ## 来源
   ## 要点（bullet）
   ## 印象（1 句话）
```

然后按回车。

---

## Step 3 观察 Hermes 怎么干活

你会看到类似这样的输出序列：

```
调用工具: web_search(query="Hermes Agent setup 2026")
调用工具: web_extract(url="...")
调用工具: write_file(path="/tmp/notes.md", content="...")
```

**不代表你要读懂每一步**，但你要确认：
- 它在调用 web_extract，而不是自己瞎编
- 最终产物真实落盘
- 没有 `404` 或 `401` 这类网络错误

如果结果不好，下一步接着追。

---

## Step 4 不满意的地方继续追

把下面补进对话：

```
notes.md 中文还不够通顺，每节后面的「印象」改成附加一句我作为初学者的真实感受，语气像博客。
```

Hermes 会基于上次产物直接修改，不用重新描述任务。

---

## Step 5 扩展

再追加一轮：

```
加一个 /tmp/notes.html，和 notes.md 内容一致，但用简洁的 markdown 风格 CSS。
```

---

## 验证清单

- [x] `web_search` 被调用
- [x] 抓取了至少 3 个不同来源
- [x] `notes.md` 真实写入磁盘
- [x] Hermes 在二三轮里记住了上轮结果
- [x] HTML 格式可用

---

## 原理（一句）

turns、工具调用、输出就是 Hermes 的最小工作单元。  
你在这个例子里看到的流程，就是后续所有复杂项目的基础形态。

---

**下一步（§04）**：把这套整理流程变成随时可复用的 Skill。

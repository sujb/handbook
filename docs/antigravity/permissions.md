# 第四部分 · 你怎么管控它

---

## §09 权限与审核三层

### 三层结构

Antigravity 的权限不是"开/关"二态，而是三层配置：

#### 1. 全局默认（Global Default）

所有 Project 继承的基线权限。在 Antigravity 主 **Settings** 里配置。

#### 2. 项目级 Security Preset

每个 Project 可以在全局基础上叠加更严格的规则：

| Preset | 说明 |
|--------|------|
| **Default** | 继承全局权限，不额外限制 |
| **Require manual review** | 所有终端命令和文件访问都需人工审核后才能执行 |

设置路径：Project 设置 → Security Preset → 选 Require manual review

#### 3. Secure Mode + Auto Execution 关闭

这是最严格的组合：
- **Secure Mode**：阻断网络访问、防止 workspace 外写入、沙箱化所有命令
- **Auto Execution 关闭**：Agent 每执行一个动作都要你确认

#### 可以叠加的细粒度规则

- **Local Permissions**：白名单 / 黑名单特定文件路径和 URL
- **MCP Tool Configuration**：项目级别启用 / 禁用特定 MCP 服务器

---

## §10 沙箱边界

### Secure Mode 的三条红线

1. **网络阻断**：Agent 无法访问外部网络
2. **文件写入限制**：不能写 project 文件夹以外的位置
3. **命令沙箱**：所有 shell 命令都在受限环境执行

### 沙箱逃逸漏洞历史

2026 年 4 月，Pillar Security 披露了一个严重漏洞：

- **漏洞位置**：`find_by_name` 工具的 `Pattern` 参数
- **问题**：参数直接传给 `fd` 命令，没有验证和终止符
- **绕过方式**：通过 prompt injection 先写一个恶意文件，再用 `-Xsh` 参数触发执行
- **影响范围**：Secure Mode + Auto Execution 关闭的最严配置也未能阻止
- **修复状态**：Google 在 2026 年 2 月底已修补

### 对你的实际意义

- **保持最新版本**：沙箱漏洞主要影响旧版
- **不要在 Antigravity 里处理敏感凭证**：即使是沙箱内，也不要让 Agent 访问生产密钥
- **慎开网络权限**：如果必须联网，只在必要时临时开，用完关掉

---

## §11 第一个完整项目：代码审查助手

### Case

你有一个 Git 仓库，每次有人提 PR，你希望 Antigravity 帮你：
1. 看 diff
2. 对照编码规范 review
3. 输出一份结构化报告

### Follow

#### Step 1：创建 Project

```
1. 本地创建文件夹：~/agy2-projects/code-review-demo
2. Desktop App → Select Project → New Project
3. Add Folder → 选刚才的文件夹
4. Security Preset：Default（先跑通，后面再加审核）
5. 命名 → Create
```

#### Step 2：准备知识文件

在项目文件夹里创建 `KNOWLEDGE.md`：

```markdown
# 编码规范
- 使用 TypeScript strict 模式
- 函数长度不超过 50 行
- 所有 public 方法必须有 JSDoc
- 不用 `any`
- 异步函数统一用 `async/await`，不用回调
```

#### Step 3：写第一条指令

```
我在这个项目中有一份编码规范在 KNOWLEDGE.md。
请模拟一个 PR review：
1. 生成一段 30 行的 TypeScript diff（包含 2 个违反规范的点）
2. 对照 KNOWLEDGE.md 逐条检查
3. 输出 /tmp/review.md，格式：
   # PR Review Report
   ## Summary
   ## Violations
   ## Suggestions
```

#### Step 4：调权限

如果第一次跑得太顺（Agent 直接改了文件），去 Project Settings：
- Security Preset → Require manual review
- 重新跑一遍，观察 Agent 每步都来问你

#### Step 5：加自动化

在输入框里：

```
把这个 review 流程做成一个 Schedule：
- 每天下午 6 点自动跑一次
- 输入是当天新的 commits
- 输出写到 /tmp/daily-review.md
```

### 验证清单

- [x] `KNOWLEDGE.md` 被 Agent 读取
- [x] Agent 能基于规范找出 violations
- [x] 报告文件真实写入磁盘
- [x] 加了审核模式后 Agent 会询问再执行
- [x] Schedule 能定时触发

---

**第五部分（§12-§14）**：创作层——Plugins、SDK、Loop 设计。

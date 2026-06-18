# 第五部分 · 怎么创作

---

## §12 创建你的第一个 Plugin

### 目录结构

一个最小的 Antigravity Plugin：

```
my-first-plugin/
├── manifest.json
├── README.md
└── src/
    └── index.ts
```

### manifest.json

```json
{
  "name": "my-first-plugin",
  "version": "0.1.0",
  "description": "我的第一个 Antigravity 插件",
  "entry": "src/index.ts",
  "permissions": ["read", "write"],
  "hooks": ["onTaskStart", "onTaskComplete"]
}
```

### src/index.ts 骨架

```typescript
export async function onTaskStart(task: any) {
  console.log(`[my-first-plugin] 任务开始: ${task.id}`);
}

export async function onTaskComplete(task: any) {
  console.log(`[my-first-plugin] 任务完成: ${task.id}`);
}
```

### 本地调试

```bash
# 在 Desktop App 或 CLI 中加载本地插件
antigravity plugin load ./my-first-plugin

# 查看已加载的插件
antigravity plugin list
```

### 注意点

- Plugin 的 hooks 只能拦截 Antigravity 暴露的事件，不能访问底层 harness
- `permissions` 字段声明插件需要的能力，超范围的会被静默拒绝
- 目前社区更常用 **Skills** 而不是 Plugins 扩展行为，Plugins 更多用于深度集成

---

## §13 精通 Antigravity SDK

### Python SDK 调用

```python
from antigravity import Agent, Project

# 创建 Agent
agent = Agent(model="gemini-2.5-flash")

# 加载 Project
project = Project(path="./my-project")

# 执行任务
result = agent.run(
    project=project,
    prompt="Review the latest commit and output a report"
)

print(result.artifacts)
```

### 构建自定义 Agent

```python
from antigravity import Agent, SubAgent

class ReviewAgent(Agent):
    def __init__(self):
        super().__init__(
            model="gemini-2.5-flash",
            system_prompt="你是一个严格的代码审查员。"
        )

reviewer = ReviewAgent()

# 让主 Agent 把 review 子任务外包给 reviewer
main_agent.run(
    prompt="Review PR #123",
    subagents=[reviewer]
)
```

### 多模型切换

```python
agent = Agent(model="gemini-2.5-flash-lite")  # 低成本
agent = Agent(model="gemini-2.5-flash")       # 均衡
agent = Agent(model="gemini-2.5-pro")         # 复杂任务
```

### 与外部 API 对接

```python
import requests
from antigravity import Agent

def post_to_slack(report: str):
    requests.post(
        "https://hooks.slack.com/services/xxx",
        json={"text": report}
    )

agent = Agent()
result = agent.run("Review latest commit and output a report")
post_to_slack(result.artifacts[0].content)
```

---

## §14 设计循环：让 Antigravity 自己干活

### Loop Engineering 在 Antigravity 中的落地点

Antigravity 2.0 提供了三个可以串成 Loop 的原语：

| 原语 | 作用 | 类比 Hermes |
|------|------|-------------|
| **Schedule** | 定时触发 | Cron |
| **Hooks** | 拦截和注入行为 | Skills / middleware |
| **Subagents** | 把复杂任务拆成多个子代理并行 | delegate_task |

### 一个真实的 Loop 设计

```
每天 9:00 [Schedule 触发]
    ↓
加载最新的 commits [Agent 读取 Git]
    ↓
跑 Review 子代理 [Subagent]
    ↓
结果写文件 [Agent write_file]
    ↓
发 Slack [Hook 或外部脚本]
    ↓
失败 → 重试一次 [Schedule 本身的重试]
```

### 验证与回退

- **验证**：Schedule 执行后检查输出文件是否存在且非空
- **回退**：如果子代理失败，主 Agent 可以把任务重新排队或发通知
- **不要做的**：让 Loop 无限嵌套，Antigravity 目前不支持子代理再触发 Schedule

---

**第六部分（§15-§16）**：边界与成本。

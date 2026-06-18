# 第六部分 · 边界与成本

---

## §15 成本：Token、延迟、Gemini API 配额

### 不同任务的实际消耗（估算）

| 任务类型 | 输入 Token | 输出 Token | 预估成本 |
|----------|-----------|-----------|---------|
| 简单问答 | 1K-5K | 200-500 | 极低 |
| 代码审查（单文件） | 10K-50K | 2K-5K | 低 |
| 代码审查（多文件） | 100K-500K | 5K-15K | 中 |
| 多 Subagent 并行 | 500K-1M+ | 10K-30K | 中高 |
| 复杂 Loop 任务 | 1M+ | 20K-50K | 高 |

### 免费额度的边界

- Gemini 免费 tier 有日/月调用上限
- SDK 调用走你自己的 API Key，**你自己承担费用**
- Desktop App 和 CLI 的非付费用户有使用限制，Google I/O 2026 后部分免费功能缩水

### 怎么估算成本

1. **先跑一次小任务**，看实际 token 消耗（API 响应里有 `usage` 字段）
2. 按 `input_price × input_tokens + output_price × output_tokens` 估算
3. **Subagent 会成倍放大**：1 个主 Agent + 3 个子代理 = 近似 4 倍成本
4. **Schedule 任务要算累计**：每天跑一次 = 30 倍月成本

---

## §16 常见坑与排错

### 安装失败

| 症状 | 排查 |
|------|------|
| `command not found` | 重启 Terminal，或手动把 `~/.local/bin` 加到 PATH |
| 登录后闪退 | 检查 Chrome 是否已安装，/browser 需要 Chrome |
| 插件安装失败 | 检查 manifest.json 格式，用 `antigravity plugin validate` 验证 |

### Context 超限

- **症状**：Agent 开始忘记前面的指令
- **原因**：对话历史太长，超过了 Gemini 的上下文窗口
- **解决**：新建 Project 或新建对话，Antigravity 会自动摘要旧对话
- **预防**：复杂任务拆成多个小任务，不要让一个对话跑几万 token

### 插件冲突

- **症状**：装了多个 Skills 后，Agent 行为异常
- **排查**：MCP Tool Configuration 里逐个禁用，找冲突来源
- **解决**：项目级 MCP 配置，不要把所有 Skills 都全局启用

### Schedule 不触发

- 检查 Desktop App 是否在运行（Schedule 依赖 App 或 CLI 常驻）
- CLI 版 Schedule 需要保持 terminal 进程活着，或用 systemd 托管

---

**附录**：命令速查表。

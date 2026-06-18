# 附录 A · 术语表

| 术语 | 英文 | 说明 |
|------|------|------|
| Antigravity | Antigravity | Google 的 agent-first 开发生态 |
| Agent Harness | Agent Harness | Agent 运行底座，四层产品共用 |
| Project | Project | Antigravity 的核心组织单元，替代了原来的 workspace |
| Knowledge | Knowledge | 项目级背景文档，Agent 自动读取 |
| Hook | Hook | JSON 格式的行为拦截器 |
| Subagent | Subagent | 主 Agent 派生的子代理，用于并行子任务 |
| Plugin | Plugin | 深度集成的扩展（目录 + manifest + 代码） |
| Skill | Skill | 较轻量的行为扩展，主要是 prompt 模板 |
| Secure Mode | Secure Mode | 最严格的安全预设：断网 + 沙箱 + 防 workspace 外写入 |
| Slash Command | Slash Command | 以 `/` 开头的内置快捷指令 |
| Schedule | Schedule | 定时或循环触发 Agent 任务 |

---

# 附录 B · 官方资源索引

| 资源 | 链接 | 说明 |
|------|------|------|
| 官网 | https://antigravity.google | 产品首页 |
| 下载页 | https://antigravity.google/download | Desktop / CLI / IDE 下载 |
| 官方 Blog | https://antigravity.google/blog | 产品更新和深度文章 |
| CLI 文档 | https://antigravity.google/docs/cli-features | CLI 功能说明 |
| 迁移文档 | https://antigravity.google/docs/gcli-migration | Gemini CLI → Antigravity CLI |
| Codelab | https://codelabs.developers.google.com/getting-started-google-antigravity | 官方入门教程 |
| Google Developers Blog | https://developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli/ | Gemini CLI 退役公告 |
| CLI GitHub | https://github.com/google-antigravity/antigravity-cli | CLI 源码和 issue |
| 社区论坛 | https://discuss.ai.google.dev | 官方社区 |

---

# 附录 C · Slash Commands 速查

> 信息来源：Google Codelabs + Antigravity Blog + 社区
> 标记 ⚠️ 的表示该命令的完整行为尚未在官方文档中完全公开。

| 命令 | 作用 | 可用入口 | 备注 |
|------|------|----------|------|
| `/browser` | 启动浏览器子代理，自动化 Chrome 任务 | Desktop / IDE / CLI | 需要 Chrome + 调试权限 |
| `/schedule` | 设置一次性或循环定时任务 | Desktop / IDE / CLI | UI 里也有 Schedule 入口 |
| `/goal` | 持续执行直到任务完成，不中断询问 | Desktop / IDE / CLI | 适合长任务 |
| `/grill-me` | Agent 先追问澄清需求，再动手 | Desktop / IDE / CLI | 降低需求歧义 |

⚠️ **说明**：
- 以上 4 个是截至 2026 年 6 月公开确认的 Slash Commands
- Antigravity CLI 支持 tab completion，输入 `/` 后会弹出可用命令列表
- Inline commands（旧版 Gemini CLI 的机制）正在逐步移除，自定义命令建议做成 Skills / Plugins
- 官方完整命令列表尚未公开，后续版本会补充

---

# 附录 D · Native Tools 速查

> Native Tools 是 Agent 可以直接调用的底层工具，不需要走 Slash Command。

| 工具 | 作用 | 来源 |
|------|------|------|
| `find_by_name` | 用 `fd` 搜索文件/目录 | 官方文档 + 安全研究披露 |
| Shell commands | 执行终端命令 | 受 Secure Mode 限制 |
| File read/write | 读写项目文件 | 受 Local Permissions 限制 |

⚠️ **说明**：
- Antigravity 的 Native Tools 完整清单**尚未公开**，目前只确认了 `find_by_name`
- Pillar Security 的安全研究披露了 `find_by_name` 的 schema 结构，说明还有更多 native tools 存在
- 建议参考官方 Codelab 和 SDK 文档获取最新工具列表

---

# 附录 E · 推荐 Skills / Plugins 清单

> 以下为社区和官方推荐，截至 2026 年 6 月。

**官方插件（随安装可选）**
- Google Developer Tools 插件：预置 Google 云服务的 Skills

**社区方向（搜索关键词）**
- GitHub 搜 `antigravity plugin` / `antigravity skill`
- Google Antigravity GitHub org：https://github.com/google-antigravity
- 社区论坛：https://discuss.ai.google.dev

⚠️ **说明**：Antigravity 的插件生态仍在早期，目前没有像 Claude Code 那样成熟的第三方插件市场。建议关注官方 Blog 和 GitHub 获取更新。

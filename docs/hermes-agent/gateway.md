# §07 接入平台：Gateway 实战

Gateway 让 Hermes 从“你自己的终端助手”变成“群里能被 @ 的公共助手”。

---

## Case

你在一个飞书群里，想直接 @hermes 让它回答问题，不用每次开终端。

---

## Follow

### 1. 在飞书开放平台创建应用

- App 名称：Hermes
- 启用机器人能力
- 获取 App ID / App Secret / Encrypt Key / Verification Token
- 配置事件订阅地址（后面会用到）

### 2. 启动 Gateway

```bash
hermes gateway setup
```

选飞书，填入上面四项信息。

### 3. 本地调试模式（推荐先做）

```yaml
# ~/.hermes/config.yaml
gateway:
  webhook:
    host: 0.0.0.0
    port: 8080
```

用 ngrok 做内网穿透：

```bash
ngrok http 8080
```

把 ngrok 生成的 URL 填回飞书开放平台的事件订阅地址。

### 4. 在群里 @hermes

发一条消息：

```
@hermes 用中文简要说明 Hermes Agent 是什么
```

期待她能正常回复。失败的常见原因：
- 没有给应用加群聊权限
- Encrypt Key / Verification Token 填错
- 端口没对外

---

## 原理（一句）

Gateway 是 Hermes 的“适配器层”。核心 Agent 不变，只换前面的接收/发送渠道。

---

## 注意点

- 公网暴露时建议加一层鉴权
- 避免把同一个 Gateway 同时用作开发和团队环境
- 会把 Hermes 的回复当作主消息返回，注意群频率

---

**下一步（§08）**：并行与子代理：`delegate_task`。

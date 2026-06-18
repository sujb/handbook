# §09 生产级部署

这一节的目标：让 Hermes 在一台 VPS 上 24 × 7 运行。

---

## Case

你把 Hermes 装在一台云服务器上，希望它：
- 开机自启
- 异常后自动重启
- 定期备份记忆和 Skills
- 支持你远程登录调试

---

## Follow

### 1. Docker Compose（快速）

```yaml
# /opt/hermes/docker-compose.yml
services:
  hermes:
    image: hermes-agent:latest
    restart: always
    volumes:
      - /root/.hermes:/root/.hermes
    environment:
      - TZ=Asia/Shanghai
```

```bash
docker compose -f /opt/hermes/docker-compose.yml up -d
```

### 2. systemd（更稳妥）

如果想用 systemd：

```ini
# /etc/systemd/system/hermes.service
[Unit]
Description=Hermes Agent
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/hermes --tui
Restart=always
Environment=TZ=Asia/Shanghai
WorkingDirectory=/root

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable --now hermes
```

### 3. 备份脚本

```bash
#!/usr/bin/env bash
# daily backup
tar -czf /opt/backups/hermes-$(date +%F).tar.gz /root/.hermes
```

加进 crontab：

```
0 2 * * * /opt/scripts/hermes-backup.sh
```

---

## 原理（两句）

- 64K context 是硬约束。低于 64K 的本地模型往往不够用
- 系统提示一旦中途作废，token 消耗可能上涨 10 倍

---

## 注意点

- Hermes 所有的状态都在 `~/.hermes/`。备份这一目录 = 完整备份
- 公网暴露时，Gateway 建议加一层 IP 或 JWT 限制
- 长期运行记得把 MEDIA_PATH 外挂到独立盘

---

**下一步（§10）**：进阶阅读。

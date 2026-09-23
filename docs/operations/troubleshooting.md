---
title: 服务管理与故障排查
nav_order: 20
---

# 服务管理与故障排查

## 服务总览

| 服务 | 作用 | 配置或定义 |
| --- | --- | --- |
| `mihomo.service` | 代理核心 | `/etc/mihomo/config.yaml` |
| `frpc.service` | 公网端口映射客户端 | `/etc/frp/frpc.toml` |
| `campus-login.timer` | 定时校园网登录 | `/etc/campus-login/` |
| `nginx.service` | 本机 Web 服务/反向代理 | `/etc/nginx/` |
| `docker.service` | 容器运行时 | Docker 配置 |

## 通用 systemd 命令

```bash
systemctl status <service>
sudo systemctl restart <service>
sudo systemctl enable <service>
sudo systemctl disable <service>
sudo journalctl -u <service> -n 100 --no-pager
sudo journalctl -u <service> -f
```

修改 systemd 单元文件后，需要重新加载配置：

```bash
sudo systemctl daemon-reload
```

## 端口检查

查看监听端口：

```bash
ss -lntp
```

常用端口：

| 端口 | 组件 |
| --- | --- |
| `22` | SSH |
| `7890` | Mihomo 代理 |
| `9090` | Mihomo 管理接口 |
| `6000` | 公网跳板机上的 SSH 映射，不是本机监听端口 |

## 公网 SSH 无法连接

按以下顺序排查：

```bash
systemctl status frpc --no-pager
sudo journalctl -u frpc -n 100 --no-pager
ss -lnt | rg ':22\b'
sudo /etc/frp/frpc verify -c /etc/frp/frpc.toml
```

本机 SSH 正常但公网仍无法连接时，问题通常在公网服务器上的 `frps`、认证配置或跳板机端口策略。

## 外网访问失败

```bash
systemctl status mihomo --no-pager
ss -lnt | rg ':7890\b'
curl --proxy http://127.0.0.1:7890 https://www.google.com -I
```

如果代理服务正常但节点不可用，打开 Mihomo 管理页面检查节点和流量状态；如果服务未运行，查看：

```bash
sudo journalctl -u mihomo -n 100 --no-pager
```

## 校园网掉线

```bash
systemctl status campus-login.timer --no-pager
sudo systemctl start campus-login.service
sudo journalctl -u campus-login.service -n 100 --no-pager
python3 /etc/campus-login/srun.py check
```

若账号密码、校园网参数发生变化，修改 `/etc/campus-login/campus_login_xuetao.env`，并确认权限为 `600`。

## 重启后的检查清单

```bash
systemctl is-active mihomo
systemctl is-active frpc
systemctl is-active campus-login.timer
ss -lnt | rg ':22\b|:7890\b|:9090\b'
```

如果 `frpc` 在开机后失败但手动启动成功，可以先执行：

```bash
sudo systemctl restart frpc
```

长期修复应检查网络就绪依赖和 `journalctl -b -u frpc` 的具体错误，不要只依赖手动重启。

---
title: frp 公网跳板
nav_order: 11
---

# frp 公网跳板

服务器使用 frp 客户端把本机服务映射到公网跳板机。相关文件位于：

```text
/etc/frp/
```

主要文件：

```text
/etc/frp/frpc       frp 客户端程序
/etc/frp/frpc.toml  客户端配置
/etc/frp/frps       frp 服务端程序
/etc/frp/frps.toml  服务端配置文件
```

本机实际运行的是 `frpc.service`；`frps` 通常运行在公网服务器上，本机不要同时启动服务端，除非有明确的部署需求。

## 当前映射

客户端配置中的映射关系如下：

| 名称 | 本机地址 | 公网跳板机端口 | 用途 |
| --- | --- | --- | --- |
| `test-tcp` | `127.0.0.1:22` | `6000` | 公网 SSH |
| `web-9090` | `10.108.119.152:9090` | `9090` | Mihomo 管理接口 |

## 公网 SSH

```bash
ssh -p 6000 <user_name>@101.200.12.244
```

这里的 `6000` 是公网跳板机上的 frp 远程端口，连接最终会转发到本机 SSH 的 `22` 端口。

## 服务管理

```bash
sudo systemctl status frpc
sudo systemctl restart frpc
sudo journalctl -u frpc -f
```

服务定义和配置分别位于：

```text
/etc/systemd/system/frpc.service
/etc/frp/frpc.toml
```

服务已经设置为开机启动。检查是否启用：

```bash
systemctl is-enabled frpc
```

## 配置检查

修改 `frpc.toml` 前建议先备份：

```bash
sudo cp -a /etc/frp/frpc.toml \
  "/etc/frp/frpc.toml.$(date +%Y%m%d-%H%M%S).bak"
sudoedit /etc/frp/frpc.toml
```

检查语法并重启：

```bash
sudo /etc/frp/frpc verify -c /etc/frp/frpc.toml
sudo systemctl restart frpc
```

如果日志显示无法登录服务器，依次检查本机网络、`serverAddr`/`serverPort`、认证配置以及公网服务器上的 `frps` 状态。不要把认证 token 或密钥提交到文档仓库。

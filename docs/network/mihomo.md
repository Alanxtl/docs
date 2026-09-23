---
title: Mihomo 代理
nav_order: 10
---

# Mihomo 代理

服务器使用 Mihomo 裸核心提供代理服务，程序由 systemd 管理。

## 文件位置

```text
程序：/usr/local/bin/mihomo
工作目录：/etc/mihomo/
主配置：/etc/mihomo/config.yaml
管理页面：/etc/mihomo/ui/
```

配置中当前的关键端口：

| 端口 | 用途 |
| --- | --- |
| `7890` | mixed-port，同时支持常见 HTTP/SOCKS 代理用法 |
| `9090` | Mihomo external controller 和 Web 管理页面 |

当前配置允许局域网访问代理，管理页面可通过以下地址打开：

```text
http://10.108.119.152:9090/ui
```

管理接口启用了认证密钥。密钥只应保存在本机配置和受信任的客户端中，不要写入文档或提交到 GitHub。

## 服务管理

```bash
sudo systemctl status mihomo
sudo systemctl restart mihomo
sudo systemctl reload mihomo
sudo journalctl -u mihomo -f
```

服务定义位于：

```text
/etc/systemd/system/mihomo.service
```

它以 `/etc/mihomo` 作为工作目录启动：

```text
/usr/local/bin/mihomo -d /etc/mihomo
```

## 修改配置

修改前先备份配置：

```bash
sudo cp -a /etc/mihomo/config.yaml \
  "/etc/mihomo/config.yaml.$(date +%Y%m%d-%H%M%S).bak"
sudoedit /etc/mihomo/config.yaml
```

修改后重载：

```bash
sudo systemctl reload mihomo
sudo systemctl status mihomo --no-pager
```

如果重载后服务异常，查看日志并恢复最近一次备份：

```bash
sudo journalctl -u mihomo -n 100 --no-pager
sudo systemctl restart mihomo
```

## 基础检查

```bash
ss -lnt | rg ':7890|:9090'
nc -zv 127.0.0.1 7890
curl --proxy http://127.0.0.1:7890 https://www.google.com -I
```

如果 `7890` 没有监听，优先检查 `mihomo.service` 状态和日志。若代理端口正常但外网不可用，在管理页面检查当前节点、流量和规则状态。

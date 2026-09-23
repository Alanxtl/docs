---
title: 快速开始
nav_order: 2
---

# 快速开始

## 登录服务器

在校园网或已连通实验室内网的环境中，使用服务器的内网地址和 SSH：

```bash
ssh <user_name>@10.108.119.152
```

如果需要从公网访问，使用 frp 提供的跳板入口：

```bash
ssh -p 6000 <user_name>@101.200.12.244
```

公网 SSH 映射的详细说明见[公网跳板](network/frp.md)。

## 登录后检查

登录后，终端会显示校园网和代理状态。也可以手动检查：

```bash
systemctl --no-pager status mihomo
systemctl --no-pager status frpc
systemctl --no-pager status campus-login.timer
ss -lnt | rg ':7890|:9090|:22'
```

## 常用入口

| 用途 | 地址或命令 |
| --- | --- |
| HTTP/SOCKS 代理 | `127.0.0.1:7890` |
| 局域网代理 | `10.108.119.152:7890` |
| Mihomo 管理页面 | `http://10.108.119.152:9090/ui` |
| 公网 SSH | `ssh -p 6000 <user_name>@101.200.12.244` |

管理页面端口属于管理接口，不要把它暴露到不可信的公网环境。

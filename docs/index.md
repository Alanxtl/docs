---
title: 文档
nav_order: 1
---

# 实验室服务器使用与运维

这是一份面向实验室成员的服务器使用和基础运维手册。

## 快速开始

- [快速开始](quickstart.md)：登录服务器、代理和公网 SSH 入口
- [常用命令与终端环境](shell.md)：启动提示、代理环境变量和命令别名

## 网络与远程访问

- [Mihomo 代理](network/mihomo.md)：代理服务、管理页面和配置维护
- [frp 公网跳板](network/frp.md)：通过公网跳板机 SSH 访问服务器
- [校园网自动登录](network/campus-login.md)：校园网认证和定时任务

## 运维

- [服务管理与故障排查](operations/troubleshooting.md)：systemd、日志和常见故障

> 文档中的密码、代理节点密钥和校园网账号均不应提交到 Git 仓库。

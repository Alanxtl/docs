---
title: 文档
nav_order: 1
---

# 实验室服务器使用与运维

这是一份面向实验室成员的服务器使用和基础运维手册。

## 服务器配置

| 项目 | 配置 |
| --- | --- |
| 主机名 | `bitse-SYS-740GP-TNRT` |
| 操作系统 | Ubuntu 22.04.4 LTS |
| 内核 | Linux 6.8.0-138-generic |
| CPU | 2 × Intel Xeon Silver 4310，48 线程 |
| 内存 | 251 GiB |
| GPU | 2 × NVIDIA GeForce RTX 3090，24 GiB 显存/卡 |
| GPU 驱动 | NVIDIA 595.71.05 |
| 系统盘 | 1 TB NVMe SSD，根分区约 884 GiB |
| 数据盘 | 7.3 TB，挂载于 `/home` |
| 内网地址 | `10.108.119.152` |

> 磁盘空间需要重点关注：当前 `/home` 使用率约 97%，新增数据或模型前请先清理空间。

## 快速开始

- [快速开始](quickstart.md)：登录服务器、代理和公网 SSH 入口
- [常用命令与终端环境](shell.md)：启动提示、代理环境变量和命令别名

## 网络与远程访问

- [Mihomo 代理](network/mihomo.md)：代理服务、管理页面和配置维护
- [frp 公网跳板](network/frp.md)：通过公网跳板机 SSH 访问服务器
- [校园网自动登录](network/campus-login.md)：校园网认证和定时任务

## 运维

- [服务管理与故障排查](operations/troubleshooting.md)：systemd、日志和常见故障

## 办事指南

- [实验室出国参会与报销指南](guides/overseas-conference-reimbursement.md)：出国参会审批、材料准备和回国报销流程

> 文档中的密码、代理节点密钥和校园网账号均不应提交到 Git 仓库。

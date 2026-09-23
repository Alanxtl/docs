---
title: 校园网自动登录
nav_order: 12
---

# 校园网自动登录

校园网登录程序位于：

```text
/etc/campus-login/
```

目录中的主要文件：

| 文件 | 用途 |
| --- | --- |
| `srun.py` | SRun 校园网登录脚本 |
| `campus_login_xuetao.sh` | 读取配置、检查状态并执行登录的包装脚本 |
| `campus_login_xuetao.env` | 账号、密码和校园网参数，包含敏感信息 |
| `README.md` | 本机部署说明 |

## 自动登录机制

systemd timer 每 5 分钟触发一次 `campus-login.service`。服务会：

1. 读取 `/etc/campus-login/campus_login_xuetao.env`；
2. 先检查当前是否已经在线；
3. 不在线时尝试登录；
4. 失败后写入 `/run/campus-login.last_fail`，在冷却时间内避免重复请求。

启动后也会在 Bash 提示中显示校园网状态，但 Bash 启动脚本只检查状态，不负责登录。

## 查看状态

```bash
systemctl status campus-login.timer
systemctl list-timers campus-login.timer
sudo systemctl status campus-login.service
sudo journalctl -u campus-login.service -n 100 --no-pager
```

手动触发一次：

```bash
sudo systemctl start campus-login.service
```

直接检查校园网状态：

```bash
python3 /etc/campus-login/srun.py check
```

成功时通常会看到类似：

```text
online: <账号> @ <在线IP> @ <套餐>
```

## 启用定时任务

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now campus-login.timer
```

## 修改账号配置

账号配置文件是：

```text
/etc/campus-login/campus_login_xuetao.env
```

该文件包含密码，不要复制到 GitHub 或聊天记录中。修改时使用：

```bash
sudoedit /etc/campus-login/campus_login_xuetao.env
sudo chown root:root /etc/campus-login/campus_login_xuetao.env
sudo chmod 600 /etc/campus-login/campus_login_xuetao.env
sudo systemctl start campus-login.service
```

如果登录失败，先看日志，再检查校园网认证地址、网卡 IP、账号状态和认证参数；不要通过反复重启服务绕过冷却机制。

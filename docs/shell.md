---
title: 常用命令与终端环境
nav_order: 3
---

# 常用命令与终端环境

## 登录启动提示

全局 Bash 启动配置位于：

```text
/etc/bash.bashrc
```

它会在打开交互式 Bash 时：

1. 检查校园网在线状态；
2. 检查本机代理端口 `127.0.0.1:7890`；
3. 在代理可用时设置当前会话的代理环境变量；
4. 必要时设置 Git 的 HTTP/HTTPS 代理；
5. 测试外网连通性并打印提示。

启动提示只负责检查状态，不负责执行校园网登录。校园网登录由 systemd 定时任务负责，见[校园网自动登录](network/campus-login.md)。

## 命令别名

服务器预设了若干现代命令替代品：

| 输入 | 实际命令 | 说明 |
| --- | --- | --- |
| `cat` | `batcat` | 带语法高亮和分页的文件查看 |
| `ls` | `eza` | 文件列表 |
| `ll` | `eza -l` | 详细文件列表 |
| `la` | `eza -A` | 包含隐藏文件的列表 |
| `grep` | `rg` | ripgrep 文本搜索 |
| `find` | `fdfind` | fd 文件查找 |

需要调用系统原生命令时，可以使用 `command` 或反斜杠绕过别名，例如：

```bash
command cat file.txt
\ls -la
```

## 代理环境变量

代理可用时，`/etc/bash.bashrc` 会设置 `http_proxy`、`https_proxy`、`ALL_PROXY` 等变量，默认指向：

```text
http://127.0.0.1:7890
```

某些工具不会自动读取这些变量，尤其是 conda、Git、Hugging Face 和 Docker。遇到下载失败时，先确认代理状态，再按工具自身的配置方式设置代理。

服务器共用一套 Conda 安装，首次使用时执行：

```bash
/usr/local/miniconda3/bin/conda init
```

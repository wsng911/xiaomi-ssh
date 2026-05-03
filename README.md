# xiaomi-ssh

一个基于 Docker 的小米路由器 SSH 自动化开启工具，支持漏洞检测、SSH 永久启用、固件备份等功能，无需手动配置 Python 环境，一条命令即可运行。

## 功能特性

- 🔑 自动开启 SSH 访问（永久生效，重启不失效）
- 🔍 自动检测路由器型号及固件版本
- 💾 完整固件备份（所有分区）
- 🌐 多语言界面支持（中文 / English / Русский）
- 🐳 Docker 一键运行，无需配置本地环境
- 📦 支持自定义固件刷写
- 🔧 支持 Breed / U-Boot 引导加载器安装

## 支持的设备

支持大多数小米/红米路由器，包括但不限于：

| 系列 | 型号 |
|------|------|
| 第三代 | R3, R3G, R3P, R3A, R3L |
| 第四代 | R4, R4A, R4C, R4AC |
| AC 系列 | AC2100, RM2100, AC2200, AC2350 |
| AX 系列 (Wi-Fi 6) | AX3600, AX6, AX9000, AX6000 |
| 企业级 | CR6606, CR6608, CR6609 |

## 快速开始

### 使用 Docker Hub（最简单）

```bash
docker run -it --network host wsng911/xiaomi-ssh:latest
```

### 本地构建

```bash
# 构建镜像
docker build -t xiaomi-ssh .

# 运行（需要与路由器在同一网络）
docker run -it --network host xiaomi-ssh
```

### 本地运行

```bash
# 要求 Python 3.8+
git clone https://github.com/wsng911/xiaomi-ssh.git
cd xiaomi-ssh
pip install -r requirements.txt
python3 run.py
```

## 使用说明

启动后进入交互菜单：

```
========================================================
xiaomi-ssh - 小米路由器 SSH 工具
1 - 设置路由器 IP（默认：192.168.31.1）
2 - 连接设备并安装漏洞利用
3 - 读取设备完整信息
4 - 创建完整固件备份
5 - 安装多语言包
6 - 开启永久 SSH
7 - 刷写固件
8 - 更多功能
9 - 重启设备
0 - 退出
```

**基本流程：**
1. 选 `1` 设置路由器 IP
2. 选 `2` 连接并安装漏洞利用（必须先执行）
3. 选 `6` 开启永久 SSH

SSH 默认凭据：`root` / `root`，端口 `22`

## 目录结构

```
xiaomi-ssh/
├── gateway.py          # 核心网关类（设备检测、SSH/Telnet/FTP 连接）
├── connect.py          # 漏洞利用入口（根据型号选择利用方式）
├── connect1~4.py       # 各型号漏洞利用实现
├── xmir_base/          # 基础库（telnetlib、fdt、ubireader）
├── data/
│   ├── payload/        # 漏洞利用 payload
│   └── payload_ssh/    # SSH 安装文件（多架构 dropbear）
├── bootloader/         # Breed / U-Boot 引导文件
├── firmware/           # 自定义固件存放目录
└── Dockerfile
```

## 注意事项

- 运行容器时必须使用 `--network host`，确保容器能访问路由器
- 操作前建议先执行选项 `4` 备份固件
- 部分型号需要特定固件版本才能利用漏洞，过新的固件可能已修复

## License

MIT License - Copyright (c) 2025 wsng911

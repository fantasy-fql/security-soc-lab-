## D2（周二，2h）环境：Ubuntu + Kali
1、 Ubuntu Server 22.04（50 min）
* 规格：2GB 内存 / 2 核 / 30GB 磁盘
* 安装时勾选 OpenSSH server（不勾后面要手动装）
* 用户名密码记在笔记里，实验环境统一用 root / root 这类方便记的密码

2、 Kali 预装镜像（40 min）
* 规格：4GB 内存 / 2 核（默认可能给 2GB，改到 4GB）
* 官方镜像默认账号 kali/kali，先改密码
* 开机后 sudo apt update 一次

3、 装 VMware Tools / open-vm-tools（20 min）
* Ubuntu：sudo apt install -y open-vm-tools
* 目的：宿主机与虚机互传文件（拖拽或共享文件夹），后面传日志、传样本全靠它

验收：三台虚机都能开机，Kali 能执行 nmap -V。
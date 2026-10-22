## D1（周一，2h）环境：VMware + Windows 虚机
前置：确认宿主机内存 ≥16GB（8GB 也能走，见文末降级方案）、磁盘剩余 ≥150GB。

1、 装虚拟化软件（40 min）

* 下载 VMware Workstation Pro（现个人免费）或 Player。VirtualBox 也行，但快照和克隆体验差一些。

* 确认 BIOS 里 VT-x/AMD-V 已开启（任务管理器 → 性能 → 虚拟化：已启用）。

2、 准备 ISO（20 min，边下边干别的）

* Win10 或 Win11 消费版 ISO（微软官网 Media Creation Tool）
* Ubuntu Server 22.04 LTS（注意选 Server，不是 Desktop，省内存）
* Kali Linux 官方 VM 镜像（kali.org/get-kali 直接下预装好的 VMware 镜像，省 40 分钟安装时间，强烈推荐）

3、 建 Win10 虚机（60 min）

* 规格：4GB 内存 / 2 核 / 80GB 磁盘 / 单文件
* 安装时选“我没有产品密钥” → 专业版（专业版才有完整的安全策略和审核策略编辑器，家庭版做不了第 3–4 周的实验）
* 装完立刻做三件事：
    * 关掉“自动更新”暂停 35 天（否则每两周重启一次，实验中断）
    * 建一个本地管理员账号 lab 和一个普通账号 user1（后面做 4624/4625 实验要两个账号）
    * 装 VMware Tools（否则复制粘贴、拖文件都不方便）

验收：Win10 能正常开机、能上网、能复制粘贴宿主机文本。
常见坑：Win11 需要 TPM，VMware 里要手动加“可信平台模块”设备；嫌麻烦就直接用 Win10。


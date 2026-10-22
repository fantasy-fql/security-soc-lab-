## D3（周三，2h）环境：网络 + 互通 + 快照

1、 双网卡设计（40 min）
给 Kali 和 Win10 各配两块网卡：

| 网卡 | VMware 模式 | 作用 |
| --- | --- | --- |
| 网卡 1 | NAT（VMnet8） | 上外网、装工具、查资料 |
| 网卡 2 | 仅主机（VMnet1） | 攻击流量专用实局域网 |

Ubuntu 也接 VMnet1（做被攻击/被监控机），需要更新时临时加 NAT。

为什么必须这样：第 7–8 周你会跑 Atomic Red Team 和 Metasploit。

VMware 里改法：虚机设置 → 网络适配器 → 自定义 → 选 VMnet1。

2、 固定 IP（40 min）
在 VMnet1 上手工配静态地址，别用 DHCP（否则每次重启 IP 变，规则和剧本全废）：

| 主机 | VMnet1 地址 |
| --- | --- |
| Kali | 192.168.56.10 |
| Win10 | 192.168.56.20 |
| Ubuntu | 192.168.56.30 |

* Kali/Ubuntu：sudo nano /etc/network/interfaces 或 nmcli，也可以用 netplan（Ubuntu 22.04 是 /etc/netplan/*.yaml）
* Win10：网络适配器 → IPv4 属性 → 手动填

3、 互通验证（20 min）
在 Kali 上跑：

```bash
ping -c 3 192.168.56.20          # 通 Win10
ping -c 3 192.168.56.30          # 通 Ubuntu
nmap -sn 192.168.56.0/24         # 全网段发现
```
* Win10 默认禁 ping，先去防火墙开“文件和打印机共享(回显请求)”，或者临时关防火墙，否则你会白白排查半小时。
* 在 Win10 上 ping 192.168.56.30 反向验证。

4、 基线快照（20 min）
三台虚机各打一个快照，命名 BASE-clean-YYYYMMDD。以后每次做实验前打 pre-实验名，实验完直接回滚。

验收清单（打勾才算过）

- [ ] Kali 能 ping 通 Win10 和 Ubuntu
- [ ] Kali 能上网（curl -I https://www.baidu.com
- [ ] 三台都在 192.168.56.0/24 且 IP 固定
- [ ] 三个 BASE 快照已打
- [ ] 拓扑图画好并提交仓库（画图用 draw.io 免费的）
## D4（周四，2h）Linux ①：用户 / 权限 / SUID
安全运营排查入侵时，80% 的动作是“找一个不该存在的文件/权限/账号”。今天只学这些。

1、 用户与登录痕迹（40 min）

```bash
id                                # 当前用户和组
cat /etc/passwd | grep -v nologin # 有哪些可登录账号
cat /etc/passwd | awk -F: '$3==0' # UID=0 的账号 —— 排查后门账号第一招
last  | head -20                  # 成功登录历史
lastb | head -20                  # 失败登录历史（爆破痕迹）
w                                 # 当前在线用户
```
重点认知：/etc/passwd 里出现第二个 UID=0 的账号，就是后门。

2、 权限模型（40 min）

```BASH
ls -l /etc/shadow        # 看到 rw-r-----，理解 3 组权限
stat /etc/passwd         # 看完整权限+时间戳
umask                    # 默认 022 意味着什么
chmod 4755 test.sh       # 亲手做一个 SUID
ls -l test.sh            # 看那个 s 位
```

3、 SUID / 能力位排查（40 min） —— 提权与后门的经典手法

```BASH
find / -perm -4000 -type f 2>/dev/null        # 全盘 SUID 文件
getcap -r / 2>/dev/null                       # 有特殊能力的文件
find / -nouser -o -nogroup 2>/dev/null | head # 孤儿文件（删了账号但文件还在，入侵残留特征）
```
产出：02-quickref/linux-权限与账号排查.md，把你系统里实际的 SUID 列表贴进去（这就是你的基线，将来对比异常）。
---
title: linux-usage
category: computer
date: 2020-02-08 16:42:06
tags:
toc: true
---

一份我的 Linux 手扎

A paper of my Linux gists

<!-- more -->

## linux命令

### sed

从Next的Katex行内式 `$...$` 迁移到Icarus的Katex行内式 `\\(...\\)`
```console
$ sed -i 's,\$\([^$]*\)\$,\\\\(\1\\\\),g' file.md
```

### grep

根据文件内容搜索文件

```console
$ grep -iRl "your-text-to-find" ./
```

### rsync

1. 拷贝文件, 保留所有信息
   ```console
   $ rsync -aXHAv -P $SOURCE_DIR/ $TARGET_DIR/`
   ```
2. 仅复制, 不保留权限
   ```console
   $ rsync -rlt -P --no-owner --no-group --no-perms $SOURCE_DIR/ $TARGET_DIR/
   ```
3. 同步文件夹(小心, 有--delete参数, 会删光目标文件夹多余的文件)
   ```console
   $ rsync -aXHAz -v -P --exclude={"filename1","path/to/filename2"} --delete $SOURCE_DIR/ $TARGET_DIR/
   ```

### ss
ss is a member of iproute tools set
ss has only 22 parameters

列出套接字统计
```console
$ ss -s
```

查看端口占用
```console
# ss -nlptu | grep <端口号>
```

### dd

备份GPT分区表
```console
# dd if=/dev/sda of=gpt-partition.bin bs=512 count=34
```

恢复GPT分区表
```console
# dd if=gpt-partition.bin of=/dev/sda bs=512 count=34
```

### 生成随机密码

生成20个长度为12的 含大写字母 `-c` , 数字 `-n` , 符号 `-y` , 完全随机 `-s` 的密码

```console
$ pwgen -cnys 12 20
```

### Recursively chmod all directories except files

To recursively give directories read&execute privileges:

```console
# find /path/to/base/dir -type d -exec chmod 755 {} +
```

To recursively give files read privileges:

```console
# find /path/to/base/dir -type f -exec chmod 644 {} +
```

Reference from [StackExchange](https://superuser.com/questions/91935/how-to-recursively-chmod-all-directories-except-files)

### 带排除项删除文件和目录

```console
# shopt -s extglob    #打开extglob模式
# rm -fr !(filename)
```

### SWAP file

```console
# truncate -s 0 /swapfile
# chattr +C /swapfile
# btrfs property set /swapfile compression none
```

### ssh-keygen

1. 查看某个 key 的公钥指纹
   ```console
   $ ssh-keygen -l -f </path/to/ssh/key>
   $ ssh-keygen -l -E md5 -f <Your key> # MD5 格式
   ```
2. 修改某个 key 的 Comment
   ```console
   $ ssh-keygen -c -C <Your comment> -f <Your key>
   ```
3. 输出某个 key 的公钥格式
   ```console
   $ ssh-keygen -y -f <Your key>
   ```

### 泛域名证书

```console
$ certbot certonly --preferred-challenges dns --manual -d *.example.com
```

### Kernel interface

1. 获取uuid
   ```console
   $ cat /proc/sys/kernel/random/uuid
   ```
2. 查看熵池
   ```console
   cat /proc/sys/kernel/random/entropy_avail
   ```
3. 查看电池电量
   ```console
   $ cat /sys/class/power_supply/<Your battery name>/capacity
   ```

### iptables

1. 透明代理(TPROXY)
   ```console
   # 设置策略路由
   ip rule add fwmark 1 table 100
   ip route add local 0.0.0.0/0 dev lo table 100
   
   # 代理局域网设备
   iptables -t mangle -N V2RAY
   iptables -t mangle -A V2RAY -d 127.0.0.1/32 -j RETURN
   iptables -t mangle -A V2RAY -d 224.0.0.0/4 -j RETURN
   iptables -t mangle -A V2RAY -d 255.255.255.255/32 -j RETURN
   iptables -t mangle -A V2RAY -d 192.168.0.0/16 -p tcp -j RETURN # 直连局域网，避免 V2Ray 无法启动时无法连网关的 SSH，如果你配置的是其他网段（如 10.x.x.x 等），则修改成自己的
   iptables -t mangle -A V2RAY -d 192.168.0.0/16 -p udp -j RETURN # 直连局域网
   iptables -t mangle -A V2RAY -p udp -j TPROXY --on-port 12345 --tproxy-mark 1 # 给 UDP 打标记 1，转发至 12345 端口
   iptables -t mangle -A V2RAY -p tcp -j TPROXY --on-port 12345 --tproxy-mark 1 # 给 TCP 打标记 1，转发至 12345 端口
   iptables -t mangle -A PREROUTING -j V2RAY # 应用规则
   
   # 代理网关本机
   iptables -t mangle -N V2RAY_MASK
   iptables -t mangle -A V2RAY_MASK -d 224.0.0.0/4 -j RETURN
   iptables -t mangle -A V2RAY_MASK -d 255.255.255.255/32 -j RETURN
   iptables -t mangle -A V2RAY_MASK -d 192.168.0.0/16 -p tcp -j RETURN # 直连局域网
   iptables -t mangle -A V2RAY_MASK -d 192.168.0.0/16 -p udp ! --dport 53 -j RETURN # 直连局域网，53 端口除外（因为要使用 V2Ray 的 DNS）
   iptables -t mangle -A V2RAY_MASK -j RETURN -m mark --mark 0xff    # 直连 SO_MARK 为 0xff 的流量(0xff 是 16 进制数，数值上等同与上面V2Ray 配置的 255)，此规则目的是避免代理本机(网关)流量出现回环问题
   iptables -t mangle -A V2RAY_MASK -p udp -j MARK --set-mark 1   # 给 UDP 打标记,重路由
   iptables -t mangle -A V2RAY_MASK -p tcp -j MARK --set-mark 1   # 给 TCP 打标记，重路由
   iptables -t mangle -A OUTPUT -j V2RAY_MASK # 应用规则
   ```
   [参考来源](https://guide.v2fly.org/app/tproxy.html)
2. IPSET u32 匹配
   判断一个包的 TCP Seq 的最后一个值是否等于 41 : `0>>22&0x3C@ 4 &0xFF=0x29`
   举个例子(可以用 WireShark 抓包):
   ```
   Source IP: 121.41.89.52
   = 01111001 00101001 01011001 00110100B = 79 29 59 34H = 2032752948D
   IP Header：
   45 00 00 3c 00 00 40 00 31 06 ef 34 **79 29 59 34** c0 a8 c7 81
   TCP Header：
   00 50 95 3c 8d 7f 52 ac 69 15 33 be a0 12 71 20 cd dc 00 00 02 04 05 14 04 02 08 0a 08 c8 62 fa 00 1c 30 a1 01 03 03 07
   ```
   `0>>22` 的含义是从 IP 报头的 0 下标取 4 字节(共 32 位, u32 默认取 4 字节), 然后按位右移 22 位, 从而得到剩余的开头 10 位.
   如 `45 00 00 3c = 0100 0101 0000 0000 0000 0000 0011 1100` 右移 22 位
   得到 `1 14 = 01 0001 0100`
   
   后面的 `&0x3C` 的含义是和 `0x3C = 0011 1100` 进行按位与运算(实际上就是过滤)
   因此本例中 `0>>22&0x3C` 即 `01 0001 0100 & 00 0011 1100` 得到 `00 0001 0100`,
   
   通过这两个操作我们得到了 IP 头的第 4~7 位的值
   记录 IP 头长度的值是 IP 头的第 4~7 位的值值再加两个 0 也就是 `01 0100` (十进制的 20)
   `@` 的含义是根据左边的值推进指针, 本例中 `0>>22&0x3C@` 即推进 20 个字节

   剩下的也没什么好说的了,
   从 TCP 头第 4 下标处取 4 字节然后用掩码 `0xFF` 按位与取得其中的最后一个字节,
   然后比较是否等于 `0x29 = 41D`
   > 等号后可以是单个值也可以是一个区间, 如判断一个包的 TCP Seq 的最后一个值是否在 41~60 之间 `0>>22&0x3C@ 4 &0xFF=0x29:0x3C`

## bash

### if 判断

```
# 用于数字判断
-eq 相等
-ne 不等
-gt 大于
-ge 大于等于
-lt 小于
-le 小于等于

# 用于文件判断
-r 可读
-w 可写
-x 可执行
-f 是否是标准文件
-d 是否是目录
-c 是否是字符特殊文件
-b 是否是块特殊文件
-s 文件大小非0时为真
-t 当文件描述符(默认为1)指定的设备为终端时为真

# 用于字符串判断
string1 = string2 and string1 == string2 - The equality operator returns true if the operands are equal.
    Use the = operator with the test [ command.
    Use the == operator with the [[ command for pattern matching.
string1 != string2 - The inequality operator returns true if the operands are not equal.
string1 =~ regex- The regex operator returns true if the left operand matches the extended regular expression on the right.
string1 > string2 - The greater than operator returns true if the left operand is greater than the right sorted by lexicographical (alphabetical) order.
string1 < string2 - The less than operator returns true if the right operand is greater than the right sorted by lexicographical (alphabetical) order.
-z string - True if the string length is zero.
-n string - True if the string length is non-zero.

# 逻辑判断
-a 与
-o 或
! 非
```

## 字符串变量匹配

```
::x 正数时从左往右截取x个, 负数时从右往左截掉x个
:x 从x开始截取后面所有内容, 负数时 :(-x) 或者 : -x 从右往左截取所有内容
:x:y 从x开始截取y个字符
${food:-Cake} 若 $food 不存在则输出 "Cake"

STR="/path/to/foo.cpp"
echo ${STR%/*}   # /path/to     % 是从右向左截去, 单个 % 是非贪婪模式, 
echo ${STR#*/}   # path/to/foo.cpp   # 是从左向右截去, 单个 # 是非贪婪模式, 
echo ${STR%.cpp} # /path/to/foo 两个 %% 是贪婪模式
echo ${STR##*.}  # cpp          两个 ## 是贪婪模式
```

## ssh-agent 自启动

参考 [Archlinux Wiki](https://wiki.archlinux.org/index.php/SSH_keys#SSH_agents)

Windows用户见[Github Docs](https://help.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows)

## Github SSH 测试

先添加ssh key: [快捷通道](https://github.com/settings/keys)

然后:

```console
$ ssh -T git@github.com
```

### GitHub 的 SSH 密钥指纹

以下是 MD5 格式(十六进制格式):

* 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48 (RSA)
* ad:1c:08:a4:40:e3:6f:9c:f5:66:26:5d:4b:33:5d:8c (DSA)

以下是 SHA256 格式(base64 格式):

* SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8 (RSA)
* SHA256:br9IjFspm1vxR3iA35FWE+4VTyz1hYVLIE2t1/CeyWQ (DSA)

## GIT 常用命令

1. 友好地输出日志:
   ```console
   $ git log --graph --oneline --decorate --all
   ```
2. 输出最近一次commit及diff格式的改动:
   ```console
   git log --cc -1
   ```
3. 查看commit间的修改
   ```console
   git diff HEAD HEAD~
   git diff HEAD^! --stat
   ```
4. 撤销对某个文件的更改
   ```console
   # 工作区
   $ git checkout -- <file> # unstaged changes
   $ git reset HEAD <file>  # staged changes
   # 缓存区
   $ git restore --staged <file>
   ```
5. 撤销合并
   * 移动HEAD指针(有人在此间隔提交可能会丢失更改) `git reset --hard HEAD~<n>`
   * 新建一个revert提交(适用于多人合作) `git revert <Commit ID>`

## Git SSH 协议代理

```bash ~/.ssh/config
ProxyCommand nc -v -x 127.0.0.1:1080 %h %p

# 或者 socat (http代理)
ProxyCommand=socat - PROXY:127.0.0.1:%h:%p,proxyport=1080
```

## 更好的Submodule

使用[git-subrepo](https://github.com/ingydotnet/git-subrepo)

然后使用`git subrepo`为开头

还可将子目录变为子仓库:

```console
$ git subrepo init <subdir> [-r <remote>] [-b <branch>] [--method <merge|rebase>]
```

## Git LFS

https://zzz.buzz/zh/2016/04/19/the-guide-to-git-lfs/

## Git Cherry-pick

git cherry-pick用于把另一个本地分支的commit修改应用到当前分支

假设`dev`分支有一个hash为`38361a68`的commit
```
$ git checkout master
$ git cherry-pick 38361a68
```

## Git Format-patch

1. 两个commit间的修改(包含两个commit)
```console
$ git format-patch <r1>..<r2>
```

2. 单个commit
```console
$ git format-patch -1 <r1>
```

3. 从某commit以来的修改(不包含该commit)
```console
$ git format-patch <r1>
```

4. 应用.patch文件
```console
$ git am 0001-xxxx.patch
```

## 限制 git-gc 的内存占用

```console
$ git config --global pack.windowMemory "100m"
$ git config --global pack.packSizeLimit "100m"
$ git config --global pack.threads "1"
```

## X11vnc startup

With SDDM and SSH Tunnel.
Please be aware that this command need to be executed on client-side.

```console
ssh -t -L 5900:localhost:5900 <REMOTE HOST> 'sudo x11vnc -localhost -display :0 -auth $(find /var/run/sddm/ -type f)'
```
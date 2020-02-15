---
title: linux
category: computer
date: 2020-02-08 16:42:06
tags:
---

一份我的Linux手扎

命令只记录篇幅不大的

<!-- more -->

## linux命令

### grep

根据文件内容搜索文件

```bash
$ grep -iRl "your-text-to-find" ./
```

### rsync

#### Rsync常用命令

拷贝文件, 保留所有信息

```bash
$ rsync -aXv -P $SOURCE_DIR/ $TARGET_DIR/`
```

仅复制, 不保留权限

```bash
`rsync -rlt -P --no-owner --no-group --no-perms $SOURCE_DIR/ $TARGET_DIR/`
```

同步文件夹(小心, 有--delete参数, 会删光目标文件夹多余的文件)

```bash
`rsync -aXv -P --delete $SOURCE_DIR/ $TARGET_DIR/`
```

#### 参数详解

```bash
-a, --archive               archive mode; equals -rlptgoD (no -H,-A,-X)
-X, --xattrs                preserve extended attributes
-v, --verbose               increase verbosity

-r, --recursive             recurse into directories
-l, --links                 copy symlinks as symlinks
-p, --perms                 preserve permissions
-t, --times                 preserve modification times
-g, --group                 preserve group
-o, --owner                 preserve owner (super-user only)
-D                          same as --devices --specials

--delete                delete extraneous files from destination dirs
-P                          same as --partial --progress
--partial               keep partially transferred files
--progress              show progress during transfer

-W, --whole-file            copy files whole (without delta-xfer algorithm)
```

### 查看电池电量

以我的平板举个例子

```bash
$ cat /sys/class/power_supply/axp288_fuel_gauge/capacity
```

### 生成随机密码

生成20个长度为12的 含大写字母 `-c` , 数字 `-n` , 符号 `-y` , 完全随机 `-s` 的密码

```bash
pwgen -cnys 12 20
```

### Recursively chmod all directories except files

To recursively give directories read&execute privileges:

    find /path/to/base/dir -type d -exec chmod 755 {} +

To recursively give files read privileges:

    find /path/to/base/dir -type f -exec chmod 644 {} +

Reference from [StackExchange](https://superuser.com/questions/91935/how-to-recursively-chmod-all-directories-except-files)

### SWAP file

```
# truncate -s 0 /swapfile
# chattr +C /swapfile
# btrfs property set /swapfile compression none
```

## shell相关知识

### ssh访问遇到 Access denied

请检查该用户的 shell 的路径是否在 `/etc/shells` 列表中

### if判断条件

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

# 逻辑判断
-a 与
-o 或
! 非
```
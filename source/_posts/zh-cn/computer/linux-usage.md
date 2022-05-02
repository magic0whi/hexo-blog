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

## Linux Common Commands

### sed

The usage of group in regular expression: (`(regex)` and `\1`)
```console
$ sed -i 's,\$\([^$]*\)\$,\\\\(\1\\\\),g' file.md
```

### grep

Find in files:
```console
$ grep -iRl "your-text-to-find" ./
```

### rsync

1. Copy and preserve all attributes
   ```console
   $ rsync -aXHAv -P $SOURCE_DIR/ $TARGET_DIR/`
   ```
2. Copy only, don't keep permission and owner (keep only touch times)
   ```console
   $ rsync -rlt -P --no-owner --no-group --no-perms $SOURCE_DIR/ $TARGET_DIR/
   ```
3. Synchronize folders, be aware that `--delete` will delete any additional folder / files from `$TARGET_DIR` which is not in `$SOURCE_DIR`
   ```console
   $ rsync -aXHAz -v -P --exclude={"filename1","path/to/filename2"} --delete $SOURCE_DIR/ $TARGET_DIR/
   ```

### ss

`ss` is member of iproute tools set

List sockets statitics: `ss -s`
To see which process was using specific port: `ss -nlptu | grep $PORT_NUMBER`

### dd

To backup your GPT:
```console
# dd if=/dev/sda of=gpt-partition.bin bs=512 count=34
```

To restore your GPT
```console
# dd if=gpt-partition.bin of=/dev/sda bs=512 count=34
```

### pwgen

Random password generator
To generate 20 different passwords which has length 12 and at least one big letter (`-c`), number (`-n`), symbol (`-y`). Furthmore, `-s` can generate more randomize passwords
```console
$ pwgen -cnys 12 20
```

### Recursively chmod all directories and exclude files

To recursively give directories read & execute privileges:
```console
# find /path/to/dir -type d -exec chmod 755 {} +
```

To recursively give files read privileges:
```console
# find /path/to/dir -type f -exec chmod 644 {} +
```

Reference from [StackExchange](https://superuser.com/questions/91935/how-to-recursively-chmod-all-directories-except-files)

### Exclusive delete

By using glob extension in bash:
```console
# shopt -s extglob
# rm -r !(filename1|filename2|dir1)
```

### ssh-keygen

1. Show the fingerprint of a keyfile:
   ```console
   $ ssh-keygen -l -f </path/to/key>
   $ ssh-keygen -l -E md5 -f </path/to/key>
   ```
2. Modify the comment of a keyfile:
   ```console
   $ ssh-keygen -c -C <Your comment> -f </path/to/key>
   ```
3. Show the information of a keyfile:
   ```console
   $ ssh-keygen -y -f <Your key>
   ```

### Kernel interface

1. UUID Generator (Or `uuidgen`):
   ```console
   $ cat /proc/sys/kernel/random/uuid
   ```
2. Show available entropy:
   ```console
   cat /proc/sys/kernel/random/entropy_avail
   ```
3. Show battery capacity remain:
   ```console
   $ cat /sys/class/power_supply/<Your battery name>/capacity
   ```
4. List network interfaces: ` ls /sys/class/net` or `ip link`

## Iptables

1. IPSET u32 匹配
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

### IF Statement in Bash

- Integer Operator
  ```plaintext
  -eq    equal
  -ne    not equal
  -gt    greater
  -ge    geeater or equal
  -lt    lesser
  -le    lesser or equal
  ```
- File Operator
  ```
  -e            File or directory exist
  -r            Readable
  -w            Writable
  -x            Executable
  -f            Whether a normal file
  -d "dir"      Whether a directory exists
  ! -d "dir"    Whether a directioy not exists
  -c            Whether a char file
  -b            Whether a block file
  -s            Ture if file size is not zero
  -t            Whether a tty file
  ```
- String Operator
  ```
  ==POSIX sh
  str1 = str2
  str1 > str2       compare by alphabetical order
  str1 < str2
  -z str            True if the string length is zero.
  -n str            True if the string length is non-zero.
  ==Bash specific (Can use pattern matching '[[' ']]')
  str1 == str2
  str1 != str2
  str1 =~ regex     extended regular expression
  ```
- Logical Operator
  ```plaintext
  -a    And
  -o    Or
  !     Not
  [] && []     And (For pattern matching use '[[' ']]')
  [] || []     Or
  ```

### Bash Extended Globbing

| Glob         | Regular Expression Equivalent |
| --           | --                            |
| `*`          | `.*`                          |
| `?`          | `.`                           |
| `[a-z]`      | SAME                          |
| `?(pattern)` | `(regex)?`                    |
| `*(pattern)` | `(regex)*`                    |
| `+(pattern)` | `(regex)+`                    |
| `@(pattern)` | `(regex){1}`                  |
| `!(pattern)` | `^((?!regex).)*$`             |

### Variable Parameter Expansions

|                 |                                                                                                 |
| --              | --                                                                                              |
| `::n`           | Cut `n` chars from left to right if `n` is positive, otherwise right to left if negative        |
| `:n`            | Cut to end start from column `n`, if `n` is negative then right to left (use `:(-n)` or `: -n`) |
| `:x:y`          | Cut `y` chars start from column `x`                                                             |
| `${food:-Cake}` | Defaults to `Cake` if `$food` does not exist                                                    |

| `STR="/path/to/foo.cpp"`                 |                                                      |
| --                                       | --                                                   |
| `echo ${STR%/*}   # /path/to`            | Cut from right to left, single `%` means non-greedy. |
| `echo ${STR#*/}   # path/to/foo.cpp`     | Cut from left to right, single `#` means non-greedy. |
| `echo ${STR%.cpp} # /path/to/foo`        | Two `%` is greedy.                                   |
| `echo ${STR##*.}  # cpp`                 | Two `#` is greedy.                                   |
| `echo ${STR/foo/bar} # /path/to/bar.cpp` | String substitition, single `/` means non-greedy     |
| `echo ${STR//o/b} # /path/tb/fbb.cpp`    | Two `/` is greedy

### Bash Built-in variables

`$#` number of arguments

## SSH Tunnel

X11vnc startup

With SDDM and SSH Tunnel.
Please be aware that this command need to be executed on client-side.

```console
ssh -t -L 5900:localhost:5900 <REMOTE HOST> 'sudo x11vnc -localhost -display :0 -auth $(find /var/run/sddm/ -type f)'
```

## Trap

Reset signal `TERM`'s action to the default: `trap - TERM`

| Signal Number | Signal Name | Default Action                                                |
| --            | --          |
| 0 | EXIT | Nothing |
| 2             | INT      | Terminate (Interrupt, weakest, Ctrl+C)                        |
| 15            | TERM     | Terminate (Exit cleanly, normal)                              |
| 1             | HUP     | Terminate (Hangup, normal, sent from SSH disconnect) |
| 3             | QUIT     | Terminate (Harshest but still handle ignorable, core dump)    |
| 9             | KILL     | Terminate (Unconditionally)                                   |


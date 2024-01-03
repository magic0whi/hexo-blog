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

### Shell builtin & Concept

- Shortcut:
  `<M-b/f>` Move back / forward a word.
  `<C-b/f>` Move back / forward a char.
  `<C-s>` Pause STDOUT
  `<C-q>` resume STDOUT
  `<C-r>` Enter history search mode
  `<C-g>` Beaak out a newline; Leave history search mode
- filter: Programs that can be used in shell pipe.
  What programs can be used in shell pipe? Programs that process `stdin` line-by-line, then output results to `stdout`.
  E.G. `cp`, `diff` are NOT belong to filter
- Linux Shell Features
  - If no argument, default read from `stdin`.
  - Default output into stdout

- `type xxx` Show command type (builtin or outer)
- `man builtins` Show manual of shell builtins.
- `help xxx` Fast help for shell builtin `xxx`.
- `history` Show shell history, which can be used along with `fc`.

Redirect:
- Write (Override) Output redirect `>`
- Append output redirect `>>`
- Input redirect (Strings) `<<<`

- How to process shell args and `stdin`, `stdout`, `stderr`:
  ```C++
  #include<stdio.h>
  int main(int argc, char *argv[]) {
	  int i = 0;
	  char c;
     for (; i < argc; ++i) {
		  fprintf(stdout, "%d: %s\n", i, argv[i]);
	  }
	  fflush(stdout);
	  c = fgetc(stdin);
	  while (c != EOF) {
		  if (c >= 'a' && c <= 'z') {
		  c = c + 'A' - 'a';
	  }
		  fputc(c, stdout);
		  c = fgetc(stdin);
	  }
	  fflush(stdout);
	  fprintf(stderr, "I am stderr\n");
	  fflush(stderr);
  }
  ```

- 取值符 `$`
  `$$`(等价于 `${$}`) 当前 Shell 的 PID
  `$0` 上一个命令返回的状态编号 (≥0)
- Shell中的变量
  - 默认是字符串
  - `declare -i` 可定于整数类型变量
    ```Console
    $ x=5
    $ y=3
    $ declare -i l=$x+$y
    $ l=l+3 // 此时变量l是整数类型
    // 也可用 k=$(($x+$y)), 不过 k 依然是字符串类型
    ```
  - 如果做整数数值运算, 会把非整数都理解为 0.
  - `declare +i` 把变量整数属性去掉
- Shell For loop
  ```bash for.sh
  sum=$1
  for ((i=0;i<$num; i=i+1))
  do
      nohup ./run-folder.sh $num $i &
  done
  ```
- Shell Function
  ```bash sum.sh
  function sum() {
     echo $# # 收到的参数数量
     echo $* # 参数的字符串
     echo $0 # 第0个参数, 执行文件的名字
     echo $1 # 第一个参数
     echo $@ # 参数数组
  }

  sum 1 2 7 'a' # 函数调用
  # 结果是
  # 4
  # 1 2 7 a
  # ./sum.sh
  # 1
  # 1 2 7 a
  ```

- awk
  awk 过滤第三列成绩大于等于 90 的同学: `awk '$3 >= 90 {print}' scores.txt4 | less`
  `$3 >= 90` 为条件式 (pattern) `{print}` 为动作 (action)
  输出单独第二列列可用 `{print $2}` (`$0`则表示所有列, 即整行)
  条件式为可选, 如输出第二列和第五列 `awk '{print $2, $5}' scores.txt | less`
  特殊变量`NR`表示当前行号, 如过滤掉第一行 `awk 'NR > 1 {print $2, $5}' scores.txt | less`
  特殊模式 `BEGIN` 表示文件开始, `END` 表示文件结尾, 如在文件开始添加一行表头, 在结尾加一句 "Hi, bye": `awk 'BEGIN {print "No. ID. Score"}; {print}; END {print "HI, BYE"}' scores.txt4 | less`
  awk 还能定义变量计算平均成绩: `awk 'BEGIN {sum = 0}; {sum = sum + $3}; END {aver = sum / NR; print NR, sum, aver}' scores.txt4 | less`
  或者计算字符数 `awk '{cn = cn + length($0) + 1}; END {print NR, cn}' scores.txt4` (+1 是因为每行的末尾还有一个换行符)
  或者计算字段(用到特殊变量`NF`, 表示当前行字段数) `awk '{cn = cn + length($0) + 1; wn = wn + NF}; END {print NR, wn, cn}' scores.txt4`
  (可通过 `wc scores.txt4` 来验证行数、字段数和字符数)

- `egrep -o` 只显示匹配上的文字, 如只显示匹配2000年人民日报的序号 `cat */*.txt | egrep '^[0-9-]+' -o | sort | less`
  `-n` 可显示行号, `-v` 匹配取反, `-i` 大小写不敏感, `-r` 递归查询文件夹目录
- `sort`默认是使用ASCII码排序, 对于数字排序, 需要加上参数 `-n`
   对 scores.txt4 的第三列(成绩)进行排序 `sort -n -k 3 scores.txt4 | less`
   反向排序时可加参数 `-r`.
   `-u` 可删除排序内容相同的行, 与 `sort | uniq` 的区别是 `uniq` 命令是删除完全一样的行, 而前者会删除匹配列内容相同的行.
- `tr` 可转换文字大小写, 如 `tr 'a-z' 'A-Z'`
- `xargs` 可将管道前一个命令的输出作为输入参数
   如计算一堆文件内数字及'-'的数量 `find . -name '*[0-9][0-9][0-9]*.txt' | xargs egrep -o '^[0-9-]+' | wc -l`

- `diff` 共有三个命令: d(delete)、c(change)、a(add)
   如 `130d129` 为删除 130 行对齐到 129 行, `249a130,131` 为将后者的 130, 131 行添加到前者的 249 行后, `271,373c163,271` 为拿后者的 163, 271 行去替换前者的 271, 373行

- `nohup` 防止退出账户导致程序挂起
  让程序后台运行 `nohup bash run0.sh &`

### Filesystem

1. 一个文件对应一个 inode (存储文件属性+block的指针列表)和0~n个block(存储文件真实内容)
2. 硬链接: 同一个文件, 有多个名字; 只针对文件, 不支持目录; 不能跨文件系统.
   软连接: 不同文件; 文件/目录都可以; 可用跨文件系统.
3. 为什么文件夹不能创建硬链接, 硬连接数却大于1? 因为目录下的"."和子目录的".."能够增加目录硬链接数.

### Misc

- `findmnt` list all mounted filesystems.
- `# systool -v -m module_name` list options that are set for a loaded module.

### Clean Build

- Prepare
  ```console
  $ RAMDISK_SIZE=20
  $ sudo mount --mkdir -t tmpfs -o defaults,size=${RAMDISK_SIZE}G tmpfs /mnt/chroots/tmp
  $ sudo dd if=/dev/zero of=/mnt/chroots/tmp/ramdisk status=progress bs=1M count=$(($RAMDISK_SIZE*1024))
  $ sudo mkfs.btrfs -n 8k -m single -f /mnt/chroots/tmp/ramdisk
  $ sudo mount --mkdir -t btrfs -o loop,sync,compress=zstd /mnt/chroots/tmp/ramdisk /mnt/chroots/arch
  $ CHROOT=/mnt/chroots/arch
  ```
- Use paru
  ```console
  $ paru -S --chroot=/mnt/chroots/arch <Packages>
  ```
- Manual Way
  ```console
  $ mkarchroot $CHROOT/root base-devel
  $ makechrootpkg -c -r $CHROOT [-I ../Build_Deps/Build_Deps.pkg.tar.zst]
  ```
- Umount
  ```console
  sudo umount $CHROOT /mnt/chroots/tmp
  ```

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
5. Show monitor modes from kernel DRM module
   ```console
   $ cat /sys/class/drm/card0/card0-eDP-1/modes
   ```

## Iptables

1. IPSET u32 match
   Check whether the last value of TCP Seq in a network package equals 41 : `0>>22&0x3C@ 4 &0xFF=0x29`
   Example (Use WireShark to catch packages):
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

| Signal Number | Signal Name | Default Action                                             |
| --            | --          | --                                                         |
| 0             | EXIT        | Nothing                                                    |
| 2             | INT         | Terminate (Interrupt, weakest, Ctrl+C)                     |
| 15            | TERM        | Terminate (Exit cleanly, normal)                           |
| 1             | HUP         | Terminate (Hangup, normal, sent from SSH disconnect)       |
| 3             | QUIT        | Terminate (Harshest but still handle ignorable, core dump) |
| 9             | KILL        | Terminate (Unconditionally)                                |

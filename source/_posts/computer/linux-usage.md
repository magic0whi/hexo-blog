---
title: linux-usage
category: computer
date: 2020-02-08 16:42:06
tags:
toc: true
---

A manual of my Linux gists

<!-- more -->

## Linux Common Commands

### Reset User faillocks

```console
$ faillock --user <username> --reset
```

### Binary File Editing

Generate hexdump:
```console
$ xxd TIAS2781RCA4.bin > TIAS2781RCA4.bin.txt
```

Revert plaintext hexdump back into binary:
```console
$ cat TIAS2781RCA4.bin.txt | xxd -r > TIAS2781RCA4_mod.bin
```

### PDF Editing

PDF Extracting:
```console
$ pdfimages --all in.pdf out_dir/
```

PDF Regenerate:
```console
$ img2pdf --output out.pdf \
--creator 'Canon SC1011' \
--producer 'IJ Scan Utility' \
--creationdate 'Wed Mar 20 16:33:38 2024 CST' \
-D \
--engine internal \
-s 600dpi \
[1-5].jpg
$ pdfinfo out.pdf
```

### Steam

Launch Heart of Iron IV (With FSR1, VRR, MangoHud)
```console
$ gamescope -w 1728 -h 1080 -W 2944 -H 1840 --adaptive-sync -F fsr --fsr-sharpness 10 -- env LD_PRELOAD='/usr/lib/mangohud/libMangoApp.so /usr/lib/mangohud/libMangoHud.so /usr/lib/mangohud/libMangoHud_dlsym.so /usr/lib/mangohud/libMangoHud_opengl.so' sh ./cream.sh %command%
```

### Libvirt

> Use `--print-xml | less` to see generated xml only for debugging.

- Install a Arm Cortex-a53 machine
  ```console
  virt-install --connect qemu:///system \
   --memory 2048 --memorybacking hugepages.page0.size=2048 \
   --arch=aarch64 --cpu cortex-a53 --machine virt \
   --vcpus vcpu=4,vcpu.placement=static --iothreads 1 \
   --cputune vcpupin0.vcpu=0,vcpupin0.cpuset=2,vcpupin1.vcpu=1,vcpupin1.cpuset=10,vcpupin2.vcpu=2,vcpupin2.cpuset=3,vcpupin3.vcpu=3,vcpupin3.cpuset=11,emulatorpin.cpuset='1,9',iothreadpin0.iothread=1,iothreadpin0.cpuset='1,9' \
   --cpu topology.sockets=1,topology.dies=1,topology.cores=4,topology.threads=1,numa.cell0.memory=2048,numa.cell0.unit=MiB,numa.cell0.memAccess=shared \
   --osinfo archlinux \
   --disk size=10,format=qcow2,driver.cache=none,driver.io=native,driver.discard=unmap,driver.iothread=1,driver.queues=4,driver.iommu=on,target.bus=virtio \
   --boot firmware=efi,loader=/usr/share/edk2/aarch64/QEMU_CODE.fd,loader.readonly=yes,loader.type=pflash,nvram.template=/usr/share/edk2/aarch64/QEMU_VARS.fd,boot0.dev=network,boot1.dev=hd \
   --features gic.version=3,kvm.hidden.state=off,pmu.state=on \
   --clock offset=localtime,timer0.name=rtc,timer0.tickpolicy=catchup,timer0.track=guest,timer1.name=pit,timer1.tickpolicy=delay \
   --network direct,trustGuestRxFilters=yes,source=macvtap0,source.mode=vepa,model=virtio,driver.iommu=on \
   --controller virtio-serial,driver.iommu=on \
   --video virtio,model.vram=16384,model.heads=1 \
   --watchdog i6300esb \
   --rng /dev/random,model=virtio,driver.iommu=on \
   --tpm emulator,backend.version=2.0 \
   --memballoon virtio,driver.iommu=on \
   --iommu virtio \
   --panic pvpanic \
   --import
  ```
- Install a RISC-V machine
  ```console
  virt-install --connect qemu:///system \
   --memory 2048 --memorybacking hugepages.page0.size=2048 \
   --arch riscv64 --machine virt \
   --vcpus vcpu=4,vcpu.placement=static --iothreads 1 \
   --cputune vcpupin0.vcpu=0,vcpupin0.cpuset=2,vcpupin1.vcpu=1,vcpupin1.cpuset=10,vcpupin2.vcpu=2,vcpupin2.cpuset=3,vcpupin3.vcpu=3,vcpupin3.cpuset=11,emulatorpin.cpuset='1,9',iothreadpin0.iothread=1,iothreadpin0.cpuset='1,9' \
   --cpu topology.sockets=1,topology.dies=1,topology.cores=4,topology.threads=1,numa.cell0.memory=2048,numa.cell0.unit=MiB,numa.cell0.memAccess=shared \
   --osinfo archlinux \
   --disk size=10,format=qcow2,driver.cache=none,driver.io=native,driver.discard=unmap,driver.iothread=1,driver.queues=4,driver.iommu=on,target.bus=virtio \
   --boot loader=/usr/share/qemu/opensbi-riscv64-generic-fw_dynamic.bin,boot0.dev=network,boot1.dev=hd \
   --features kvm.hidden.state=off \
   --clock offset=localtime,timer0.name=rtc,timer0.tickpolicy=catchup,timer0.track=guest,timer1.name=pit,timer1.tickpolicy=delay \
   --network direct,trustGuestRxFilters=yes,source=macvtap0,source.mode=vepa,model=virtio,driver.iommu=on \
   --controller virtio-serial,driver.iommu=on \
   --video virtio,model.vram=16384,model.heads=1 \
   --watchdog i6300esb \
   --rng /dev/random,model=virtio,driver.iommu=on \
   --tpm emulator,backend.version=2.0 \
   --memballoon virtio,driver.iommu=on \
   --import
  ```

### Shell builtin & Concept

- Shortcut:
  `<M-b/f>` Move back / forward a word.
  `<C-b/f>` Move back / forward a char.
  `<C-s>` Pause STDOUT.
  `<C-q>` resume STDOUT.
  `<C-r>` Enter history search mode.
  `<C-g>` Beaak out a newline; Leave history search mode.
  `<M-h>` Open manual for current typed command.
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
  ```c++
  #include<stdio.h>
  int main(int argc, char *argv[]) {
	  int i = 0;
	  char c;
     for (; i < argc; ++i)
		  fprintf(stdout, "%d: %s\n", i, argv[i]);
	  fflush(stdout);
	  c = fgetc(stdin);
	  while (c != EOF) {
		  if (c >= 'a' && c <= 'z')
		    c = c + 'A' - 'a';
		  fputc(c, stdout);
		  c = fgetc(stdin);
	  }
	  fflush(stdout);
	  fprintf(stderr, "I am stderr\n");
	  fflush(stderr);
  }
  ```

- `$$` (equivalent to `${$}`) is current shell's PID.
  Variables in shell default to string.
  Shell treat all string type as zero if do arithmetic to them.
  `declare -i` defines integer variable
  ```Console
  $ x=5
  $ y=3
  $ declare -i l=$x+$y
  $ l=l+3 // Integer variable can do arithmetic directly
  $ k=$(($x+$y)) // do arithmetic but the type of 'k' still remains string
  ```
  `declare +i` removes integer attribute of the variable.
- Shell For loop
  ```bash for.sh
  for ((i=0;i<10; i=i+1)); do
    echo $i
  done

  for i in {1..10}; do
    echo $i
  done

  # Step = 2
  for i in {1..10..2}; do
    echo $i
  done
  ```
- Shell Function
  ```bash sum.sh
  function sum() {
     echo $# # The numeber of parameters
     echo $* # Parameters in string type
     echo $0 # The 0st parameter, which is the name of executed file itself
     echo $1 # The first parameter
     echo $@ # Parameters in array type
  }

  sum 1 2 7 'a'
  # Result:
  # 4
  # 1 2 7 a
  # ./sum.sh
  # 1
  # 1 2 7 a
  ```

- awk
  To filter only the column 3 with value >= 90: `awk '$3 >= 90 { print }'`,
  where `$3 >= 90` is pattern (Optional), `print` is the action.
  `$0` means whole line.
  `NR` means current line number. e.g. To ignore the first line `awk 'NR > 1 { print $2, $5 }'`
  `BEGIN` means the start of file. `END` means end of file e.g. `awk 'BEGIN { print "SOF" } { print } END { print "EOF" }'`
  awk support variables, To calculate sum `awk 'BEGIN { sum = 0 } { sum = sum + $3 } END { avg = sum / NR; print NR, sum, avg }'`
  e.g. To do char count `awk '{ cc = cc + length($0) + 1 } END { print NR, wc }'` (Validate by `wc -ml`. The extra +1 because there is `\n` at each end of line)
  `NF` means mean word count of current line. e.g. Do word counts `awk '{ cc = cc + length($0) + 1; wc = wc + NF } END {print NR, cc, wc }'` (Validate by `wc`)
  `-F` set the delimiter `awk -F':'` 
- `egrep -o` show only contents that matched, e.g. To show the order number only:  `cat */*.txt | egrep '^[0-9-]+' -o | sort | less`
  `-n` show line nunber, `-v` reverse mathch, `-i` case insensitive, `-r` search directories recursively.
- `sort` default to ASCII code sort, for nunberic sort, use parameter `-n`.
   e.g. Sort the third column of `scores.txt`: `sort -n -k 3 scores.txt | less`
   `-r` do reverse sort.
   `-u` remove duplicates. The difference to `sort | uniq` is `uniq` removes exactly same content line, while `sort -u` remove lines which has the same match.
- `tr` do uppercase / lowercase. e.g. `tr 'a-z' 'A-Z'` (equivalent to `tr '[:lower:]' '[:upper:]'`)
- `xargs` format stdin as parameters to the command.
   e.g. To calculate numbers and the symbol '-' in a pile of files `find . -name '*[0-9][0-9][0-9]*.txt' | xargs egrep -o '^[0-9-]+' | wc -l`.

- `diff`'s format explanation:
   Three actions: d(elete), c(hange), a(dd)
   e.g. `130d129` delete line 130 and then align to line 129; `249a130,131` add later's line 130, 131 to former's line 249; `271,373c163,271` replace later's line 163, 271 to former's line 271, 373.
- `nohup` prevents program to hang on session terminate. e.g. `nohup bash run0.sh &`
- `set -eufo pipefail`
  `-e` let bash exit immediately if any command has non-zero exit status.
  `-u` let bash exit immediately if has any reference to variable haven't defined yet.
  `-f` disables pathname expansion (globbing).
  `-o pipefail` prevents erros in pipeline from begin masked. Any command fails in pipeline will keep its return code for whole pipeline.

### DSDT (Differentiated System Description Table)

Here is an example to fix s2idle issue on the Lenovo Yoga Air 14s APU8 (aka. Yoga Slim 7 Gen 8 14APU8)

Extract & Modify DSDT
```console
$ # Extract the binary ACPI tables
# cat /sys/firmware/acpi/tables/DSDT > dsdt.dat
$ # Disassemble the ACPI tables to a .dsl file
$ iasl -d dsdt.dat
$ # Modify DSDT
$ vim dsdt.dsl
$ # Attempt to create a hex AML table (in C) dsdt.aml
$ iasl -tc dsdt.dsl | grep Errors
```

Extract & Modify SSDT
```console
$ # Modify SSDT (System Service Descriptor Table)
# for i in /sys/firmware/acpi/tables/SSDT*; do \
    cat $i > ${i##*/}; \
done
$ iasl -d SSDT*
$ rg "SB.PCI0.GPP8.NVME" SSDT*.dsl
$ # Found keywords in SSDT14.dsl, comment out it
$ vim SSDT14.dsl
$ # Compile SSDT to generate SSDT14.aml
$ iasl -tc SSDT14.dsl
$ # Create a CPIO archive
$ mkdir -p kernel/firmware/acpi
$ # through dsdt.aml is optional if it keep untouched
$ cp dsdt.aml SSDT14.aml kernel/firmware/acpi
$ find kernel | cpio -o -H newc > SSDT14
```

> To extract CPIO archive
> ```console
> $ cpio -iv -H newc < SSDT14
> $ iasl -d kernel/firmware/acpi/SSDT14.aml
> ```

### Inkscape

1. <u>N</u>ode Tool
  - Dragging over line with `<M>` to select their nodes, release to switch to rubberband mode.
  - `!` inverts node selection in current subpath(s).
  - `[`,`]` rotate 15&deg; `<`,`>` scale.
  - `2x<LMB>` or `<C-M-LMB>`: Add node.
  - `2x<LMB>` or `<C-M-LMB>` or ``<Del>``: Delete node. Use `<C-Del>` if you don't wanna Inkscape to preserve the shape.
  - `<S>-d` duplicate selected nodes, `<S>-b` breaks selected nodes, `<S>-j` joins two selected endnodes.
  - `<S>-c` mades node cusp, which means its two handles can move independently at any angle, to each other.
    `<S>-s` mades node smooth, which means its handles are always colinear.
    `<S>-y` mades node symmetric, which is same as smooth, but the handles also have the same length.
    `<S>-a` mades node auto-smooth, which is a special node that automatically adjusts the handles of the node and surrounding auto-smooth nodes to maintain a smooth curve. 
    > When switching the type of node, preseve one position of the two handles by hovering cursor over it. So that only the other handle is rotated / scaled to match.
  - `<C-LMB>`: Retract handle.
    `<S-LMB>`: Pull out handle.
    `<C>-k`: Combine into compound path.
    `<C-S>-k`: Break apart compound path.
  > - Compound path is not the same as a group. It's a single object which is only selectable as a whole.
  > - Parts of a path (i.e. a selection of nodes) can also be copied with `<C>-c` and inserted as a new path with `<C>-v`.
  > - `<C-S>-c`: Convert shape or text to path.
  - Boolean Operators
    Union `<C>-+`; Difference `<C>--`(Bottom minus top); Intersection `<C>-*`;
    Exclusion `<C>-^`; Division `<C>-/`; Cut Path `<C-M>-/`.
    > <u>Exclusion</u> command looks similar to <u>Combine</u>, but it is different in that <u>Exclusion</u> adds extra nodes where the original paths intersect.
    > The difference between <u>Division</u> and <u>Cut Path</u> is that the former cuts the entire buttom object by the path of the top object, while the latter only cuts the bottom object's stroke and removes any fill.
  - Common use cases
    <u>Split Path</u>: `<C-S-M>-k` splits non-connected sections of a path.
    <u>Fracture</u>: `<S-M>-f` fractures connected paths piece by piece.
    <u>Flatten</u>: `<S>-f`, new difference that remove all overlapped areas in the selected paths.
  - Shape Build Tool (<u>X</u>)
    Before switch to the tool, select at least one overlapping object.
    `<LMB>` to add a section to the result;
    `<S-LMB>` to removee it (it can be used to create a hole in it's place).
    `<LMB>-Drag` to connect multiple sections to one.
    `<S-LMB>-Drag` to remove a contiguous section.
  - Inset and Outset
    Inkscape can expand and contract shapes not only by scaling, but also by <u>offsetting</u> an object's path, i.e. by displacing perpendicular to the path in each point.
    `<C>-(` Inset; `<C>-)` Outset.
    `<C>-j` Dynamic Offset (create an object with a draggable handle controlling the offset distance).
    `<C-M>-i` Linked Offset, create a dynamic offset linked to the original path.
  - Simplitication
    `<C>-l`: Simplify. This may be useful for paths created by the Pencil tool. The amount of simplication depends on the size of the selesction. Moreover, the simply command is accelerated if press `<C>-l` several times in quick succession.
2. Pen Tool (B)
  - `<LMB>` creates a sharp node.
  - `<LMB>-Drag` creates a smooth Bezier node.
  - `<S>` while dragging out a handle.
  - `<C>` limits the direction of either the current line segment or the Bezier handles to 15 &deg; increments.
  - `<CR>` finalizes the line; `<Esc>` cancels it.
3. <u>T</u>ext Tool
  `<C-S>-t`: Open the Text and Font dialog.
  `<M>-<`,`>`: Decrease, Increase the <u>letter spacing</u> of a text object. `<C-M>-<`,`>` adjust <u>line spacing</u> in multi-line text objects.
  `<M-Left,Right,Up,Down>`: Kerning the letters right of the cursor horizontally, vertically.
  `<LMB>-Drag`: Click and drag with the text tool.
4. XML Editor
  `<C-S>-x`: Allows to do tricks.

### Filesystem

1. A file corresponds to one inode (which stores the file properties and the pointer table of blocks) and ordered number 0~n blocks (stores the data of the file).
2. Hard symbol links: Same file but has multiple names; Only apply to file, does not support directory; Can't across filesystems.
   Soft symbol links: Different file but same name; Could apply to file or directory; Can across filesystems.
3. Why directories could not have hard link, but it has hard link count more than 1?
   Because directories have pointer "." and "..", which increase the hard link count.

### Misc

- `findmnt` list all mounted filesystems.
- `# systool -v -m module_name` list options that are set for a loaded module.

### Clean Build

- Prepare
  ```console
  $ RAMDISK_SIZE=20
  $ sudo mount --mkdir -t tmpfs -o defaults,size=${RAMDISK_SIZE}G tmpfs /mnt/chroots/tmp
  $ sudo dd if=/dev/zero of=/mnt/chroots/tmp/ramdisk status=progress bs=128K count=$(($RAMDISK_SIZE*8192))
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

### Sed

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

### `chmod` directories only (exclude files)

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

Enable glob extension

- Bash
  ```console
  $ shopt -s extglob
  $ rm -r !(file1|file2|dir)
  ```
- Zsh
  ```console
   $ setopt extendedglob
   $ rm -r ^(file1|file2|dir)
   ```

### Bluetooth dual boot pairing

Extracting on Linux

```console
# cryptsetup open --type=bitlk /dev/nvme0n1p3 win11 <<<XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX
# mount /dev/mapper/win11 /mnt/win11
$ cd /mnt/Windows/System32/config
$ chntpw -e SYSTEM
> cd ControlSet001\Services\BTHPORT\Parameters\Keys
> cd xxxxxxxxxxxx
> hex xxxxxxxxxxxx # For not a Bluetooth 5.1 devices
> cd xxxxxxxxxxxx # For Bluetooth 5.1 devices
> hex LTK
> hex ERand
> hex EDIV
> hex IRK
```

Useful python snippets
```console
$ python
>>> LTK='<hex-of-LTK>'.replace(' ', '')
>>> ERand=int(''.join(list(reversed('<hex-of-ERand>'.strip().split()))), 16)
>>> EDIV=int(''.join(list(reversed('<hex-of-EDIV>'.strip().split()))), 16)
>>> IRK=list(reversed('<hex-of-IRK>'.strip().split()))
>>> print('LTK: ', LTK, '\n', 'ERand: ', ERand, '\n', 'EDIV: ', EDIV, '\n', 'IRK: ', ''.join(IRK))
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
2. Show battery capacity remain:
   ```console
   $ cat /sys/class/power_supply/<Your battery name>/capacity
   ```
3. List network interfaces: ` ls /sys/class/net` or `ip link`
4. Show monitor modes from kernel DRM module
   ```console
   $ cat /sys/class/drm/card1/card1-eDP-1/modes
   ```
5. (D-Bus) Manually inhibit / pause [clighti<sup>AUR<sup>](https://aur.archlinux.org/packages/clight).
   Check all
   ```console
   $ busctl --user introspect org.clight.clight /org/clight/clight org.clight.clight
   ```
   Call method Inhibit
   ```console
   $ busctl --user call org.clight.clight /org/clight/clight org.clight/clight Inhibit b true
   ```
   Check property Inhibited
   ```console
   $ busctl --user get-property org.clight.clight /org/clight/clight org.clight.clight Inhibited
   ```
   Or call method Pause
   ```console
   $ busctl --user call org.clight.clight /org/clight/clight org.clight.clight Pause b true
   ```
   Check property Suspended
   ```console
   $ busctl --user get-property org.clight.clight /org/clight/clight org.clight.clight Suspended
   ```
6. Show current modules paraeter
  ```console
  # cat /sys/module/nvidia_drm/parameters/modeset
  ```

## Iptables

1. IPSET u32 match
  Check whether the last value of TCP Seq in a network package equals 41 : `0>>22&0x3C@ 4 &0xFF=0x29`
  Example (Use WireShark to catch packages), have this IP header:
  ```
  Source IP: 121.41.89.52
  = 01111001 00101001 01011001 00110100B = 79 29 59 34H = 2032752948D
  IP Header：
  45 00 00 3c 00 00 40 00 31 06 ef 34 **79 29 59 34** c0 a8 c7 81
  TCP Header：
  00 50 95 3c 8d 7f 52 ac 69 15 33 be a0 12 71 20 cd dc 00 00 02 04 05 14 04 02 08 0a 08 c8 62 fa 00 1c 30 a1 01 03 03 07
  ```
  `0`: Start from offset 0, get 4 bytes (since u32 means 32 bits). Here is `45 00 00 3c`.
  `>>22` shift right 22 bits. So `45 00 00 3c = 0100 0101 0000 0000 0000 0000 0011 1100` becomes `1 14 = 01 0001 0100`.
  `&0x3C` do bitwise AND with `0x3C` (like a filter). Then
  ```
    01 0001 0100
  & 00 0011 1100
  --------------
    00 0001 0100
  ````
  Conclusion, `0>>22&0x3C` get the 4-7 bits from the IP header.
  So the length of this IP header is `01 0100 = 20D`.
   `@` offset the pointer with the value on the left. So `0>>22&0x3C@` gets 20 bytes forward from the start address.
  We want to know the TTL value of this tcp packet. So get value from TCP header at index of 4 and filter it using mask `0xFF` to get last 1 byte (8 bits).
   Finally compares to`0x29 = 41D`
   > It can also compares to a range. e.g. Whether between 41-60: `0>>22&0x3C@ 4 &0xFF=0x29:0x3C`

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

X11vnc startup with SDDM and SSH Tunnel.
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

## GPU Environment Variables

```console
# /usr/share/glvnd/egl_vendor.d/*
__EGL_VENDOR_LIBRARY_FILENAMES='/usr/share/glvnd/egl_vendor.d/10_nvidia.json'
__GLX_VENDOR_LIBRARY_NAME=nvidia
# /usr/share/vulkan/icd.d/*
VK_ICD_FILENAMES='/usr/share/vulkan/icd.d/nvidia_icd.json'

# lspci -D | grep VGA
DRI_PRIME=pci-0000_06_00_0 glxinfo | grep 'OpenGL renderer'

# /usr/lib/dri/*_dri.so
MESA_LOADER_DRIVER_OVERRIDE=iris

# /usr/lib/dri/*_drv_video.so
LIBVA_DRIVER_NAME=

LIBVA_DRIVER_NAME=radeonsi vainfo --display drm --device /
```

## Matlab

```ini matlab.desktop
Name=Matlab2022b
Comment=A high-level language for numerical computation and visualization
GenericName=Matlab
Exec=env _JAVA_AWT_WM_NONREPARENTING=1 LD_PRELOAD=/usr/lib/libstdc++.so LD_LIBRARY_PATH=/usr/lib/xorg/modules/drivers/ MESA_LOADER_DRIVER_OVERRIDE=iris /home/proteus/MATLAB/R2022b/bin/matlab -desktop
Categories=Education
Type=Application
```

```plaintext java.opts
-Djpgl.disable.openglarbcontext=1
```

## Certbot

Register a wildcard domain hosted on Cloudflare
```console
# certbot certonly --dns-cloudflare --dns-cloudflare-credentials ~/.secrets/cloudflare.ini --server https://acme-v02.api.letsencrypt.org/directory --email <EMAIL> --agree-tos --no-eff-email -d '*.ndoskrnl.net'
```

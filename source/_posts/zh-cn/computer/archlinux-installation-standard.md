---
title: archlinux-installation-standard
lang: zh-cn
category: computer
date: 2020-10-20 21:46:02
tags:
toc: true
---

The standard of archlinux installation & configuration for me

Enter F12 for Boot Menu when bootstrap

<!-- more -->

## Pre-installation

1. Update the system clock
   ```console
   # timedatectl set-ntp true
   ```
2. Preparing the partitions

   Partition layout:
   ```
   +-----------------------+------------------------+-----------------------+
   | Boot partition        | LUKS2 encrypted system | Swap partition        |
   |                       | partition              | plain-encrypted       |
   |                       |                        |                       |
   | /boot                 | /                      |                       |
   |                       |                        |                       |
   |                       | /dev/mapper/cryptroot  | [SWAP]                |
   |-----------------------|------------------------|-----------------------|
   | /dev/sda1             | /dev/sda2              | /dev/sda3             |
   +-----------------------+------------------------+-----------------------+
   ```

   Preparing non-boot partitions
   ```console
   # cryptsetup -y -v luksFormat /dev/sda2
   # cryptsetup open /dev/sda2 cryptroot
   # mkfs.btrfs -L arch_os /dev/mapper/cryptroot
   # mount /dev/mapper/cryptroot /mnt
   ```

   > You can set the filesystem label later by using `btrfs filesystem label /dev/mapper/cryptroot "arch_os"`

   Preparing the boot partition
   ```console
   # mkfs.fat -F32 /dev/sda1
   ```

## Btrfs subvolumes

1. Create top-level subvolumes
   ```console
   # btrfs subvolume create /mnt/@
   # btrfs subvolume create /mnt/@snapshots
   # btrfs subvolume create /mnt/@home
   ```
2. Mount top-level subvolumes
   Unmount the system partition at /mnt.
   ```console
   # umount /mnt
   ```
   Now mount the newly created subvolumes by using the `subvol=` mount option (with enabled compress `zstd`).
   ```
   # mount -o compress=zstd,subvol=@ /dev/mapper/cryptroot /mnt
   # mkdir /mnt/{boot,home,.snapshots}
   # mount /dev/sda1 /mnt/boot
   # mount -o compress=zstd,subvol=@home /dev/mapper/cryptroot /mnt/home
   # mount -o compress=zstd,subvol=@snapshots /dev/mapper/cryptroot /mnt/.snapshots
   ```
3. Create nested subvolumes
   Create any nested subvolumes you do **not** want to have snapshots of when taking a snapshot of `/`.
   Every nested subvolume will be an empty directory inside the snapshot.
   Since the `@` subvolume is mounted at `/mnt` you will need to create a subvolume at `/mnt/var/cache/pacman/pkg` as a nested subvolume
   You may have to create any parent directories first.
   ```console
   # mkdir -p /mnt/var/cache/pacman
   # btrfs subvolume create /mnt/var/cache/pacman/pkg
   # btrfs subvolume create /mnt/var/tmp
   ```
4. Configuring swap
   TODO

## Installation

1. Select the mirrors
   I will use huaweicloud as main mirror
   ```console
   # sed -i '1aServer = https://mirrors.huaweicloud.com/archlinux/$repo/os/$arch\n' /etc/pacman.d/mirrorlist
   ```
2. Install essential packages
   ```console
   # pacstrap /mnt base base-devel linux linux-firmware btrfs-progs vim rng-tools git tmux openssh bash-completion iwd systemd-swap bluez bluez-utils
   ```

## Configure the system

1. Fstab
   ```console
   # genfstab -U /mnt >> /mnt/etc/fstab
   ```
2. Chroot
   ```console
   # arch-chroot /mnt
   ```
3. Time zone
   ```console
   # ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
   # timedatectl set-ntp true
   # hwclock --systohc
   ```
4. Localization
   ```console
   # sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
   # sed -i 's/#zh_CN.UTF-8 UTF-8/zh_CN.UTF-8 UTF-8/g' /etc/locale.gen
   # locale-gen
   # echo "LANG=en_US.UTF-8" >> /etc/locale.conf
   # echo "KEYMAP=us" >> /etc/vconsole.conf
   ```
5. Network configuration
   Create the `/etc/hostname` file:
   ```console
   # echo "myhostname" > /etc/hostname
   ```
   Add matching entries to [hosts(5)](https://jlk.fjfi.cvut.cz/arch/manpages/man/hosts.5):
   ```conf /etc/hosts
   127.0.0.1	localhost
   ::1          localhost
   127.0.1.1	myhostname.neo	myhostname
   ```
   Wireless adapter configuration
   > Use `ip link` to show network interface names
   ```conf /etc/systemd/network/25-wireless.network
   [Match]
   Name=<Your wireless interface name>

   [Network]
   DHCP=yes
   ```
   Enable iwd and Systemd-networkd:
   ```console
   # systemctl enable iwd.service
   # systemctl enable systemd-networkd.service
   # systemctl enable systemd-resolved
   ```
6. Random number generation
   Enable Rng-tools
   ```console
   # systemctl enable rngd.service
   ```
7. Configuring mkinitcpio
   I prefer using the sd-encrypt hook with the systemd-base initramfs. (replace hook `udev` with hook `system` )
   ```conf /etc/mkinitcpio.conf
   HOOKS=(base **systemd** autodects **keyboard** **sd-vconsole** modconf block **sd-encrypt** filesystems fsck)
   ```
   Recreate the initramfs image
   ```console
   # mkinitcpio -P
   ```
8. Users and password
   Create and use an unprivileged(non-root) user account(s) for most tasks
   ```console
   # useradd -m -s /bin/bash <Username>
   ```
   sudo: add user to sudoers and disable password prompt timeout
   ```console
   # echo "<Username> ALL=(ALL) ALL" >> /etc/sudoers.d/<Username>
   # echo "Defaults passwd_timeout=0" > /etc/sudoers.d/notimeout
   ```
   Setting the new user and root user's password
   ```console
   # passwd <Username>
   # passwd root
   ```
9. AUR helper
   I will use [yay](https://aur.archlinux.org/packages/yay/) as my AUR helper
   1. Use makepkg wrapper `makepkg-shallow` to make makepkg do shallow clone
      ```shell /usr/bin/makepkg-shallow
      #!/bin/bash

      git() {
        if [[ $# -gt 1 && $1 == 'clone' && $2 != '-s' ]]; then
          /bin/git "$@" --depth=1 --no-single-branch
        elif [[ $# -gt 1 && $1 == 'fetch' ]]; then
          /bin/git fetch --depth=3 -p
        elif [[ $# -gt 1 && [$1 == 'describe' || $1 == 'rev-list'] ]]; then
          /bin/git fetch --unshallow -p
          /bin/git "$@"
        else
          /bin/git "$@"
        fi
      }

      source /bin/makepkg "$@"
      ```
   2. Build & Install yay
      ```console
      # chmod 755 /usr/bin/makepkg-shallow
      # mkdir /build
      # chown -R <Username>:<Username> /build
      # cd /build
      # sudo -u <Username> git clone --depth=1 https://aur.archlinux.org/yay.git
      # cd yay
      # sudo -u <Username> makepkg-shallow --noconfirm -si
      # sudo -u <Username> yay --save --cleanafter --removemake --makepkg /usr/bin/makepkg-shallow
      # pacman -Qtdq | xargs -r pacman --noconfirm -Rcns
      # rm -rf /home/<Username>/.cache
      # rm -rf /build
      ```
10. Boot loader
    Installing the EFI boot manager
    ```console
    # bootctl install
    ```
    Automatic update
    The package [systemd-boot-pacman-hook](https://aur.archlinux.org/packages/systemd-boot-pacman-hook/) provides a Pacman hook to automate the update process.

    Configuring the boot loader
    Add the following kernel parameter to boot loader configuration
    ```conf /boot/loader/loader.conf
    default  arch.conf
    timeout  4
    console-mode max
    editor   no
    ```

    > use `lsblk -f` to show persistent block device naming
    ```conf /boot/loader/entries/arch.conf
    title   Arch Linux
    linux   /vmlinuz-linux
    initrd  /initramfs-linux.img
    options rd.luks.name=<sda2-UUID>=cryptroot root=/dev/mapper/cryptroot rootflags=subvol=@
    ```
11. (Optional) Enable sshd
    ```console
    # systemctl enable sshd.service
    ```
12. (Optional) Enable ZRAM
    Configuration
    ```conf /etc/systemd/swap.conf
    zram_enabled=1
    zram_size=$(( RAM_SIZE / 2 ))
    zram_count=${NCPU}
    zram_streams=${NCPU}
    zram_alg=zstd
    ```
    Enable systemd-swap
    ```console
    # systemctl enable systemd-swap
    ```
13. (Optional) Enable bluetooth
    ```console
    # systemctl enable bluetooth.service
    ```

## Desktop Environment

I will use GNOME as my desktop environment

1. Installation
   ```console
   # yay -S gnome-shell gnome-shell-extensions gdm gdm3setup \
   nautilus file-roller sushi seahorse \
   noto-fonts{,-cjk,-emoji} \
   gnome-{control-center,terminal,tweaks,keyring,backgrounds,clocks,logs,screenshot,menus} \
   gnome-shell-extension-kimpanel-git gnome-shell-extension-dash-to-dock gnome-shell-extension-desktop-icons-ng \
   gtk-engine-murrine materia-gtk-theme \
   gdm3setup dconf-editor \
   fcitx5-im fcitx5-rime fcitx5-configtool fcitx5-pinyin-{zhwiki,moegirl} fcitx5-skin-adwaita-dark
   ```

## NVIDIA & NVIDIA Optimus

I will use the method of `PRIME render offload` which was official method supported by NVIDIA

1. The [nvidia-prime](https://www.archlinux.org/packages/?name=nvidia-prime) package provides a script that can be used to run programs on the NVIDIA card.
   ```console
   # pacman -S nvidia nvidia-prime
   ```
   To run a program on the NVIDIA card you can use the prime-run command:
   ```console
   $ prime-run glxinfo | grep "OpenGL renderer"
   $ prime-run vulkaninfo
   ```
2. Dynamic power management of the dGPU
   Enable runtime power management for each PCI function
   ```console
   echo auto > /sys/bus/pci/devices/<Domain>:<Bus>:<Device>.<Function>/power/control
   ```
   ```console
   # modprobe nvidia "NVreg_DynamicPowerManagement=0x02"
   ```
   
   The automated ways to perform the manual steps mentioned above so that this feature works seamlessly after boot:
   1. Create a file named `80-nvidia-pm.rules` in `/lib/udev/rules.d/` directory
      ```conf /lib/udev/rules.d/80-nvidia-pm.rules
      # Remove NVIDIA Audio devices, if present
      ACTION=="add", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x040300", ATTR{remove}="1"

      # Enable runtime PM for NVIDIA VGA/3D controller devices on driver bind
      ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="auto"

      # Disable runtime PM for NVIDIA VGA/3D controller devices on driver unbind
      ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="on"
      ```
      > You can use `udevadm info --attribute-walk --path=/sys/bus/pci/devices/<Domain>\:<Bus>\:<Slot>.<Function>` to get a PCI device's attribution
   2. Set the driver option via the kernel module configuration files
      ```conf /etc/modprobe.d/nvidia.conf
      options nvidia "NVreg_DynamicPowerManagement=0x02"
      ```
   3. Reboot the system

## Troubleshot

1. blacklist kernel module
   ```conf /etc/modprobe.d/blacklist.conf
   blacklist ideapad_laptop
   blacklist nouveau
   ```
2. yay: `go get` is slow in China
   ```console
   $ export GO111MODULE=on
   $ export GOPROXY=https://goproxy.io
   ```
3. Disable media automount in GNOME
   ```console
   $ gsettings set org.gnome.desktop.media-handling automount false
   $ gsettings set org.gnome.desktop.media-handling automount-open false 
   ```

## Additions

1. Archlinuxcn
   ```
   [archlinuxcn]
   Server = https://repo.archlinuxcn.org/$arch
   ## or install archlinuxcn-mirrorlist-git and use the mirrorlist
   #Include = /etc/pacman.d/archlinuxcn-mirrorlist
   ```
   Install archlinux-keyring
   ```console
   # pacman -S archlinuxcn-keyring
   ```
2. Additional Packages
   ```
   bpytop
   htop
   docker
   docker-compose
   zerotier-one
   man
   cmake
   gdb
   arch-install-scripts
   pacman-contrib
   tree
   nmap
   traceroute
   compsize
   wireguard-tools
   picocom
   gvfs-mtp

   zsh
   zsh-autosuggestions
   zsh-syntax-highlighting
   zsh-history-substring-search

   npm
   nodejs-hexo-cli

   debootstrap
   ubuntu-keyring

   qemu-user-static
   binfmt-qemu-static-all-arch

   mesa-demo
   vulkan-tools
   vulkan-intel
   libva-intel-driver
   libva-utils

   qtcreator
   visual-studio-code-bin
   microsoft-edge-dev
   telegram-desktop
   thunderbird
   anki
   netease-cloud-music-gtk
   remmina
   grsync
   syncthing-gtk
   v2ray
   v2raya
   xmind-2020
   obs-studio
   mpv

   steam
   - ttf-liberation
   - lib32-vulkan-intel

   python-pip
   python-matplotlib
   python-pandas
   python-seaborn

   lutris
   wine
   - lib32-giflib
   - lib32-libldap
   - lib32-gnutls
   - lib32-mpg123
   - lib32-openal
   - lib32-v4l-utils
   - lib32-libpulse
   - lib32-libxcomposite
   - lib32-libxinerama
   - lib32-libxslt
   - lib32-gst-plugins-base-libs

   tealdeer
   cppman
   ```
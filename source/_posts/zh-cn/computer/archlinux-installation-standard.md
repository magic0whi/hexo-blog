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

   You can set the filesystem label later by using `btrfs filesystem label /dev/mapper/cryptroot "arch_os"`

   Preparing the boot partition

   ```console
   # mkfs.fat -F32 /dev/sda1
   ```
3. Configuring mkinitcpio
   I prefer using the sd-encrypt hook with the systemd-base initramfs.
   ```console
   HOOKS=(base **systemd** autodects **keyboard** **sd-vconsole** modconf block **sd-encrypt** filesystems fsck)
   ```

## Btrfs subvolumes with swap (WIP)


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
   # cryptsetup close cryptroot
   ```
   Now mount the newly created subvolumes by using the `subvol=` mount option (with enabled compress `zstd`).
   ```
   # mount -o compress=zstd,subvol=@ /dev/mapper/cryptroot /mnt
   # mount -o compress=zstd,subvol=@home /dev/mapper/cryptroot /mnt/home
   # mount -o compress=zstd,subvol=@snapshots /dev/mapper/cryptroot /mnt/.snapshots
   ```
3. Create nested subvolumes
   Create any nested subvolumes you do **not** want to have snapshots of when taking a snapshot of `/`.
   Every nested subvolume will be an empty directory inside the snapshot.
   Since the `@` subvolume is mounted at `/mnt` you will need to create a subvolume at `/mnt/var/cache/pacman/pkg` as a nested subvolume
   You may have to create any parent directories first.
   ```console
   # mkdir -p /mnt/var/cache/pacman/pkg
   # btrfs subvolume create /mnt/var/cache/pacman/pkg
   ```
4. Configuring swap
   TODO

## Installation

1. Select the mirrors
   sed -i '1s/^/Server = https:\/\/mirrors.huaweicloud.com\/archlinux\/\$repo\/os\/\$arch/' /etc/pacman.d/mirrorlist
2. Install essential packages
   ```console
   # pacstrap /mnt base base-devel linux linux-firmware btrfs-progs vim networkmanager
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
   # hwclock --systohc
   # timedatectl set-ntp true
   ```
4. Localization
   ```console
   # sed -i 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
   # sed -i 's/#zh_CN.UTF-8 UTF-8/zh_CN.UTF-8 UTF-8/g' /etc/locale.gen
   # locale-gen
   # echo "LANG=en_US.UTF-8" >> /etc/locale.conf
   ```
5. Network configuration
   Create the `/etc/hostname` file:
   ```console
   # hostnamectl set-hostname myhostname
   ```
   Add matching entries to [hosts(5)](https://jlk.fjfi.cvut.cz/arch/manpages/man/hosts.5):
   ```conf /etc/hosts
   127.0.0.1	localhost
   ::1          localhost
   127.0.1.1	myhostname.neo	myhostname
   ```
   Enable NetworkManager:
   ```console
   # systemctl enable NetworkManager.service
   ```

6. Root password
   ```
   # passwd
   ```
7. Boot loader
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

   ```conf /boot/loader/entries/arch.conf
   title   Arch Linux
   linux   /vmlinuz-linux
   initrd  /intel-ucode.img
   initrd  /initramfs-linux.img
   options root="LABEL=arch_os" rw **rd.luks.name=sda2-UUID=cryptroot root=/dev/mapper/cryptroot**
   ```

   use `lsblk -f` to show persistent block device naming


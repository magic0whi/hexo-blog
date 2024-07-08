---
title: archlinux-installation-standard
lang: zh-cn
category: computer
date: 2020-10-20 21:46:02
tags:
toc: true
---

My standards of install Arch Linux.

<!-- more -->

## Pre-installation

1. Update the system clock
   ```console
   # timedatectl set-ntp true
   ```
2. Preparing the partitions

   Partition layout:
   ```plaintext
   +-----------------------+------------------------+
   | Boot partition        | LUKS2 encrypted system |
   |                       | partition              |
   |                       |                        |
   | /boot                 | /                      |
   |                       |                        |
   |                       | /dev/mapper/cryptroot  |
   |-----------------------|------------------------|
   | /dev/sda1             | /dev/sda2              |
   +-----------------------+------------------------+
   ```

   Preparing non-boot partitions
   ```console
   # cryptsetup -y -v --pbkdf-memory=114514 luksFormat /dev/sda2
   # cryptsetup open /dev/sda2 cryptroot
   # cryptsetup --allow-discards --perf-no_read_workqueue --perf-no_write_workqueue --persistent refresh cryptroot
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
   # btrfs subvolume create /mnt/@var_log
   ```
2. Mount top-level subvolumes
   Unmount the system partition at /mnt.
   ```console
   # umount /mnt
   ```
   Now mount the newly created subvolumes by using the `subvol=` mount option (with enabled compress `zstd`).
   ```console
   # mount -o compress=zstd,subvol=@,discard=async /dev/mapper/cryptroot /mnt
   # mkdir -p /mnt/{boot,home,.snapshots,var/log}
   # mount -o discard /dev/sda1 /mnt/boot
   # mount -o compress=zstd,subvol=@home,discard=async /dev/mapper/cryptroot /mnt/home
   # mount -o compress=zstd,subvol=@snapshots,discard=async /dev/mapper/cryptroot /mnt/.snapshots
   # mount -o compress=zstd,subvol=@var_log,discard=async /dev/mapper/cryptroot /mnt/var/log
   ```
3. Create nested subvolumes
   Create any nested subvolumes you do **not** want to have snapshots when taking a snapshot of `/`.
   Every nested subvolume will be an empty directory inside the snapshot.
   ```console
   # mkdir -p /mnt/var/cache/pacman
   # btrfs subvolume create /mnt/var/cache/pacman/pkg
   # btrfs subvolume create /mnt/var/tmp
   ```

## Installation

1. Select mirrors
   ```console
   # sed -i '1iServer = https://mirrors.cloud.tencent.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist
   ```

## Configure the system

```console shell
# arch-chroot /mnt
# cd /etc
# git config --global commit.gpgsign true
# git clone --depth=1 --bare https://github.com/magic0whi/proteuslaptop_etc.git .git
# git config core.bare false
# rm /etc/resolv.conf
# git checkout HEAD .
# hwclock --systohc
# # Modify /etc/{crypttab.initramfs,fstab,hostname,hosts,systemd/networks/*}
# # Installing the EFI boot manager
# bootctl install
# # Recreate the initramfs image
# mkinitcpio -P
# rm /boot/*.img
# # Secure boot
# sbctl create-keys
# sbctl enroll-keys -m
# sbctl verify | sed 's/✗ /sbctl sign -s /e'
# locale-gen
# useradd -m -s /bin/zsh <Username>
# passwd <Username>
# passwd root
# su <Username>
$ cd
$ git config --global commit.gpgsign true
$ git clone --depth=1 --bare https://github.com/magic0whi/proteuslaptop_dotfiles.git .dotfiles
$ alias gitdot='git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
$ gitdot config core.bare false
$ gitdot checkout HEAD .
$ exit
```

Configuring the boot loader
```properties /boot/loader/loader.conf
default  arch.conf
timeout  4
console-mode max
editor   no
```

AUR helper [paru](https://aur.archlinux.org/packages/paru/).
```console
# mkdir /build
# chown -R <Username>:<Username> /build
# cd /build
# sudo -u <Username> git clone --depth=1 https://aur.archlinux.org/paru.git
# cd paru
# sudo -u <Username> GITFLAGS="--depth=1" makepkg --noconfirm -si
# pacman -Qtdq | xargs -r pacman --noconfirm -Rcns
# rm -rf /home/<Username>/.cache
# rm -rf /build
```

> Reboot to installed system to ensure that systemd is running.

## Post-installation

### Enable daemons

```console shell
# systemctl enable --now iwd.service
# systemctl enable --now systemd-networkd.service
# systemctl enable --now systemd-resolved.service
$ # Enable Random number generation
# systemctl enable --now rngd.service
$ # Enable sshd
# systemctl enable --now sshd.service
$ # Enable bluetooth auto power-on
# systemctl enable --now bluetooth.service
$ Enable systemd-oomd (Userspace OOM daemon)
# systemctl enable --now systemd-oomd.service
# systemctl enable --now ly.service
$ systemctl --user enable --now clight.service
$ systemctl --user enable --now fcitx5.service
$ systemctl --user enable --now plasma-polkit-agent.service
$ systemctl --user enable --now syncthing.service
$ systemctl --user enable --now waybar.service
```
### Enroll TPM key

list installed TPMs and the driver in use:
```console shell
$ systemd-cryptenroll --tpm2-device=list
```

Binds the key to PCRs 0 and 7 (System firmware and Secure Boot state):
```console
# systemd-cryptenroll --tpm2-device=auto --tpm2-pcrs=0+7 /dev/sda2
```

> To remove a key enrolled, run:
> ```console
> # systemd-cryptenroll --wipe-slot=slot_number /dev/sdX
> ```
> where `slot_number` is the numeric LUKS slot number in which your TPM key is stored.
> Alternatively, run:
> ```console
> # systemd-cryptenroll --wipe-slot=tpm2 /dev/sdX
> ```
> to remove all TPM-associated keys from your LUKS volume.

### Swapfile in a btrfs filesystem and enable hibernation (support dm-crypt)

1. Swap file creation
   ```console
   # btrfs filesystem mkswapfile --size 32g --uuid clear /.snapshots/swapfile
   # swapon /.snapshots/swapfile
   ```
   Add appropriate entry in `fstab`:
   ```properties /etc/fstab
   ...
   /.snapshots/swapfile none swap defaults 0 0
   ```
2. Setting the required kernel parameters
   ```console
   # btrfs inspect-internal map-swapfile -r /.snapshots/swapfile
   198122980
   ```
   ```properties /etc/cmdline.d/root.conf
   ...
   resume=/dev/mapper/cryptroot
   resume_offset=198122980
   ```
   Regenerate UKI:
   ```console
   # mkinitcpio -P
   ```

## Tips

- Run `fc-cache -fv` to rebuild font information cache files.
- Use `blkid` or `lsblk -f` to see the persistent block device naming
- Use `ip link` to show network interface names
- Configure memory pressure killing (Here I set it slice wide to make it observable in `oomctl`):
  ```console shell
  # systemctl edit user.slice
  ```
  Having this in your editor:
  ```properties
  [Slice]
  ManagedOOMMemoryPressure=kill
  ManagedOOMMemoryPressureLimit=50%
  ```
- Configure swap-based killing:
  ```console
  # systemctl edit --force -- -.slice
  ```
  With this in your edior:
  ```properties
  [Slice]
  ManagedOOMSwap=kill
  ```
  
  (Optional) See also [oomd.conf(5)](https://man.archlinux.org/man/oomd.conf.5.en):
  ```properties /etc/systemd/oomd.conf
  [OOM]
  SwapUsedLimit=80%
  DefaultMemoryPressureDurationSec=20s
  ```

  > Furthmore, you can set `OOMPolicy=kill` to a service unit, which says if one of the process belong to this service is being killed by systemd-oomd, the whole service will also get killed (this option sets service's cgroup `memory.oom.group` to `1`, which means all tasks belonging to this cgroup were killed together).

```plaintext Additional Packages
^[AUR] libva-intel-driver-hybrid intel-hybrid-codec-driver

# GPU Tools
[AUR] raytracinginvulkan-git

# Spell checkers
nuspell hunspell-en_us

# CLI Utilities
yt-dlp
gdb
nmap
docker{,-compose}
howdy linux-enable-ir-emitter
pacman-contrib devtools
picocom  # ($ picocom -b 1500000 /dev/ttyUSB0, Ctrl-a Ctrl-q to quit)
tinyxxd
[AUR] cppman
traceroute
wireguard-tools

## Desktop apps
libreoffice-still
remmina libvncserver freerdp
thunderbird
[AUR] yesplaymusic-electron

sunshine

python-pip
python-matplotlib
python-pandas
python-seaborn

## MATLAB
libxcrypt-compat gtk2

## Systemd-nspawn bootstrap
debootstrap ubuntu-keyring

## Manager & VM
samba
[AUR] wsdd2
  ^[AUR] qemu-user-static
   - [AUR] binfmt-qemu-static-all-arch

[AUR] modrinth-app-git flite

qtcreator
fcitx5-material-color
[AUR] qv2ray
[AUR] microsoft-edge-dev-bin
[AUR] visual-studio-code-bin
- gnome-keyring # required to store vscode login token
- seahorse      # GUI to manage keyring
vkd3d-proton-mingw-git
[AUR] snowflake-pt-client-git
```

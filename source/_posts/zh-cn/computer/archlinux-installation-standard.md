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
2. Install essential packages
   ```console
   # pacstrap -K /mnt base linux linux-firmware \
   rng-tools openssh zram-generator bluez bluez-utils iwd zerotier-one \
   btrfs-progs tmux bash-completion udisks2 btop man rsync tealdeer \
   zsh{,-autosuggestions,-syntax-highlighting,-history-substring-search} \
   pipewire wireplumber pipewire-alsa pipewire-pulse \
   base-devel git gvim ripgrep fzf ctags \
   vulkan-tools libva-utils hyfetch \
   xorg-server bspwm sxhkd ly polybar xdo xorg-xrdb picom rofi redshift flameshot alacritty feh \
   noto-fonts{,-cjk,-emoji} \
   fcitx5-im fcitx5-chinese-addons
   ```

## Configure the system

```console shell
# genfstab -U /mnt >> /mnt/etc/fstab
# arch-chroot /mnt
# hwclock --systohc
# locale-gen
$ #Recreate the initramfs image
# mkinitcpio -P
# useradd -m -s /bin/bash <Username>
$ # Setting the new user and root user's password
# passwd <Username>
# passwd root
```

Installing the EFI boot manager
```console shell
# bootctl install
```
Configuring the boot loader
```properties /boot/loader/loader.conf
default  arch.conf
timeout  4
console-mode max
editor   no
```

```properties /boot/loader/entries/arch.conf
title   Arch Linux
linux   /vmlinuz-linux
initrd  /initramfs-linux.img
options root=/dev/mapper/cryptroot rootflags=compress=zstd,subvol=@,discard=async
```
```properties /boot/loader/entries/arch-fallback.conf
title Arch Linux (fallback)
linux /vmlinuz-linux
initrd /initramfs-linux-fallback.img
options root=/dev/mapper/cryptroot rootflags=compress=zstd,subvol=@,discard=async
```

AUR helper [paru](https://aur.archlinux.org/packages/paru/).
1. [Optional] Create makepkg wrapper `makepkg-shallow` to make makepkg do shallow clone
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
2. Build & Install paru
   ```console
   # chmod 755 /usr/bin/makepkg-shallow
   # mkdir /build
   # chown -R <Username>:<Username> /build
   # cd /build
   # sudo -u <Username> git clone --depth=1 https://aur.archlinux.org/paru.git
   # cd paru
   # sudo -u <Username> makepkg-shallow --noconfirm -si
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
# systemctl enable systemd-oomd.service
```
### Enroll TPM key

list installed TPMs and the driver in use:
```console shell
$ systemd-cryptenroll --tpm2-device=list
```
> If you encounter messages such as "<span style="color:#FF0000;">TPM2 support is not installed</span>", try install `tpm2-tss`.

Binds the key to PCRs 0 and 7 (System firmware and Secure Boot state):
```console
# systemd-cryptenroll --tpm2-device=auto --tpm2-pcrs=0+7 /dev/sda2
```

Regenerate the initramfs:
```console
# mkinitcpio -P
```
> To remove a key enrolled, run:
> ```console
> # systemd-cryptenroll /dev/sdX --wipe-slot=slot_number
> ```
> where `slot_number` is the numeric LUKS slot number in which your TPM key is stored.
> Alternatively, run:
> ```console
> # systemd-cryptenroll /dev/sdX --wipe-slot=tpm2
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
   # btrfs inspect-internal map-swapfile -r /swap/swapfile
   198122980
   ```

   Finally, edit the bootloader's configuration:
   ```properties /boot/loader/entries/arch.conf
   ...
   options ... resume=/dev/mapper/cryptroot resume_offset=198122980
   ```

### Secure Boot by using a signed boot loader (shim)

Install `shim-signed`<sup>[AUR]</sup>, `sbsigntools` and `efibootmgr`
```console
# paru -S shim-signed sbsigntools efibootmgr
```
As shim tries to launch `grubx64.efi`, rename systemd boot loader to it.
```console
# cp /boot/EFI/systemd/systemd-bootx64.efi /boot/EFI/BOOT/grubx64.efi
```
Copy shim and MokManager to boot loader directory:
```console
# cp /usr/share/shim-signed/shimx64.efi /boot/EFI/BOOT/BOOTx64.EFI
# cp /usr/share/shim-signed/mmx64.efi /boot/EFI/BOOT/
```
(Optional) create a new NVRAM entry to boot `BOOTx64.EFI`:
```console
# efibootmgr --verbose --disk /dev/sda --part 1 --create --label "Shim" --loader /EFI/BOOT/BOOTx64.EFI
```
Generate a Machine Owner Key:
```console
$ openssl req -newkey rsa:4096 -nodes -keyout MOK.key -new -x509 -sha256 -days 3650 -subj "/CN=my Machine Owner Key/" -out MOK.crt
$ openssl x509 -outform DER -in MOK.crt -out MOK.cer
```
Sign boot loader (named `grubx64.efi`) and kernel:
```console
# sbsign --key MOK.key --cert MOK.crt --output /boot/vmlinuz-linux /boot/vmlinuz-linux
# sbsign --key MOK.key --cert MOK.crt --output /boot/EFI/BOOT/grubx64.efi /boot/EFI/BOOT/grubx64.efi
```

Copy `MOK.cer` to a FAT formatted file system (Here I use EFI system partition).
```console
# cp MOK.cer /boot/
```
Reboot and enable Secure Boot. If shim does not find the certificate `grubx64.efi` is signed with in MokList it will launch MokManager (`mmx64.efi`).
In MokManager select Enroll key from disk, find `MOK.cer` and add it to MokList. When done select Continue boot and your boot loader will launch and it will be capable launching any binary signed with your Machine Owner Key.

### NVIDIA & NVIDIA Optimus

Using `PRIME render offload` which was official method supported by NVIDIA

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
   * Enable runtime power management for each PCI function
     ```console
     # echo auto > /sys/bus/pci/devices/<Domain>:<Bus>:<Device>.<Function>/power/control
     # modprobe nvidia "NVreg_DynamicPowerManagement=0x02"
     ```

   * The automated ways to perform the manual steps mentioned above so that this feature works seamlessly after boot:
     1. Create a file named `80-nvidia-pm.rules` in `/lib/udev/rules.d/` directory
        ```properties /lib/udev/rules.d/80-nvidia-pm.rules
        # Remove NVIDIA Audio devices, if present
        ACTION=="add", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x040300", ATTR{remove}="1"

        # Enable runtime PM for NVIDIA VGA/3D controller devices on driver bind
        ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="auto"

        # Disable runtime PM for NVIDIA VGA/3D controller devices on driver unbind
        ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="on"
        ```
        > You can use `udevadm info --attribute-walk --path=/sys/bus/pci/devices/<Domain>\:<Bus>\:<Slot>.<Function>` to get a PCI device's   attribution
     2. Set the driver option via the kernel module configuration files
        ```properties /etc/modprobe.d/nvidia.conf
        options nvidia "NVreg_DynamicPowerManagement=0x02"
        ```
     3. Reboot the system

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
[AUR] fcitx5-pinyin-zhwiki
fcitx5-material-color

# GPU - Intel
vulkan-intel
intel-media-driver
^[AUR] libva-intel-driver-hybrid
- [AUR] intel-hybrid-codec-driver

# GPU - AMD
mesa
vulkan-radeon
libva-mesa

htop
cmake
gdb
tree
nmap
compsize
bc
p7zip
unrar
openbsd-netcat
traceroute
wireguard-tools
ntfs-3g
docker{,-compose}
howdy
arch-install-scripts pacman-contrib
picocom  # ($ picocom -b 1500000 /dev/ttyUSB0, Ctrl-a Ctrl-q to quit)
[AUR] snowflake-pt-client-git
[AUR] cppman

cmus # Music Player
gdu/ncdu/dust # Calculate storage usage

## Desktop apps
anki-bin
blender
krita
libreoffice-still
mpv
obs-studio
remmina libvncserver freerdp
telegram-desktop
zathura
thunderbird
[AUR] bitwarden
[AUR] yesplaymusic-electron
[AUR] microsoft-edge-dev-bin
[AUR] qv2ray
[AUR] visual-studio-code-bin
- gnome-keyring # required to store vscode login token
- seahorse      # GUI to manage keyring

## Gaming
steam
vkd3d-proton-mingw-git

lutris
innoextract

python-pip
python-matplotlib
python-pandas
python-seaborn

[AUR] hexo-cli
[AUR] clight clightd

texlive-most
- [AUR] tllocalmgr-git

## MATLAB
libxcrypt-compat gtk2

[AUR] wsdd2

## Systemd-nspawn bootstrap
debootstrap ubuntu-keyring

## VM
virt-manager libvirt dmidecode dnsmasq iptables-nft edk2-ovmf swtpm
qemu
  ^[AUR] qemu-user-static
   - [AUR] binfmt-qemu-static-all-arch

[AUR] xray
[AUR] v2raya
[AUR] fcitx5-pinyin-moegirl
qtcreator
```

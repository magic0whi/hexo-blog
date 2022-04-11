---
title: archlinux-installation-standard
lang: zh-cn
category: computer
date: 2020-10-20 21:46:02
tags:
toc: true
---

The standard of archlinux installation & configurations for me

For Lenovo user, Enter `F12` for Boot Menu when on bootstrap stage

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
   # mount -o compress=zstd,subvol=@,discard /dev/mapper/cryptroot /mnt
   # mkdir -p /mnt/{boot,home,.snapshots,var/log}
   # mount -o discard /dev/sda1 /mnt/boot
   # mount -o compress=zstd,subvol=@home,discard /dev/mapper/cryptroot /mnt/home
   # mount -o compress=zstd,subvol=@snapshots,discard /dev/mapper/cryptroot /mnt/.snapshots
   # mount -o compress=zstd,subvol=@var_log,discard /dev/mapper/cryptroot /mnt/var/log
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

## Installation

1. Select the mirrors
   I will use huaweicloud as main mirror
   ```console
   # sed -i '1aServer = https://mirrors.huaweicloud.com/archlinux/$repo/os/$arch\n' /etc/pacman.d/mirrorlist
   ```
2. Install essential packages
   ```console
   # pacstrap /mnt base base-devel linux linux-firmware btrfs-progs vim rng-tools git tmux openssh bash-completion zram-generator bluez bluez-utils snapper
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
   ```properties /etc/hosts
   127.0.0.1   localhost
   ::1         localhost
   127.0.1.1   myhostname.neo    myhostname
   ```

   Choose one of the following methods:
   * Using systemd-networkd & systemd-resolved & iwd
     Install iwd: `pacman -S iwd`
     Wireless adapter configuration
     > Use `ip link` to show network interface names
     ```properties /etc/systemd/network/25-wireless.network
     [Match]
     Name=<Your wireless interface name>

     [Network]
     DHCP=yes
     ```
     Enable daemons and systemd-resolved stub mode:
     ```console
     # systemctl enable iwd.service
     # systemctl enable systemd-networkd.service
     # systemctl enable systemd-resolved.service
     # ln -rsf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
     ```
   * Using NetworkManager
     Install & Enable NetworkManager:
     ```console
     # pacman -S networkmanager
     # systemctl enable NetworkManager.service
     ```
6. Configuring mkinitcpio
   Using the `sd-encrypt` hook with the systemd-base initramfs. (replace hook `udev` with `systemd`)
   <figure class="highlight properties">
     <figcaption><span>/etc/mkinitcpio.conf</span></figcaption>
     <table>
       <tr>
         <td class="gutter">
           <pre><span class="line">1</span><br></pre>
         </td>
         <td class="code">
           <pre><span class="line">HOOKS=(base <b>systemd</b> autodetect <b>keyboard</b> <b>sd-vconsole</b> modconf block <b>sd-encrypt</b> filesystems fsck)</span><br></pre>
         </td>
      </tr>
     </table>
   </figure>

   Configure `/etc/crypttab.initramfs` (As `/etc/crypttab` but in initramfs; Here I enabled Discard/TRIM support for SSD):
   ```properties /etc/crypttab.initramfs
   cryptroot       UUID=<UUID_OF_ROOTFS>       -       discard
   ```
   Recreate the initramfs image
   ```console
   # mkinitcpio -P
   ```
7. Users and password
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
8. Boot loader
    Installing the EFI boot manager
    ```console
    # bootctl install
    ```
    > Automatic update
      The package [systemd-boot-pacman-hook<sup>[AUR]</sup>](https://aur.archlinux.org/packages/systemd-boot-pacman-hook/) provides a Pacman hook to automate the update process.
    > Please be aware that the description of configure Secure Boot below overrides the hook in this package.

    Configuring the boot loader
    ```properties /boot/loader/loader.conf
    default  arch.conf
    timeout  4
    console-mode max
    editor   no
    ```

    > use `lsblk -f` to show persistent block device naming
    ```properties /boot/loader/entries/arch.conf
    title   Arch Linux
    linux   /vmlinuz-linux
    initrd  /initramfs-linux.img
    options root=/dev/mapper/cryptroot rootflags=compress=zstd,subvol=@,discard
    ```
    ```properties /boot/loader/entries/arch-fallback.conf
    title Arch Linux (fallback)
    linux /vmlinuz-linux
    initrd /initramfs-linux-fallback.img
    options root=/dev/mapper/cryptroot rootflags=compress=zstd,subvol=@,discard
    ```

## Post-installation

### Miscellaneous

1. Random number generation
   ```console
   # systemctl enable rngd.service
   ```
2. Enable sshd
   ```console
   # systemctl enable sshd.service
   ```

### Configurate zram-generator

Configuration
```properties /etc/systemd/zram-generator.conf
[zram0]
zram-size = min(min(ram, 4096) + max(ram - 4096, 0) / 2, 32 * 1024)
compression-algorithm = zstd
```
Applying changes
```console
# systemctl daemon-reload
# systemctl restart systemd-zram-setup@zram0
```

### Enable bluetooth auto power-on

```console
# systemctl enable bluetooth.service
```
Bluetooth auto power-on after boot:
```properties /etc/bluetooth/main.conf
[Policy]
AutoEnable=true
```

Bluetooth audio support:
```console
pulseaudio-bluetooth
```

### Userspace OOM daemon

Enable [systemd-oomd](https://fedoraproject.org/wiki/Changes/EnableSystemdOomd)
```console
# systemctl enable systemd-oomd.service
```

Configure memory pressure killing (Here I set it slice wide to make it observable in `oomctl`):
```console
# systemctl edit user.slice
```
Have this in your editor:
```properties
[Slice]
ManagedOOMMemoryPressure=kill
ManagedOOMMemoryPressureLimit=50%
```

Configure swap-based killing:
```console
sudo systemctl edit --force -- -.slice
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

Furthmore, you can set `OOMPolicy=kill` to a service unit, which says if one of the process belong to this service is being killed by systemd-oomd, the whole service will also get killed (this option sets service's cgroup `memory.oom.group` to `1`, which means all tasks belonging to this cgroup were killed together).

### Archlinuxcn's repository

```properties /etc/pacman.conf
[archlinuxcn]
Server = https://repo.archlinuxcn.org/$arch
## or install archlinuxcn-mirrorlist-git and use the mirrorlist
#Include = /etc/pacman.d/archlinuxcn-mirrorlist
```

Install archlinux-keyring:
```console
# pacman -S archlinuxcn-keyring
```

### Swapfile in a btrfs filesystem within dm-crypt and also hibernation support

1. Swap file creation
   Create a zero length file, set the `No_COW` attribute on it with `chattr`, and make sure compression is disabled, then using `dd` to allocate a swap file:
   ```console
   # truncate -s 0 /swapfile
   # chattr +C /swapfile
   # btrfs property set /swapfile compression none
   # dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
   # chmod 600 /swapfile
   # mkswap /swapfile
   # swapon /swapfile
   ```
   Adding the appropriate entry in `fstab`:
   ```properties /etc/fstab
   ...
   /swapfile none swap defaults 0 0
   ```
2. Setting the required kernel parameters
   The `resume_offset` number can be computed using the tool [btrfs_map_physical.c](https://github.com/osandov/osandov-linux/blob/master/scripts/btrfs_map_physical.c). Do not try to use the `filefrag` tool, on Btrfs the "physical" offset you get from `filefrag` is not the real physical offset on disk
   <figure class="highlight console"><table><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br></pre></td><td class="code"><pre><span class="line"><span class="meta prompt_">$ </span><span class="language-bash">curl https://raw.githubusercontent.com/osandov/osandov-linux/master/scripts/btrfs_map_physical.c &gt; btrfs_map_physical.c</span></span><br><span class="line"><span class="meta prompt_">$ </span><span class="language-bash">gcc -O2 -o btrfs_map_physical btrfs_map_physical.c</span></span><br><span class="line"><span class="meta prompt_"># </span><span class="language-bash">./btrfs_map_physical /swapfile</span></span><br><span class="line">FILE OFFSET     FILE SIZE       EXTENT OFFSET   EXTENT TYPE     LOGICAL SIZE    LOGICAL OFFSET  PHYSICAL SIZE   DEVID   PHYSICAL OFFSET</span><br><span class="line">0       134217728       0       regular 134217728       21358571520     134217728       1       <b>22440701952</b></span><br><span class="line">134217728       134217728       0       regular 134217728       21642440704     134217728       1       22724571136</span><br><span class="line">268435456       134217728       0       regular 134217728       22067875840     134217728       1       23150006272</span><br><span class="line">...</span><br><span class="line"><span class="meta prompt_">$ </span><span class="language-bash">getconf PAGESIZE</span></span><br><span class="line">4096</span><br><span class="line"><span class="meta prompt_">$ </span><span class="language-bash"><span class="built_in">echo</span> $((<span class="number">22440701952</span>/<span class="number">4096</span>))</span></span><br><span class="line">5478687</span><br></pre></td></tr></table></figure>

   Note the the first physical offset returned by this tool. In this example, we use `22440701952`. Also note the pagesize that can be found with `getconf PAGESIZE`.
   To compute the `resume_offset` value, divide the physical offset by the pagesize. In this example, it is `22440701952 / 4096 = 5478687`.

   Finally, edit the boot loader's configuration:
   <figure class="highlight properties"><figcaption><span>/boot/loader/entries/arch.conf</span></figcaption><table><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br></pre></td><td class="code"><pre><span class="line"><span class="attr">title</span>   <span class="string">Arch Linux</span></span><br><span class="line"><span class="attr">linux</span>   <span class="string">/vmlinuz-linux</span></span><br><span class="line"><span class="attr">initrd</span>  <span class="string">/initramfs-linux.img</span></span><br><span class="line"><span class="attr">options</span> <span class="string">... <b>resume=/dev/mapper/cryptroot resume_offset=5478687</b></span></span><br></pre></td></tr></table></figure>

### AUR helper

Here I Using [paru](https://aur.archlinux.org/packages/paru/) as AUR helper.

1. Create makepkg wrapper `makepkg-shallow` to make makepkg do shallow clone
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

> Rebooting to installed system to ensure that systemd is running.

### Btrfs snapshots & Snapper

Ensure `/.snapshots` is not mounted and does not exist as folder:
```console
# umount /.snapshots
# rm -r /.snapshots
```
Then create a new configuration for `/`. Snapper create-config automatically creates a subvolume `.snapshots` with the root subvolume `@` as its parent, that is not needed for current filesystem layout, and can be deleted.
```console
# snapper -c root create-config /
# btrfs subvolume delete /.snapshots
# mkdir /.snapshots
```
Now mount `@snapshots` to `/.snapshots`:
```console
# mount -o compress=zstd,subvol=@snapshots,discard /dev/mapper/cryptroot /.snapshots
```

Pacman Hook:
```console
pacman -S snap-pac
```

### Unlocking encrypted root filesystem by using TPM 2

list installed TPMs and the driver in use:
```console
$ systemd-cryptenroll --tpm2-device=list
```
> If you encounter such message "<span style="color:#FF0000;">TPM2 support is not installed</span>" then try to install `tpm2-tss`

A key may be enrolled in both the TPM and the LUKS volume using only one command. The following example binds the key to PCRs 0 and 7 (the system firmware and Secure Boot state):
```console
# systemd-cryptenroll --tpm2-device=/path/to/tpm2_device --tpm2-pcrs=0+7 /dev/sda2
```
> Tip: If your computer has only one TPM installed, which is usually the case, you may instead specify `--tpm2-device=auto` to automatically select the only available TPM.

Specifying the root volume using the `/etc/crypttab.initramfs`:
```properties /etc/crypttab.initramfs
cryptroot       UUID=<UUID_OF_ROOTFS>       -       tpm2-device=auto,discard
```
Regenerate the initramfs:
```console
# mkinitcpio -P
```
> To remove a key enrolled using this method, run:
> ```console
> # systemd-cryptenroll /dev/sdX --wipe-slot=slot_number
> ```
> where `slot_number` is the numeric LUKS slot number in which your TPM key is stored.
> Alternatively, run:
> ```console
> # systemd-cryptenroll /dev/sdX --wipe-slot=tpm2
> ```
> to remove all TPM-associated keys from your LUKS volume.

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
We can automate the kernel signing with a pacman hook:
> Create pacman's default hooks directory if it doesn's exist:
> ```console
> # mkdir /etc/pacman.d/hooks
> ```

```properties /etc/pacman.d/hooks/999-sign_kernel_for_secureboot.hook
[Trigger]
Operation = Install
Operation = Upgrade
Type = Package
Target = linux
Target = linux-lts
Target = linux-hardened
Target = linux-zen

[Action]
Description = Signing kernel with Machine Owner Key for Secure Boot
When = PostTransaction
Exec = /usr/bin/find /boot/ -maxdepth 1 -name 'vmlinuz-*' -exec /usr/bin/sh -c 'if ! /usr/bin/sbverify --list {} 2>/dev/null | /usr/bin/grep -q "signature certificates"; then /usr/bin/sbsign --key /etc/pacman.d/hooks/MOK.key --cert /etc/pacman.d/hooks/MOK.crt --output {} {}; fi' ;
Depends = sbsigntools
Depends = findutils
Depends = grep
```
Also, there is a pacman hook for systemd-boot upgrades:
```properties /etc/pacman.d/hooks/100-systemd-boot.hook
[Trigger]
Type = Package
Operation = Upgrade
Target = systemd

[Action]
Description = Gracefully upgrading systemd-boot...
When = PostTransaction
Exec = /bin/sh -c '/usr/bin/systemctl restart systemd-boot-update.service && cp /boot/EFI/systemd/systemd-bootx64.efi /boot/EFI/BOOT/grubx64.efi && cp /usr/share/shim-signed/shimx64.efi /boot/EFI/BOOT/BOOTx64.EFI && sbsign --key /etc/pacman.d/hooks/MOK.key --cert /etc/pacman.d/hooks/MOK.crt --output /boot/EFI/BOOT/grubx64.efi /boot/EFI/BOOT/grubx64.efi'
Depends = shim-signed
Depends = sbsigntools
```

Then copy `Mok.key` and `MOK.crt` to the path which is specified above:
```console
# cp MOK.key MOK.crt /etc/pacman.d/hooks/
```
Copy `MOK.cer` to a FAT formatted file system (you can use EFI system partition).
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

## Desktop Environment

1. Installation
   * KDE
     ```console
     # pacman -S plasma-meta konsole dolphin ark kio-fuse
     # systemctl enable sddm.service
     ```
   * GNOME
     > Some packages require archlinuxcn's repository
     ```console
     $ paru -S gnome-shell gnome-shell-extensions gdm \
     nautilus file-roller gvfs-mtp sushi seahorse eog \
     gnome-{control-center,terminal,tweaks,keyring,backgrounds,clocks,logs,screenshot,menus} \
     gtk-engine-murrine materia-gtk-theme \
     dconf-editor loginized gpaste
     # systemctl enable gdm.service
     ```
2. (Optional) Install & Configure input method:
   ```console
   # pacman -S fcitx5-im fcitx5-chinese-addons
   # cp /usr/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart/
   ```
   ```shell ~/.pam_environment
   GTK_IM_MODULE DEFAULT=fcitx
   QT_IM_MODULE  DEFAULT=fcitx
   XMODIFIERS    DEFAULT=\@im=fcitx
   ```
3. (Optional|KDE) Turn off screen (DPMS) together with locking session:
   Go to: System Settings > Notifications > Applications(The button "Configure") > Search "Screen Saver" > Configure Events:
   Select Screen locked and check box "Run command", paste `/bin/sleep 2; /usr/bin/xset dpms force off` into it.

## Troubleshot

1. Disable media automount in GNOME
   ```console
   $ gsettings set org.gnome.desktop.media-handling automount false
   $ gsettings set org.gnome.desktop.media-handling automount-open false 
   ```
2. Video Decode is disabled in Microsoft Edge
   ```properties ~/.config/microsoft-edge-dev-flags.conf
   --ignore-gpu-blocklist
   --enable-features=VaapiVideoDecoder
   --enable-accelerated-video-decode
   ```
3. In Surface Devices:
   > I use KDE for more smooth experience
   * Instal linux-surface kernel
     ```console
     $ curl -s https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \
         | sudo pacman-key --add -
     $ sudo pacman-key --finger 56C464BAAC421453
     $ sudo pacman-key --lsign-key 56C464BAAC421453
     ```
     Add the repository:
     ```properties /etc/pacman.conf
     [linux-surface]
     Server = https://pkg.surfacelinux.com/arch/
     ```
     Then you can install the linux-surface kernel and its dependencies. You should also enable the iptsd service for touchscreen and stylus support:
     ```console
     $ sudo pacman -Syu
     $ sudo pacman -S linux-surface linux-surface-headers iptsd
     $ sudo systemctl enable iptsd
     ```
     Don't forget to change the corresponding loader entries' config
   * SDDM has problem with NetworkManager & Long loading time:
     Use GDM as a replace, don't forget to configure KDE Wallet's PAM:
     ```properties /etc/pam.d/gdm-password
     ...
     auth            optional        pam_kwallet5.so
     session         optional        pam_kwallet5.so auto_start
     ```
   * Auto disable touch screen after login (X11):
     First install `xorg-xinput`.
     Then create a shell script:
     ```bash disable_touch_screen.sh
     #!/bin/bash
     if [[ ${XDG_SESSION_TYPE} = "x11" ]]; then
         xinput disable `xinput list | egrep -o "IPTS Touch.+id=[0-9]+" | egrep -o "[0-9]+"`
     fi
     ```
     Finally go to System Settings > Startup and Shutdown > Autostart:
     Press Add button and select Add Login Script, choose the script you just created.

## Additional Packages

   ```plaintext
   ## GNOME
   [AUR] gnome-shell-extension-appindicator
   [AUR] gnome-shell-extension-kimpanel-git
   [AUR] gnome-shell-extension-dash-to-dock
   [AUR] gnome-shell-extension-desktop-icons-ng
   [AUR] gnome-shell-extension-freon-git

   noto-fonts{,-cjk,-emoji}

   [AUR] fcitx5-pinyin-zhwiki
   fcitx5-material-color

   ## GPU
   xf86-video-intel
   vulkan-intel
   vulkan-tools
   libva-utils
   intel-media-driver
   ^[AUR] libva-intel-driver-hybrid

   ## Misc
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
   rsync
   tree
   nmap
   traceroute
   compsize
   wireguard-tools
   picocom  # ($ picocom -b 1500000 /dev/ttyUSB0, Ctrl-a Ctrl-q to quit)
   [AUR] tealdeer
   [AUR] cppman
   ncdu
   openbsd-netcat
   bc
   p7zip
   unrar

   zsh
   zsh-autosuggestions
   zsh-syntax-highlighting
   zsh-history-substring-search
   
   ## Desktop apps
   anki
   blender
   krita
   libreoffice-still
   mpv
   obs-studio
   remmina
   - libvncserver
   - freerdp
   telegram-desktop
   thunderbird

   [AUR] bitwarden
   [AUR] yesplaymusic-electron
   [AUR] microsoft-edge-dev-bin
   [AUR] qv2ray
   [AUR] visual-studio-code-bin
   - gnome-keyring # required to store vscode login token
   - seahorse      # GUI to manage keyring

   ## Gaming
   ## Yep, probably the fastest way to set up a 32 runtime environment
   steam
   - ttf-liberation
   - lib32-vulkan-intel

   lutris
   wine
   - lib32-giflib
   - lib32-gnutls
   - lib32-mpg123
   - lib32-openal
   - lib32-v4l-utils
   - lib32-libpulse
   - lib32-libxcomposite
   - lib32-libxinerama
   - lib32-libxslt
   - lib32-gst-plugins-base-libs

   python-pip
   python-matplotlib
   python-pandas
   python-seaborn

   npm
   [AUR] hexo-cli

   texlive-most
   - [AUR] tllocalmgr-git

   ## MATLAB
   libxcrypt-compat
   gtk2

   ## KDE file shareing
   ## This enable dolphin accesses fileshares of another Windows machine.
   samba
   kdenetwork-filesharing
   [AUR] wsdd2

   ## Vim's things
   ripgrep
   fzf
   ctags

   ## Systemd-nspawn bootstrap
   debootstrap
   ubuntu-keyring

   ## VM
   virt-manager
   libvirt
   - dmidecode
   - dnsmasq
   - ebtables
   - edk2-ovmf
   - qemu
     ^[AUR] qemu-user-static
      - [AUR] binfmt-qemu-static-all-arch

   [AUR] xray
   [AUR] v2raya
   [Archlinuxcn] fcitx5-pinyin-moegirl
   [AUR] syncthing-gtk
   qtcreator
   ```

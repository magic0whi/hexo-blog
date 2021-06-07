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
   ```console
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

## Installation

1. Select the mirrors
   I will use huaweicloud as main mirror
   ```console
   # sed -i '1aServer = https://mirrors.huaweicloud.com/archlinux/$repo/os/$arch\n' /etc/pacman.d/mirrorlist
   ```
2. Install essential packages
   ```console
   # pacstrap /mnt base base-devel linux linux-firmware btrfs-progs vim rng-tools git tmux openssh bash-completion systemd-swap bluez bluez-utils
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
   * Use NetworkManager
     Install & Enable NetworkManager:
     ```console
     # pacman -S networkmanager
     # systemctl enable NetworkManager.service
     # systemctl enable systemd-resolved
     ```
   * Use iwd
     Install iwd: `pacman -S iwd`
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
   HOOKS=(base **systemd** autodetect **keyboard** **sd-vconsole** modconf block **sd-encrypt** filesystems fsck)
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
    > Automatic update
      The package [systemd-boot-pacman-hook](https://aur.archlinux.org/packages/systemd-boot-pacman-hook/) provides a Pacman hook to automate the update process.

    Configuring the boot loader
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
    options rd.luks.name=<sda2-UUID>=cryptroot rd.luks.options=discard root=/dev/mapper/cryptroot rootflags=compress=zstd,subvol=@
    ```
    ```conf /boot/loader/entries/arch-fallback.conf
    title Arch Linux (fallback)
    linux /vmlinuz-linux
    initrd /initramfs-linux-fallback.img
    options rd.luks.name=7fb34a50-f2ae-4799-afc1-85c064a28fcc=cryptroot rd.luks.options=discard root=/dev/mapper/cryptroot rootflags=compress=zstd,subvol=@
    ```
11. (Optional) Enable sshd
    ```console
    # systemctl enable sshd.service
    ```
12. (Optional) Enable ZSWAP, ZRAM, SWAPFC
    Configuration
    ```conf /etc/systemd/swap.conf
    ...
    zswap_enabled=1
    ...
    zram_enabled=1
    ...
    swapfc_enabled=1
    ...
    ```
    Enable systemd-swap
    ```console
    # systemctl enable systemd-swap
    ```
13. (Optional) Enable bluetooth
    ```console
    # systemctl enable bluetooth.service
    ```

    Bluetooth auto power-on after boot:
    ```conf /etc/bluetooth/main.conf
    [Policy]
    AutoEnable=true
    ```
14. (Optional) Add Archlinuxcn repository
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

## Desktop Environment

1. Installation
   * KDE
     ```console
     # pacman plasma-meta kde-system-meta kde-utilities-meta kde-utilities-meta kde-system-meta kde-network-meta
     # systemctl enable sddm.service
     ```
   * GNOME
     > Some packages require archlinuxcn's repository
     ```console
     $ yay -S gnome-shell gnome-shell-extensions gdm \
     nautilus file-roller sushi seahorse eog \
     gnome-{control-center,terminal,tweaks,keyring,backgrounds,clocks,logs,screenshot,menus} \
     gtk-engine-murrine materia-gtk-theme \
     dconf-editor loginized
     # systemctl enable gdm.service
     ```
2. (Optional) Install & Configure input method:
   ```console
   # pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-configtool
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
   * Enable runtime power management for each PCI function
     ```console
     # echo auto > /sys/bus/pci/devices/<Domain>:<Bus>:<Device>.<Function>/power/control
     # modprobe nvidia "NVreg_DynamicPowerManagement=0x02"
     ```
   
   * The automated ways to perform the manual steps mentioned above so that this feature works seamlessly after boot:
     1. Create a file named `80-nvidia-pm.rules` in `/lib/udev/rules.d/` directory
        ```conf /lib/udev/rules.d/80-nvidia-pm.rules
        # Remove NVIDIA Audio devices, if present
        ACTION=="add", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x040300", ATTR{remove}="1"

        # Enable runtime PM for NVIDIA VGA/3D controller devices on driver bind
        ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="auto"

        # Disable runtime PM for NVIDIA VGA/3D controller devices on driver unbind
        ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="on"
        ```
        > You can use `udevadm info --attribute-walk --path=/sys/bus/pci/devices/<Domain>\:<Bus>\:<Slot>.<Function>` to get a PCI device's   attribution
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
   $ export GOPROXY=https://goproxy.io,direct
   ```
3. Disable media automount in GNOME
   ```console
   $ gsettings set org.gnome.desktop.media-handling automount false
   $ gsettings set org.gnome.desktop.media-handling automount-open false 
   ```
4. Video Decode is disabled in Microsoft Edge
   ```conf ~/.config/microsoft-edge-dev-flags.conf
   --ignore-gpu-blocklist
   --enable-features=VaapiVideoDecoder
   --enable-accelerated-video-decode
   ```
5. In Surface Devices:
   > I use KDE for more smooth experience
   * Instal linux-surface kernel
     ```console
     $ curl -s https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \
         | sudo pacman-key --add -
     $ sudo pacman-key --finger 56C464BAAC421453
     $ sudo pacman-key --lsign-key 56C464BAAC421453
     ```
     Add the repository:
     ```conf /etc/pacman.conf
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
     ```conf /etc/pam.d/gdm-password
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

1. Additional Packages
   ```
   [AUR] gnome-shell-extension-appindicator
   [AUR] gnome-shell-extension-kimpanel-git
   [AUR] gnome-shell-extension-dash-to-dock
   [AUR] gnome-shell-extension-desktop-icons-ng
   [AUR] gnome-shell-extension-freon-git
   gvfs-mtp
   gpaste

   noto-fonts{,-cjk,-emoji}

   xf86-video-intel
   vulkan-intel
   vulkan-tools
   libva-utils
   ^[AUR] libva-intel-driver-hybrid
   ^intel-media-driver

   [AUR] fcitx5-pinyin-zhwiki
   [AUR] fcitx5-skin-material

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
   picocom  ($ picocom -b 1500000 /dev/ttyUSB0, Ctrl-a Ctrl-q to quit)
   [AUR] tealdeer
   ncdu
   [AUR] cppman
   openbsd-netcat
   bc

   zsh
   zsh-autosuggestions
   zsh-syntax-highlighting
   zsh-history-substring-search

   npm
   [AUR] nodejs-hexo-cli
   
   telegram-desktop
   thunderbird
   anki
   remmina
   - libvncserver
   - freerdp
   v2ray
   texlive-most
   - [AUR] tllocalmgr-git
   libreoffice-still
   blender
   krita
   [AUR] bitwarden
   [AUR] qv2ray-dev-git
   [AUR] v2ray-cap-git
   [AUR] cgproxy

   obs-studio
   mpv
   [AUR] visual-studio-code-bin
   [AUR] microsoft-edge-dev-bin
   [AUR] xmind-2020
   [AUR] netease-cloud-music-gtk

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
   - lib32-gnutls
   - lib32-mpg123
   - lib32-openal
   - lib32-v4l-utils
   - lib32-libpulse
   - lib32-libxcomposite
   - lib32-libxinerama
   - lib32-libxslt
   - lib32-gst-plugins-base-libs

   debootstrap
   ubuntu-keyring

   virt-manager
   libvirt
   - dmidecode
   - dnsmasq
   - ebtables
   - ^qemu
     ^[AUR] qemu-user-static
        [AUR] binfmt-qemu-static-all-arch
   edk2-ovmf

   [AUR] v2raya
   [Archlinuxcn] fcitx5-pinyin-moegirl
   [AUR] syncthing-gtk
   qtcreator
   ```

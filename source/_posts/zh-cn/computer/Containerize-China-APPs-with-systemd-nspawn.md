---
title: Containerize China APPs with systemd-nspawn
toc: true
category: computer
date: 2022-03-10 14:45:53
tags:
---

A prescription for 国产软件洁癖症. Thanks to liolok's work<sup id="cite_ref-1"><a href="#cite_note-1">[1]</a></sup>

<!-- more -->

## Container bringup

Here using Tencent Meeting as an example

```console
$ su
# cd /var/lib/machines
# container_name=wemeet
# btrfs subvolume create ${container_name}

# codename=focal
# repository_url='https://repo.huaweicloud.com/ubuntu/'
# debootstrap --include=systemd-container \
--components=main,universe,multiverse \
${codename} ${container_name} ${repository_url}
```

Get into the container just created:
```console
# cp /path/to/TencentMeeting_0300000000_2.8.0.3_x86_64_default.publish.deb /var/lib/machines/container_name/root/
# systemd-nspawn --machine=${container_name} --bind-ro=/etc/resolv.conf
root@wemeet # dpkg -i TencentMeeting_0300000000_2.8.0.3_x86_64_default.publish.deb
root@wemeet # apt -f install
root@wemeet # apt install xorg # If needed install xorg

root@wemeet # ln -s /opt/wemeet/wemeetapp.sh /usr/local/bin/wemeet
root@wemeet # useradd -m wemeet
root@wemeet # su --login wemeet --shell /bin/bash
wemeet@wemeet $ mkdir --parents ~/.config ~/.local/share
```

## Wrap up

1. Desktop shortcut
   ```properties ~/.local/share/applications/wemeet.desktop
   [Desktop Entry]
   Comment=Tencent Video Conferencing
   Exec=/home/proteus/.local/bin/wemeet
   Icon=/path/to/wemeetapp.png
   Name=Tencent Meeting
   Terminal=false
   Type=Application
   ```
2. Startup script
   ```bash  ~/.local/bin/wemeet
   #!/bin/bash
   
   app=wemeet
   home=/home/${app}
   host_data=/usr/share
   data=${home}/.local/share
   host_conf=${XDG_CONFIG_HOME:-~/.config}
   conf=${home}/.config
   options="--as-pid2 --machine=${app} --user=${app} --chdir=${home}"
   
   # Tray
   host_dbus=${DBUS_SESSION_BUS_ADDRESS#unix:path=}
   if [ -z "${host_dbus}" ]; then
           host_dbus="/run/user/$(id --user)/bus"
   fi
   dbus=/run/user/host/bus
   options="${options} --bind-ro=${host_dbus}:${dbus} --setenv=DBUS_SESSION_BUS_ADDRESS=unix:path=${dbus}"
   
   # Sound
   host_pulse=${PULSE_SERVER#unix:}
   if [ -z "${host_pulse}"]; then
           host_pulse="/run/user/$(id --user)/pulse/native"
   fi
   pulse=/run/user/host/pulse
   options="${options} --bind-ro=${host_pulse}:${pulse} --setenv=PULSE_SERVER=unix:${pulse}" 
   
   # Icons
   options="${options} --bind-ro=${host_data}/icons:${data}/icons --setenv=XCURSOR_PATH=${data}/icons"
   
   # Fonts
   options="${options} --bind-ro=${host_data}/fonts:${data}/fonts --bind-ro=${host_conf}/fontconfig:${conf}/fontconfig"
   
   # Display
   options="${options} --bind-ro=/tmp/.X11-unix/ --setenv=DISPLAY=$DISPLAY"
   xauth_file="/tmp/xauth-${app}"
   touch ${xauth_file}
   xauth nextract - "${DISPLAY}" | sed -e 's/^..../ffff/' | xauth -f "${xauth_file}" nmerge -
   options="${options} --bind=${xauth_file} --setenv=XAUTHORITY=${xauth_file}"
   
   # Devices
   options="${options} --bind=/dev/dri/ --property=DeviceAllow='char-drm rw'" # Graphic cards
   options="${options} --bind=/dev/input/ --property=DeviceAllow='char-input r'" # Joysticks
   
   echo "List of cmdline options applied to systemd-nspawn:"
   printf "%s\n" ${options} | sort
   
   # resolv.conf
   options="${options} --bind-ro=/etc/resolv.conf"
   
   kdesu bash -c "systemd-nspawn ${options} ${app}"
   ```
   > Please be aware that here I use `kdesu` as an alternative to `sudo`, which is include in KDE desktop.
   > Don't forget to give executable permission `chmod +x  ~/.local/bin/wemeet`

## Reference

<ol>
  <li id="cite_note-1">
    <b><a href="#cite_ref-1">^</a></b> <a href="https://liolok.com/containerize-steam-with-systemd-nspawn
">"Containerize Steam with systemd-nspawn"</a>. <i>liolok.com</i>. Retrieved 2022-3-10.
  </li>
</ol>
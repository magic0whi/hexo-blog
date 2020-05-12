---
title: cross-compile-config
category: computer
lang: zh-cn
date: 2020-05-12 12:35:43
tags:
toc: true
---

使用 distcc + crosstool-ng + ccache 的交叉编译平台搭建

<!-- more -->

## Slave 配置(x86-to-aarch64)

1. crosstool-ng的编译配置文件: {% asset_link xtools-dotconfig-v8 "xtools-dotconfig-v8" %}
2. crosstool-ng编译好后改名:
   ```bash ~/home/your_user/x-tools8-new/aarch64-unknown-linux-gnu/bin/rename.sh
   #!/bin/bash
   for file in `ls`; do
        if [[ "$file" == "link" ]]; then
                continue
        fi
        # uncomment for v8
        ln -s $file ${file#aarch64-unknown-linux-gnu-}
   done
   ```
3. distccd 配置
   ```bash /etc/conf.d/distccd
   DISTCC_ARGS="--allow 192.168.2.0/24 --jobs 8"
   ```
4. 建立一个叫 `distccd8.service` 的 systemd 服务
   ```service /etc/systemd/system/distccd8.service
   [Unit]
   Description=A distributed C/C++ compiler (ARMv8)
   Documentation=man:distccd(1)
   After=network.target

   [Service]
   User=sudaku
   EnvironmentFile=/etc/conf.d/distccd
   Environment="PATH=/usr/lib/ccache/bin/:/home/sudaku/x-tools8-new/aarch64-unknown-linux-gnu/bin:/usr/bin"
   ExecStart=/usr/bin/distccd --no-detach --daemon $DISTCC_ARGS --log-file=/tmp/distccd8.log --port 3932 --stats-port 3933

   [Install]
   WantedBy=multi-user.target
   ```
5. ccache配置
   ```console
   # ln -sf /usr/bin/ccache /usr/lib/ccache/bin/aarch64-unknown-linux-gnu-c++
   # ln -sf /usr/bin/ccache /usr/lib/ccache/bin/aarch64-unknown-linux-gnu-gcc
   # ln -sf /usr/bin/ccache /usr/lib/ccache/bin/aarch64-unknown-linux-gnu-g++
   ```
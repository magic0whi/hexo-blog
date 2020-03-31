---
title: linux
category: computer
date: 2020-02-08 16:42:06
tags:
---

记录linux的配置修改

<!-- more -->

## 配置文件

### ssh

```conf .ssh/config
# ssh代理设置
ServerAliveInterval 22
ProxyCommand nc -v -x 127.0.0.1:1080 %h %p

# 压缩频宽
Compression yes

# 减少重复连线的时间
ControlMaster auto
ControlPath /tmp/ssh-%r@%h:%p

# 延长连线时间
ControlPresist yes

# github的密钥配置
Host github.com
    User git
    IdentityFile ~/.ssh/id_ed25519.github
```

### ~/.bashrc

{% asset_link bashrc.example ".bashrc" %}

### ~/.zshrc

{% asset_link zshrc.example ".zshrc" %}

### aria2

{% asset_link aria2.conf.example "aria2.conf" %}

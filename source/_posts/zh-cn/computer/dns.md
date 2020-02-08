---
title: dns
category: computer
date: 2020-02-08 16:41:37
tags:
---

一个简单的局域网DNS

<!-- more -->

架构 Overture -> Unbound -> dnscrypt-proxy

三者监听端口分别是53, 5354, 5353

Unbound通过[dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list.git)将大陆域名请求走114, 其余请求交给dnscrypt-proxy

Overture为了局域网能够使用ECS而作为前置给到unbound的请求设置dns-subnet

## Unbound 配置

{% asset_link unbound.conf.example "/etc/unbound/unbound.conf" %}

## dnscrypt-proxy配置

修改端口, `# systemctl edit --full dnscrypt-proxy.socket` :

```
[Socket]
ListenStream=127.0.0.1:5353
ListenDatagram=127.0.0.1:5353
ListenStream=[::1]:5353
ListenDatagram=[::1]:5353
```

上游dns服务器:

```toml /etc/dnscrypt-proxy/dnscrypt-proxy.toml
server_names = ['geekdns-doh-east', 'rubyfish-ea', 'cloudflare']
```

## Overture 配置

{% asset_link config.json.example "/etc/config.json" %}

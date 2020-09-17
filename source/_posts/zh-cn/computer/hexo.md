---
title: Hexo 配置改动及常用命令
category: computer
date: 2020-02-08 12:24:49
tags:
toc: true
---

记录在搭建 Hexo 博客的途中都折腾了哪些内容

本文所有配置皆为**增量改动**

<!-- more -->

## 配置改动

### Hexo 设置

```yml hexo/_config.yml
# 标签页标题
title: Ndoskrnl's blog
description: 'There is nothing to say now'
author: Ndoskrnl
# 多语言支持
language:
- zh-cn
- en
# 时区
timezone: 'Asia/Shanghai'

# Url
url: http://blog.ndoskrnl.net
# 文章永久链接(目录)
permalink: :lang/:category/:year/:title/
# 永久链接变量默认值配置
permalink_defaults:
  lang: en

# 新文章文件目录
new_post_name: :lang/:category/:title.md

# 资源文件夹
post_asset_folder: true

# 主题
theme: icarus

# 解决半角符号渲染成全角的问题
marked:
  smartypants: false
```

### Icarus 主题设置

主题设置由于改动较多且时效性不高, 这里不作记录
具体请见主题配置文件 `theme/icarus/_config.yml`

将主页样式变为归档页形式: 直接将`theme/icarus/layout/index.jsx` 替换成 `theme/icarus/layout/archive.jsx` 内的内容

### Git Hook

```bash /home/git/blog.git/hooks/post-update
#!bin/bash
# 首先删除旧文件
# 启用extglob才能用!(xx)之类的语法
shopt -s extglob
rm -rf /srv/git/www/!(node_modules)

# git checkout
git --work-tree=/srv/git/www --git-dir=/srv/git/blog.git checkout -f

# 进入网页文件夹目录
cd /srv/git/www

# 若node_modules文件夹不存在, 安装依赖插件
if [ ! -d /srv/git/www/node_modules ]; then
    npm install
fi

# 若package.json有改动, 更新插件
CHANGED=$(git --git-dir=/srv/git/blog.git diff HEAD^! --stat -- package.json | wc -l)
if [ $CHANGED -gt 0 ]; then
    rm -rf /srv/git/www/node_modules
    npm install
fi

# 生成静态网页
hexo g
```

### 额外安装的插件

```
hexo-symbols-count-time # 字数统计
hexo-renderer-markdown-it # 更快的渲染器(需要卸载默认的hexo-renderer-marked)
hexo-tag-mplayer # 音乐播放器
hexo-filter-plantuml # UML Diagram for hexo
```

## 部署教程

以上假设已完成以下操作

```console
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

### 部署Git仓库

```console
root@blog # useradd -m -s /bin/bash git    #创建git用户
root@blog # passwd git    #给git用户设置密码,或者在git用户的.ssh目录的authorized_keys文件里面添加自己的公钥
root@blog # su git    #切换到git用户
git@blog ~$ cd /home/git
git@blog ~$ git init --bare blog.git

ndoskrnl@pc ~$ cd hexo
ndoskrnl@pc hexo$ git init
ndoskrnl@pc hexo$ git add .
ndoskrnl@pc hexo$ git commit -m 'initial commit'
ndoskrnl@pc hexo$ git remote add origin git@blog:~/blog.git    #这里的blog就是你的git服务器的ip或者域名
ndoskrnl@pc hexo$ git push -u origin master
```

然后就是Git Hook的配置, 见[前面](#Git-Hook)
别忘了 chmod +x post-update
若想使用git-shell: `usermod -s /usr/bin/git-shell git`

### Nginx配置

```console
# mkdir -p /var/www/blog    #在服务器选择一个放置网站的目录, 假设这个目录为/var/www/blog
# chown git:git -R /var/www/blog    #设置权限
```

Nginx配置文件见: {% post_link nginx "nginx配置改动" %}

## Hexo 文章加密

```console
$ cd blog
$ npm install hexo-blog-encrypt
$ nano /Hexo/_config.yml
```
添加如下内容
```yml
# Security
## 文章加密 hexo-blog-encrypt
encrypt:
    enable: true
```
然后在想加密的文章头部添加上对应字段, 如
```yml
---
title: hello world
date: 2016-03-30 21:18:02
tags:
toc: true
password: 12345
abstract: 是该博文的摘要
message: 这个是博文查看时, 密码输入框上面的描述性文字
---
```

另见[Github项目说明](https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md)

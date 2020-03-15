---
title: Hexo 配置改动及常用命令
category: computer
date: 2020-02-08 12:24:49
tags:
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
theme: next

# 站内搜索[1/2]
search:
  path: search.xml
  field: post
  format: html
  limit: 10000

# 解决半角符号渲染成全角的问题
marked:
  smartypants: false
```

### NexT 主题设置

```yml hexo/theme/next/_config.yml
# 展开所有各级标题
toc:
  ...
  expand_all: true

# 主题
scheme: Gemini

# 关于页面, 添加关于, 标签, 分类页面
menu:
  ...
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th

# Back to top 按钮
back2top:
  scrollpercent: true # 阅读进度百分比

# 页脚
footer:
  since: 2020
  ...
  powered:
    ...
    version: false # 不显示具体版本
  theme:
    ...
    version: false # 不显示具体版本

# 阅读字数统计和时间估计
symbols_count_time:
  awl: 2 # 多少字符统计为一个字, 中文为2
  wpm: 250 # 一分钟读多少字

# 禁止百度提供"移动端优化版"网页
disable_baidu_transformation: true

# 站内搜索[2/2]
local_search:
  enable: true

font:
  enable: true
  ...
  global:
    ...
    family: Noto Serif SC # 谷歌思源宋体
    size: 0.875 # 更好的字体大小, 约14px

# 文本居左以避免出现奇怪的空格间距
text_align:
  desktop: start
  mobile: start
```

### Git Hook

```bash /home/git/blog.git/hooks/post-update
#!bin/bash
# 首先删除旧文件
# 启用extglob才能用!(xx)之类的语法
shopt -s extglob
rm -rf /var/www/blog/!(node_modules)

# git checkout
git --work-tree=/var/www/blog --git-dir=/home/git/blog.git checkout -f


# 安装依赖, 生成静态网页
cd /var/www/blog
if [ ! -d /var/www/blog/node_modules ]; then
    npm install;
    npm install hexo-symbols-count-time
    npm install hexo-generator-searchdb
fi

hexo g
```

### 额外需要的插件

```
hexo-symbols-count-time # 字数统计
hexo-generator-searchdb # 站内搜索
```

## 部署教程

以上假设已完成以下操作

```bash
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

### 部署Git仓库

```bash
#创建git用户
useradd -m -s /bin/bash git
#给git用户设置密码,或者在git用户的.ssh目录的authorized_keys文件里面添加自己的公钥
passwd git
#切换到git用户
su git
cd /home/git
git init --bare blog.git

#回到hexo目录
git init
git add .
git commit -m 'initial commit'
#这里的gitserver就是你的git服务器的ip或者域名
git remote add origin git@gitserver:~/blog.git
git push -u origin master
```

然后就是Git Hook的配置, 见[前面](#Git-Hook)
别忘了 chmod +x post-update
若想使用git-shell: `usermod -s /usr/bin/git-shell git`

### Nginx配置

```bash
#在服务器选择一个放置网站的目录, 假设这个目录为/var/www/blog
mkdir -p /var/www/blog
#设置权限
chown git:git -R /var/www/blog
```

Nginx配置文件见: {% post_link nginx "nginx配置改动" %}

### 安装主题 - NexT

这里选用NexT, 是因为它的浮动TOC在查阅较长的文章时能提供很大的帮助

先贴上[Getting-Started链接](https://theme-next.org/docs/getting-started/#Configuring-Menu-Items)

下载主题文件

```bash
$ cd hexo
# git clone https://github.com/theme-next/hexo-theme-next themes/next -b v7.7.1
# 这里使用git-subrepo, 方便以后pull操作(不用submodule是因为不够好用)
git subrepo clone --branch=v7.7.1 https://github.com/theme-next/hexo-theme-next themes/next
```

## Hexo 文章加密

```bash
cd blog
npm install hexo-blog-encrypt

nano /Hexo/_config.yml  添加如下内容

# Security
## 文章加密 hexo-blog-encrypt
encrypt:
    enable: true

然后在想加密的文章头部添加上对应字段, 如

---
title: hello world
date: 2016-03-30 21:18:02
tags:
password: 12345   （密码）
abstract: Welcome to my blog, enter password to read.
message: Welcome to my blog, enter password to read.
---

password: 是该博客加密使用的密码
abstract: 是该博客的摘要, 会显示在博客的列表页
message: 这个是博客查看时, 密码输入框上面的描述性文字
```

另见[Github项目说明](https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md)

## LaTex支持

在需要LaTex表达式的文章Front-matter中加上 `mathjax: true`

本人还没实装, 请见 [NexT数学表达式](https://theme-next.org/docs/third-party-services/math-equations)
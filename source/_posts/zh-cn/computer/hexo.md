---
title: hexo
category: computer
date: 2020-02-08 12:24:49
tags:
---

# Hexo 配置改动及常用命令

## 常用命令

创建新文章: `$ hexo new [layout] <title> --lang en --category <你最好填个分类>`

清除缓存(在hexo g之前做一次): `$ hexo clean`

网站发布: `$ hexo generate`

## 一些概念

### 布局(Layout)

Hexo 默认有三种布局: `post`, `page`, `draft`, 分别对应了文章的生成路径source/_posts/, source/, source/_drafts/

可在scaffolds文件夹内增加新的布局模板

### Front-matter

Front-matter是文件最上方以 `---` 分隔的区域, 用于指定个别文件的变量, 举例来说

```markdown
---
title: Hello World
date: 2013/7/13 20:46:25
---
```

> 注意: 一般Front-matter使用的yaml语法, yaml语法需要注意空格, 如title: Hello World冒号需要有一个空格, 当然除YAML 外, 你也可以使用 JSON 来编写 Front-matter.

### 分类和标签

只有文章支持分流和标签, 您可以在 Front-matter 中设置. 在其他系统中, 分类和标签听起来很接近, 但是在 Hexo 中两者有着明显的差别: **分类具有顺序性和层次性而标签没有顺序和层次**

```markdown
categories:
- Diary
tags:
- PS3
- Games
```

### 文章摘要

设置文章摘要, 我们只需在想显示为摘要的内容之后添 <!-- more --> 即可. 像下面这样:

```
---
title: hello hexo markdown
date: 2016-11-16 18:11:25
tags:
- hello
- hexo
- markdown
---

我是短小精悍的文章摘要(๑•̀ㅂ•́) ✧

<!-- more -->

紧接着文章摘要的正文内容
```

### 资源引用

写个博客, 有时候会想添加个图片或者其他形式的资源等等. 有以下两种方式进行解决:

使用绝对路径引用资源, 在 Web 世界中就是资源的 URL
使用相对路径引用资源
对于使用相对路径引用资源的, 我们可以使用 Hexo 提供的资源文件夹功能.

使用文本编辑器打开站点根目录下的 _ config.yml 文件, 将 post_asset_folder 值设置为 true.

    post_asset_folder: true

修改之后会开启 Hexo 的文章资源文件管理功能. Hexo 将会在我们每一次通过 `hexo new <title>` 命令创建新文章时自动创建一个同名文件夹, 于是我们便可以将文章所引用的相关资源放到这个同名文件夹下, 然后通过相对路径引用.例如, 你把一个 example.jpg 图片放在了这个同名文件夹中，使用相对路径的常规 markdown 语法 `![](./example.jpg)`即可访问.

## 配置改动

### _config.yml

```yml
# 标签页标题
title: Sudaku's blog
description: 'There is nothing to say now'
author: Sudaku
# 多语言支持
language:
- zh-cn
- en
# 时区
timezone: 'Asia/Shanghai'

# Url
url: http://blog.sudaku233.com
# 文章永久链接(目录)
permalink: :lang/:category/:year/:title/
# 永久链接变量默认值配置
permalink_defaults:
  lang: en

# Writing
new_post_name: :lang/:category/:title.md
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
useradd -m -s /usr/bin/git-shell git
#给git用户设置密码,或者在git用户的.ssh目录的authorized_keys文件里面添加自己的公钥
passwd git
#切换到git用户
su git
cd /home/git
git init --bare hexo.git

#回到hexo目录
git init
git add .
git commit -m 'initial commit'
#这里的gitserver就是你的git服务器的ip或者域名
git remote add origin git@gitserver:~/blog.git
git push -u origin master
```

### 配置 Git hook

使用post-update以在收到commit后自动进行`hexo g`

将`/home/git/blog.git/hooks/post-update`下直接新建post-update:

```
#!bin/bash
# 首先删除旧文件
# 启用extglob才能用!(xx)之类的语法
shopt -s extglob
rm -rf /var/www/blog/!(node_modules)

# git checkout
git --work-tree=/var/www/blog --git-dir=/home/git/blog.git checkout -f


# 安装依赖, 生成静态网页
cd /var/www/blog
if [ ! -d /var/www/blog/node_modules ]; then npm install; fi
hexo g
```

别忘了 chmod +x post-update

*注意*: 后面安装主题后hook有改动

### Nginx配置

```bash
#在服务器选择一个放置网站的目录, 假设这个目录为/var/www/blog
mkdir -p /var/www/blog
#设置权限
chown git:git -R /var/www/blog
```

// TODO: 将记录在单独的Nginx改动文档nginx.md

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

## LaTex支持

本人还没实装, 请见 [LaTex支持](https://skywalkeratlas.github.io/2019/02/04/blog-hexo-git-nginx/#LaTex%E6%94%AF%E6%8C%81)

## 安装主题 - NexT

这里选用NexT, 是因为它的浮动TOC在查阅较长的文章时能提供很大的帮助

先贴上[Getting-Started链接](https://theme-next.org/docs/getting-started/#Configuring-Menu-Items)

### 下载主题文件

```
$ cd hexo
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```

由于我使用git管理整个博客文件目录, 还要设置一下git的submodule

```bash
$ git submodule add https://github.com/theme-next/hexo-theme-next themes/next
$ git submodule update --init #将.gitmodules的内容纳入.git/config文件, 并更新子模块仓库
```

#### GIT HOOK注意

**由于Bare仓库不能checkout submodule**只能这样的方式

1. git hook修改为

```bash blog.git/hooks/post-update
shopt -s extglob
#rm -rf /var/www/blog/!(node_modules)
#git --work-tree=/var/www/blog --git-dir=/home/git/blog.git checkout -f
cd /var/www/blog
git pull
git submodule update --init

if [ ! -d /var/www/blog/node_modules ]; then npm install; fi
hexo cl # 清理上一次文件
hexo g
```

2. 在/var/www/blog clone一个可以进行submodule操作的普通仓库

```bash
$ cd /var/www/blog
$ git clone git@gitserver:~/blog.git
$ git submodule update --init
```

### 启动和配置主题


#### HEXO配置

open `site config file`, find `theme` section, and change its value to `next`.

```yml hexo/_config.yml
theme: next
```

#### NexT主题配置

```yml hexo/theme/next/_config.yml
toc:
  ...
  expand_all: true
```

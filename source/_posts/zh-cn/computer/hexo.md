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

# 部署到 Github Pages
deploy:
  type: git
  repo: git@github.com:ndoskrnl/ndoskrnl.github.io.git
  branch: master
```

### Icarus 主题设置

主题设置由于改动较多且时效性不高, 这里不作记录
具体请见主题配置文件 `theme/icarus/_config.yml`

将主页样式变为归档页形式: 直接将`theme/icarus/layout/index.jsx` 替换成 `theme/icarus/layout/archive.jsx` 内的内容

### Git Hook

```bash /home/git/blog.git/hooks/post-update
#!/bin/bash
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
hexo-tag-mplayer # 音乐播放器
hexo-filter-plantuml # UML Diagram for hexo
hexo-deployer-git # Git 部署 (Github Pages)
```

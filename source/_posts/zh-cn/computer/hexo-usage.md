---
title: hexo-usage
category: computer
date: 2020-02-08 17:51:36
tags:
toc: true
---

Hexo 的一些命令和概念速记

<!-- more -->

## 部署Git仓库

```console
git@blog ~$ git init --bare blog.git
```

然后就是Git Hook的配置, 见[前面](#Git-Hook)
别忘了 chmod +x post-update
若想使用git-shell: `usermod -s /usr/bin/git-shell git`

## Nginx配置

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

## 常用命令

创建新文章: `$ hexo new [layout] <title> --lang en --category <你最好填个分类>`

清除缓存(在hexo g之前做一次): `$ hexo clean`

网站发布: `$ hexo generate`

升级Hexo:

```console
# npm update -g # 升级npm全局插件
# npm install npm-check npm-upgrade -g # 针对package.json的操作, 前者只是检查有更新的插件, 后者应用改动
$ cd /path/to/your/blog
$ npm-upgrade # 更新package.json中的插件版本
$ npm update # 实际更新插件
```

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
toc: true
- PS3
- Games
```

### 资源引用

设置 `post_asset_folder: true` 之后会开启 Hexo 的文章资源文件管理功能. Hexo 将会在我们每一次通过 `hexo new <title>` 命令创建新文章时自动创建一个同名文件夹, 于是我们便可以将文章所引用的相关资源放到这个同名文件夹下, 然后通过相对路径引用.

建议使用相对路径引用的Tag插件进行文章中资源引用

```markdown
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```

Example:

```markdown
{% asset_link unbound.conf.example "/etc/unbound/unbound.conf" %}
```

具体见: [标签插件](https://hexo.io/zh-cn/docs/tag-plugins.html)

#### UML Diagram

```markdown
{% plantuml %}
@startuml
Object <|-- ArrayList
Object : equals()
ArrayList : Object[] elementData
ArrayList : size()
@enduml
{% endplantuml %}
```

{% plantuml %}
@startuml
Object <|-- ArrayList
Object : equals()
ArrayList : Object[] elementData
ArrayList : size()
@enduml
{% endplantuml %}
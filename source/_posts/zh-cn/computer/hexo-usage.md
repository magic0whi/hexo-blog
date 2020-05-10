---
title: hexo-usage
category: computer
date: 2020-02-08 17:51:36
tags:
---

Hexo 的一些命令和概念速记

<!-- more -->

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
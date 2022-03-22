---
title: Some Hexo usages
category: computer
date: 2020-02-08 17:51:36
tags:
toc: true
variant: cyberpunk
article:
    highlight:
        theme: qtcreator_dark
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
abstract: 该博文的摘要
message: 密码输入框的描述性文字
---
```

另见[Github项目说明](https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md)

## 升级Hexo

```console
# npm install npm-check-updates # 或者 npm-check, npm-upgrade
$ cd /path/to/your/blog
$ ncu # 更新 package.json
$ npm update # 更新插件
```

## 分类和标签

只有文章支持分类和标签, 您可以在 Front-matter 中设置. 在其他系统中, 分类和标签听起来很接近, 但是在 Hexo 中两者有着明显的差别: **分类具有顺序性和层次性而标签没有顺序和层次**

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

```
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```

Example:

```
{% asset_link unbound.conf.example "/etc/unbound/unbound.conf" %}
```

具体见: [标签插件](https://hexo.io/zh-cn/docs/tag-plugins.html)

## 一些技巧示例

1. 单个页面使用赛博朋克主题, 在 Front-matter 加入如下内容:
   ```yaml
   ---
   variant: cyberpunk
   article:
       highlight:
           theme: qtcreator_dark
   ---
   ```
2. 一些 Bulma 的提示信息(来自[Icarus主题](https://raw.githubusercontent.com/ppoffice/hexo-theme-icarus/site/source/_posts/en/Getting-Started.md))
   ```html
   <article class="message message-immersive is-primary">
   <div class="message-body">
   <i class="fas fa-globe-asia mr-2"></i>这是一个页面提示
   <a href="{% post_path zh-CN/Getting-Started %}">超链接</a>.
   </div>
   </article>

   <article class="message message-immersive is-warning">
   <div class="message-body">
   <i class="fas fa-question-circle mr-2"></i>Something wrong with this article? Click <a href="https://github.com/ppoffice/hexo-theme-icarus/edit/site/source/_posts/en/Getting-Started.md">here</a> to submit your revision.
   </div>
   </article>

   <article class="message is-primary" style="font-size:inherit">
   <div class="message-body">
   这是一条讯息.
   </div>
   </article>
   ```
   如下:
<article class="message message-immersive is-primary">
<div class="message-body">
<i class="fas fa-globe-asia mr-2"></i>这是一个页面提示
<a href="{% post_path zh-cn\computer\hexo-usage %}">超链接</a>.
</div>
</article>

<article class="message message-immersive is-warning">
<div class="message-body">
<i class="fas fa-question-circle mr-2"></i>Something wrong with this article? 
Click <a href="https://github.com/ppoffice/hexo-theme-icarus/edit/site/source/_posts/en/Getting-Started.md">here</a> 
to submit your revision.
</div>
</article>

<article class="message is-primary" style="font-size:inherit">
<div class="message-body">
这是一条讯息.
</div>
</article>

3. Bulma 的带内容标签示范
   ```html
   <div class="tabs is-boxed my-3">
     <ul class="mx-0 my-0">
       <li class="is-active">
         <a href="#tab1">
           <span class="icon is-small"><i class="fas fa-file-code" aria-hidden="true"></i></span>
           <span>标签1</span>
         </a>
       </li>
       <li>
         <a href="#tab2">
           <span class="icon is-small"><i class="fas fa-cubes" aria-hidden="true"></i></span>
           <span>标签2</span>
         </a>
       </li>
     </ul>
   </div>
   
   <div id="tab1" class="tab-content">
     foooooooooo!
   </div>
   
   <div id="tab2" class="tab-content is-hidden">
     woooooooooo!
   </div>
   
   <!-- 最好加条横线以和下文内容区分 -->
   <hr>
   
   <!-- 在页面底部添加 JS 脚本 -->
   <script>
   window.addEventListener('DOMContentLoaded', () => {
       Array
           .from(document.querySelectorAll('.tabs li'))
           .forEach((tab) => {
               tab.addEventListener('click', (e) => {
                   e.preventDefault();
                   const currentTab = document.querySelector('.tabs li.is-active');
                   currentTab.classList.remove('is-active');
                   document
                       .getElementById(currentTab.querySelector('a').getAttribute('href').substring(1))
                       .classList.add('is-hidden');
                   
                   const newTab = e.currentTarget;
                   newTab.classList.add('is-active');
                   document
                       .getElementById(e.currentTarget.querySelector('a').getAttribute('href').substring(1))
                       .classList.remove('is-hidden');
                   
               });
           });
   });
   </script>
   ```
   <div class="tabs is-boxed my-3">
     <ul class="mx-0 my-0">
       <li class="is-active">
         <a href="#tab1">
           <span class="icon is-small"><i class="fas fa-file-code" aria-hidden="true"></i></span>
           <span>标签1</span>
         </a>
       </li>
       <li>
         <a href="#tab2">
           <span class="icon is-small"><i class="fas fa-cubes" aria-hidden="true"></i></span>
           <span>标签2</span>
         </a>
       </li>
     </ul>
   </div>
   
   <div id="tab1" class="tab-content">
     foooooooooo!
   </div>
   
   <div id="tab2" class="tab-content is-hidden">
     woooooooooo!
   </div>


1. 使用内联 CSS 管理行首缩进. (虽然我更喜欢用在段落前加 `&emsp\;` 这种方式)
   ```html
   <span class=sentence>凌晨 1 时, 大多数人已睡了三小时, 进入易醒的浅睡阶段, 对疼痛特别敏感.</span>
   
   <style type="text/css" rel="stylesheet">
   .sentence {
         /* padding:.3em .5em .1em 2em; */
         padding-left: 2em;
       	background: pink;
       }
   </style>
   ```

## KaTeX

1. div 标签中 DisplayMode (用 "\\\\[,\\\\]" 包裹) 的 KaTeX 会莫名地有条滚动条, 通过内联 css 隐藏它:
   ```css
   <style type="text/css" rel="stylesheet">
   .katex-html {
     overflow-x: hidden;
   }
   </style>
   ```
2. 通过内联 JS 脚本实现 KaTex 参数自定义:
   ```html
   <div id="katex-1">
   </div>
   <script>
   window.onload = () => {
       var html = katex.renderToString(`
       \\begin{equation}
       \\begin{split}
         a &=b+c \\\\
         &=e+f
       \\end{split}
       \\end{equation} \\\\
       \\htmlStyle{color: red;}{x} \\\\
       \\href{https://katex.org/}{\KaTeX} \\\\
       \\includegraphics[height=0.8em, totalheight=0.9em, width=0.9em, alt=KA logo]{https://katex.org/img/khan-academy.png}`
       , {
       displayMode: true,
       trust: (context) => ['\\includegraphics', '\\href', '\\htmlStyle'].includes(context.command)
       });
       document.getElementById('katex-1').insertAdjacentHTML(
       'afterend',
       html
       );
   };
   </script>
   ```
3. 一些踩坑:
   Hexo 中的内嵌 \\(\KaTeX\\) 如果出现这样的字符 `(R)` 会被识别成商标 \((R)\), 需转义括号: `\(R\)`
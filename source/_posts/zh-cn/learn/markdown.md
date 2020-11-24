---
title: markdown
category: learn
date: 2020-02-08 16:45:30
tags:
toc: true
mathjax: true
---

旨在统一文档的符号、Katex 书写格式

<!-- more -->

## 规范

1. 正文
   * 使用顿号 "、" 来表示同级. **不需要空格**(因为是全角符号)
   * 下一行要留空:
     * html表格标签结尾
     * 图片资源标签 `{%img %}`
     * Markdown 子列表(若接下来还有文本)
   * 内容尽量用数字列表(`1. 2. 3.`)包装(比如本文)
   * 若定义需要多行来解释, 第一行留空, 如:
     ```
     1. 定义内容
        xxx
        xxxx
        xxxxx
     ```
2. 标题避免使用 KaTeX 式子(因为TOC无法解析)
3. LaTeX
   * 所有数学式子、符号必须用 LaTeX 修饰
   * KaTeX式子两边要留一个空格间距
   * 尽量不在 KaTeX 式子中使用逗号; 两个 Katex 式子之间的逗号 "," 留一个空格间距
   * 式子后面括号注释(如 \\(y=x \quad(x\geqslant 0)\\) 使用的空格间距统一为 `\quad`
   * 为了满足使源文档中 KaTeX 尽量地短(比如等号 `=` 周围**不**留空格)

## 提示

1. 下划线 `<u></u>`
2. KaTeX 使用自定义的空格长度可用 `\mskip{1em}`
3. Katex 中 省略号 "..." 可以用 `\dots` 表示

## Markdown 语法

### 标题
```
# 最大标题
## 第二大标题
###### 最小标题
```

### 样式文本
| 样式 | 语法 | 键盘快捷键 | 示例 | 输出 |
| --- | --- | --------- | --- | --- |
| 粗体 | `** **` 或 `__ __` | 命令/控制键 + b | `**这是粗体文本**` | **这是粗体文本** |
| 斜体 | `* *` 或 `_ _` | 命令/控制键 + i | `*这是斜体文本*` | *这是斜体文本* |
| 删除线 | `~~ ~~` | | `~~这是错误文本~~` | ~~这是错误文本~~ |
| 粗体和嵌入的斜体 | `** **` 和 `_ _` | | `**此文本 _非常_ 重要**` | **此文本 _非常_ 重要** |
| 全部粗体和斜体 | `*** ***` | | `***所有这些文本都很重要***` | ***所有这些文本都是斜体***

### 引用文本

您可以使用`>`来引用文本。

```
用 Abraham Lincoln 的话来说：

> 原谅我爆粗口
```

### 表格
```
| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```

有用的链接：

[Github-MD基础书写和格式](https://help.github.com/cn/github/writing-on-github/basic-writing-and-formatting-syntax)

[Github-使用表格组织化信息](https://help.github.com/cn/github/writing-on-github/organizing-information-with-tables)

### 锚点

```markdown
<!-- 定义锚点 --> 
<span id="const_method"></span>

<!-- 锚点链接 --> 
[const method](#const_method)
```
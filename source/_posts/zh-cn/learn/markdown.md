---
title: markdown
category: learn
date: 2020-02-08 16:45:30
tags:
---

Markdown语法记录

Github格式的Markdown
===================

<!-- more -->

TOC
===
<!--还需要填坑-->

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
[#const method](#const_method)
```

## katex语法

详见[Katex支持语法](https://katex.org/docs/supported.html)

使用左花括号(opening curly brackets)和右花括号(closing curly brackets)来包含多个字符: 如`e^{2 \pi i \xi x}`

空格Space: `\,`

除数`\frac a b`, $\frac a b$

上标Superscript: 如`x^3`, $x^3$

下标Subscript: 如`x_1`, $x_1$

包含: 如`x\in R`, $x\in R$

无限infinty符号: `\infty`, $\infty$

函数上面的小帽子: 如`\hat f`, $\hat f$

积分(Integral)符号 `\int`, $\int$


### 希腊字母

ξ: `\xi`

π: `\pi`
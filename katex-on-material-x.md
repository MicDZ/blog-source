---
title: 在Material-X上使用KaTeX
categories:
  - Solutions
date: 2018-12-15 19:24:28
tags: [katex,material-x]
---



> Upd2019.10.19：以适配新版本的hexo-renderer-markdown-it-plus
>
> Upd2019.12.11：一些复杂公式的渲染问题可以在[katex.org](https://katex.org/)上进行测试，如果[katex.org](https://katex.org/)上无法正常渲染且您的TeX源码无语法错误的话，请与 $\KaTeX$ 的开发者联系。

随着Material-X的使用用户越来越多、使用用户的范围越来越广。不少用户被如何在Material-X上渲染公式所困。xaoxuu给出的方案是完美的，你只需要在使用最新的版本下将文章开头加入一行`mathjax: true`即可，十分简洁，可以渲染绝大部分简单的公式。

*注：本文部分素材来自网络，相关插件来源github。*

<!--more-->

## 存在的问题

至于有一些复杂的公式为什么渲染不了，大概是有几种 $\LaTeX$ 的符号与markdown冲突，在渲染时就会转义错误。错误通常会直接导致无法生成。

## 操作原理

具体的解决方案是卸载掉原来的渲染器，安装一个针对 $\KaTeX$ 优化过的渲染器。

这个渲染器包括了 $\KaTeX$ 。

## 具体操作步骤

卸载原有的渲染器，并安装大神的渲染器（基本稳定可靠）

```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-markdown-it-plus --save
```

在根目录的配置文件`_config.yml`加入
> 此为历史版本
```
  plugins:
    - plugin:
        name: markdown-it-katex
        enable: true
    - plugin:
        name: markdown-it-mark
        enable: false
```

> 此为更新后版本

```
markdown_it_plus:
    highlight: true
    html: true
    xhtmlOut: true
    breaks: true
    langPrefix:
    linkify: true
    typographer:
    quotes: “”‘’
    plugins:
        - plugin:
            name: markdown-it-mark
            enable: false
```

至此，你在Material-X下就可以成功使用 $\KaTeX$ 渲染公式了。

如果失败，尝试继续进行以下操作：

找到`/themes/material-x/layout/_partial/head.ejs`

在该文件最后加入一行

> 此为历史版本

```ejs
<link href="https://cdn.bootcss.com/KaTeX/0.7.1/katex.min.css" rel="stylesheet">
```

> 此为更新后版本

```ejs
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.css" integrity="sha384-zB1R0rpPzHqg7Kpt0Aljp8JPLqbXI3bhnPWROx27a9N0Ll6ZP/+DiW/UqRcLbRjq" crossorigin="anonymous">

    <!-- The loading of KaTeX is deferred to speed up page rendering -->
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.js" integrity="sha384-y23I5Q6l+B6vatafAwxRu/0oK/79VlbSz7Q9aiSZUvyWYIYsd+qj+o24G5ZU2zJz" crossorigin="anonymous"></script>

    <!-- To automatically render math in text elements, include the auto-render extension: -->
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous"
        onload="renderMathInElement(document.body);"></script>
```

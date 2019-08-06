---
title: 在material-x上使用KaTeX
categories:
  - Solutions
date: 2018-12-15 19:24:28
tags: [katex,material-x]
---



> 本文随着KaTeX的更新在windows平台已经不再适用，请windows用户结合其他资料参考本文

随着Material-x的使用用户越来越多、使用用户的范围越来越广。不少用户被如何在Material-x上渲染公式所困。xaoxuu给出的方案是完美的，你只需要在使用最新的版本下将文章开头加入一行`mathjax: true`即可，十分简洁，可以渲染绝大部分简单的公式。

*注：本文部分素材来自网络，相关插件来源github。*

<!--more-->

# mathjax存在的问题

至于有一些复杂的公式为什么渲染不了，大概是有几种LaTeX的符号与markdown冲突，在渲染时就会转义错误。错误通常会直接导致无法生成。

# 操作原理

具体的解决方案是卸载掉原来的渲染器，安装一个针对KeTeX优化过的渲染器。

这个渲染器包括了KaTeX

# 具体操作步骤

卸载原有的渲染器，并安装大神的渲染器（基本稳定可靠）

```
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-markdown-it-plus --save
```

在根目录的配置文件`_config.yml`加入

```
  plugins:
    - plugin:
        name: markdown-it-katex
        enable: true
    - plugin:
        name: markdown-it-mark
        enable: false
```

至此，你在material-x下就可以成功使用KaTeX渲染公式了。

如果失败，尝试继续进行以下操作：

找到`/themes/material-x/layout/_partial/head.ejs`

在该文件最后加入一行

```ejs
<link href="https://cdn.bootcss.com/KaTeX/0.7.1/katex.min.css" rel="stylesheet">
```

如果仍旧失败，可以尝试自己到谷歌上搜索`KaTeX hexo`，可以得到许多你想要的结果。
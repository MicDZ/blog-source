---
title: '【SDOI2009】SuperGCD 题解'
date: 2018-06-08 22:58:43
tags: [题解,数学,高精度]
categories:
- OI   
---



最“快”的一道紫题。

> 这是一篇原始文章，不保证内容的正确性

<!--more-->

[题目链接](https://www.lydsy.com/JudgeOnline/problem.php?id=1876)

辗转相除法并不会RE，python水过，好像py3自带了GCD函数（我不会写）。

```python
a=int(input()) 
b=int(input()) 
while b!=0: 
    t=a 
    a=b 
    b=t%b 
print(a)
```

最慢的点164ms可以接受

noi不支持python，这是多么遗憾的一件事情。

中国还没有在OI圈普及，俄罗斯美国已经相当发达了。



就这么*掉了一道紫题，有点不好意思呢。
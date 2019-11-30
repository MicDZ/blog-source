---
title: 【NOIP2012】国王游戏 题解
categories:
  - OI
date: 2019-10-29 12:48:04
tags: [题解,搜索]
---

[NOIP2012](https://www.luogu.org/problem/P1080)

<!--more-->

# 核心思路

很经典的贪心了。这道题从17年的附中集训第一次看到，到现在才写下总结。

证明很多大佬有非常高级的方法。这里用邻项交换法。
$$
\begin{aligned}
&s\\ 
&a_i\ \  b_i\\
&a_{i+1}\ \ b_{i+1}
\end{aligned}
$$
$$
\begin{aligned}
&s\\ 
&a_{i+1}\ \  b_{i+1}\\
&a_i\ \ b_i
\end{aligned}
$$

考虑上面的两种情况，假设第二种比第一种更优。
$$
S_1=\frac{s}{b_i}+\frac{s+a_i}{b_{i+1}}\\
S_2=\frac{s}{b_{i+1}}+\frac{s+a_{i+1}}{b_i}
$$
令 $S_2<S_1$
$$
sb_{i+1}+sb_i+a_ib_i+b_i>sb_i+sb_{i+1}+a_ib_{i+1}\\
a_ib_i>a_{i+1}b_{i+1}
$$
所以按照这个排序就是最终的顺序了。

这道题要用到高精度和一定的常数优化，这里就不给出代码了。
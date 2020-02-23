---
title: '【UVA11538】Chess Queen题解'
date: 2019-07-23 22:06:30
tags: [题解,数学]
categories:
- [OI,题解]   
---

此题有$\Theta(T)$的做法，感谢lrc大佬讲解

<!--more-->

已经得到了
$$
ans_1=C_m^1A_n^2=nm(n-1)
$$

$$
ans_2=C_n^1A_m^2=nm(m-1)
$$
$ans_1$与$ans_2$为在同一行与同一列互相攻击的皇后数

$ans_3$为在同一斜列的互相攻击皇后数
显然
$$
ans_3=2\times[A_2^2+A_3^2+...+A_n^2\times(m-n+1)+...+A_3^2+A_2^2]
$$

如何化简？将此式展开
$$
ans_3=[1\times2+2\times3+...+(n-1)\times n]\times4+2n(n-1)(m-n)
$$

前半部分就像小学奥数中的通项求和问题了


$$
ans_3=\sum_{i=1}^{n-1}{i(i+1)}\times 4+2n(n-1)(m-n)
$$

$$
=\sum_{i=1}^{n-1}{i^2} \times 4+ \sum_{i=1}^{n-1} \times 4+2n(n-1)(m-n)
$$

由$\sum_{i=1}^ni^2=\frac{n(n+1)(2n+1)}{6}$得

$$
ans_3=[\frac{n(n-1)(2n-1)}{6}+\frac{n(n-1)}{2}]\times4+2n(n-1)(m-n)
$$
$$
=\frac {2n(n-1)(2n-1)}{3}+2n(n-1)(m-n)
$$

最后再把$ans_1$，$ans_2$，$ans_3$加起来即可



Ps：此题不能通过分析边角与非边角位置的攻击数来统计答案，这样会使解答更为复杂。
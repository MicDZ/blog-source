---
title: OI中的和式
date: 2018-04-22 18:23:11
tags: [数学,笔记,随笔]
categories:
- OI   
---

一些基础的数学知识可以为我们提供非常好的思路，这里主要介绍的是$$\sum $$求和符号。

<!--more-->

# 定义

求和是数学中常见又基本的操作, 我们需要为它找一个记号。假设现在有一个数列$\{ a_1,a_2,...,s_n\}$我们需要有一种灵活又不复杂的记号, 来表示它们的和。

>$\sum\limits^n_{i=1}a_i​$
>
>表示数列数列$\{a\}$的和.



这是种记号非常类似for循环的前两个语句。(记号省略了第三个语句$i++$)

# 定律

就像所有运算符号一样，和式也有类似的规则：

$\sum\limits_{k\in K}(a_k+b_k)=\sum\limits_{k\in K}a_k+\sum\limits_{k\in K}b_k$

证明的方法是加法交换律。

$\sum\limits_{k\in K}ca_k=c\sum\limits_{k\in K}a_k$

证明的方法是乘法分配律。

$\sum\limits_{i\in A}a_i\sum\limits_{j\in B}j_j=\sum\limits_{i\in A}\sum\limits_{j\in B}a_ij_j$

证明的方法是加法结合律，交换也是满足的。



# 问题

> Description:
>
> 求$S_n=\sum\limits^n_{i=0}x^i$



利用扰动法解此题：

$$
\begin{aligned}
S_n+x^{n+1}&=x^0+\sum\limits^{n+1}_{i=1}x^i\\
&=x^0+\sum\limits^n_{i=0}x^{i+1}\\
&=x^0+x\sum\limits^n_{i=0}x^i\\
&=x^0+xS_n\\
\end{aligned}
$$

解得

$$
\begin{aligned}
S_n&=\frac{1-x^{n+1}}{1-x},x\neq 1\\
&=n+1,x= 1
\end{aligned}
$$




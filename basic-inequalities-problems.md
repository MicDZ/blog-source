---
title: 基础不等式
categories:
  - Math
date: 2018-12-30 16:34:00
tags: [不等式]
---

这里将会持续添加一些基础的不等式定理、题目。

<!--more-->

# Basic Inequalities

> Theorem1. 1
>
>  $xy\geq2\sqrt{xy}$. $x,y\in \R ^+$

*Proof*
$$
(x-y)^2\geq0\\
x^2-2xy+y^2\geq0\\
x^2+2xy+y^2\geq4xy\\
(x+y)^2\geq4xy\\
x+y\geq2\sqrt{xy}\\
\Leftrightarrow xy\leq\frac{x+y}{2}\\
Equalitiy\ \ occurs\ \ if\ \ and\ \ only\ \ if\ \ x=y.
$$

> Exercise1. 1
>
> Use *Theorem1. 1* to solve this problem.
>
> Let $0<x<4$ . Prove the inequality: 
> 
>$x(8-2x)\leq8$ .

*Solution*
$$
2x+(8-2x)=8\\
x(8-2x)=\frac{1}{2}[2x(8-2x)]\leq\frac{1}{2}(\frac{2x-8-2x}{2})^2=8
$$

> Exercise1.2
>
> Let $n\in R$ . Prove the inequality:
>
> $1+\frac{1}{2^2}+\frac{1}{3^2}+...+\frac{1}{n^2}\leq2$ .

*Solution*

It's easy to know:
$$
\frac{1}{a\times a}\leq\frac{1}{a\times(a-1)}.\\
$$
So
$$
1+\frac{1}{2^2}+\frac{1}{3^2}+...+\frac{1}{n^2}<1+\frac{1}{1\times2}+\frac{1}{2\times3}+...+\frac{1}{(n-1)\times n}\\
=1+\frac1 1 -\frac1 2 +\frac1 2-\frac1 3+...+\frac{1}{n-1}-\frac{1}{n}=2-\frac{1}{n}<2
$$

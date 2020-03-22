---
title: 莫比乌斯反演学习笔记
categories: [OI,学习笔记]
date: 2020-03-21 14:49:53
tags: [数学,学习笔记]
edit: true
password: f***kmobius
abstract: 这是一篇被加密的文章，博主不会提供密码
message: 请输入文章访问密码
wrong_pass_message: 您输入的密码有误，请重试
wrong_hash_message: 您输入的密码无法验证，您可以查看以此密码解密的文本
---

<!--more-->

## 前置知识

$$
\forall a,b,c\in\mathbb{Z},\left\lfloor\frac{a}{bc}\right\rfloor=\left\lfloor\frac{\left\lfloor\frac{a}{b}\right\rfloor}{c}\right\rfloor
$$

$$
\forall n \in N,  \left|\left\{ \lfloor \frac{n}{d} \rfloor \mid d \in N \right\}\right| \leq \lfloor 2\sqrt{n} \rfloor
$$

## 数论分块

对于任意一个 $i(i\leq n)$ ，我们需要找到一个最大的 $j(i\leq j\leq n)$ ，使得 $\left\lfloor\frac{n}{i}\right\rfloor = \left\lfloor\frac{n}{j}\right\rfloor$ 。

$$
j=\left\lfloor\frac{n}{\left\lfloor\frac{n}{i}\right\rfloor}\right\rfloor
$$

## 积性函数

若 $\gcd(x,y)=1$ （ $x\perp y$ ） 且 $f(xy)=f(x)f(y)$ ，则 $f(n)$ 为积性函数。

### 性质

若 $f(x)$ 和 $g(x)$ 均为积性函数，则以下函数也为积性函数：

$$
\begin{aligned}
h(x)&=f(x^p)\\
h(x)&=f^p(x)\\
h(x)&=f(x)g(x)\\
h(x)&=\sum_{d\mid x}f(d)g(\frac{x}{d})
\end{aligned}
$$

### 例子

-   单位函数： $\epsilon(n)=[n=1]$ 
-   恒等函数： $\operatorname{id}_k(n)=n^k$  $\operatorname{id}_{1}(n)$ 通常简记作 $\operatorname{id}(n)$ 。
-   常数函数： $1(n)=1$ 
-   除数函数： $\sigma_{k}(n)=\sum_{d\mid n}d^{k}$  $\sigma_{0}(n)$ 通常简记作 $\operatorname{d}(n)$ 或 $\tau(n)$ ， $\sigma_{1}(n)$ 通常简记作 $\sigma(n)$ 。
-   欧拉函数： $\varphi(n)=\sum_{i=1}^n [\gcd(i,n)=1]$ 
-   莫比乌斯函数： $\mu(n) = \begin{cases}1 & n=1 \\ 0 & \exists d:d^{2} \mid n \\ (-1)^{\omega(n)} & otherwise\end{cases}$ 其中 $\omega(n)$ 表示 $n$ 的本质不同质因子个数，是一个加性函数。
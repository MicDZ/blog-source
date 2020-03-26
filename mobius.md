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



## Dirichlet 卷积

### 定义

定义两个数论函数 $\textbf f,\textbf g$ 的 Dirichlet 卷积为

$$
(\textbf f\ast \textbf g)(n)=\sum_{d\mid n}\textbf f(d)\textbf g(\frac{n}{d})
$$

也可以等价地写为：
$$
(\textbf f*\textbf g)(n)=\sum_{ij=n}\textbf f(i)\textbf g(j)
$$


### 性质

1. **交换律** $\textbf f*\textbf g=\textbf g*\textbf f$
2. **结合律** $(\textbf f*\textbf g)*\textbf h=\textbf f*(\textbf g*\textbf h)$
3. **分配率** $(\textbf f+\textbf g)*\textbf h=\textbf f*\textbf h+\textbf g*\textbf h$
4. **数乘** $(x\textbf f)*\textbf g=x(\textbf f*\textbf g)$
5. **单位元** $\epsilon*\textbf f=\textbf f$
6. **逆元** 对于每个 $\textbf f(1)\not=0$ 的 $\textbf f$  ，都存在一个 $\textbf g$  使得 $\textbf f*\textbf g=\epsilon$

### 函数的逆

对于函数 $\textbf f$ ，要求它的逆 $\textbf g$ 。那么 $\textbf g$ 为：



### 例子

$$
\begin{aligned}
\varepsilon=\mu \ast 1&\iff\varepsilon(n)=\sum_{d\mid n}\mu(d)\\
d=1 \ast 1&\iff d(n)=\sum_{d\mid n}1\\
\sigma=d \ast 1&\iff\sigma(n)=\sum_{d\mid n}d\\
\varphi=\mu \ast \textbf{ID}&\iff\varphi(n)=\sum_{d\mid n}d\cdot\mu(\frac{n}{d})
\end{aligned}
$$

## 积性函数

若 $\gcd(x,y)=1$ （ $x\perp y$ ） 且 $\textbf f(xy)=\textbf f(x)\textbf f(y)$ ，则 $\textbf f(n)$ 为积性函数。

### 性质

若 $\textbf f(x)$ 和 $\textbf g(x)$ 均为积性函数，则以下函数也为积性函数：

$$
\begin{aligned}\textbf h(x)&=\textbf f(x^p)\\\textbf h(x)&=\textbf f ^p(x)\\\textbf h(x)&= \textbf f(x)\textbf g(x)\\\textbf h(x)&=\sum_{d\mid x}\textbf f(d)\textbf g(\frac{x}{d})\end{aligned}
$$

### 例子

-   单位函数： $\epsilon(n)=[n=1]$ 
-   恒等函数： $\textbf{id}_k(n)=n^k$  $\textbf{id}_{1}(n)$ 通常简记作 $\textbf{id}(n)$ 。
-   常数函数： $\textbf 1(n)=\textbf 1$ 
-   除数函数： $\sigma_{k}(n)=\sum_{d\mid n}d^{k}$  $\sigma_{0}(n)$ 通常简记作 $\operatorname{d}(n)$ 或 $\tau(n)$ ， $\sigma_{1}(n)$ 通常简记作 $\sigma(n)$ 。
-   欧拉函数： $\varphi(n)=\sum_{i=1}^n [\gcd(i,n)=1]$ 
-   莫比乌斯函数

### 结论

**两个积性函数的狄利克雷卷积是积性函数**。



## 莫比乌斯函数

### 定义

 $\mu$ 为莫比乌斯函数，定义为

$$
\mu(n)=
\begin{cases}
1&n=1\\
0&n\text{ 含有平方因子}\\
(-1)^k&k\text{ 为 }n\text{ 的本质不同质因子个数}\\
\end{cases}
$$
详细解释一下：

令 $n=\prod_{i=1}^kp_i^{c_i}$ ，其中 $p_i$ 为质因子， $c_i\ge 1$ 。上述定义表示：

1.   $n=1$ 时， $\mu(n)=1$ ；
2.  对于 $n\not= 1$ 时：
    1.  当存在 $i\in [1,k]$ ，使得 $c_i > 1$ 时， $\mu(n)=0$ ，也就是说只要某个质因子出现的次数超过一次， $\mu(n)$ 就等于 $0$ ；
    2.  当任意 $i\in[1,k]$ ，都有 $c_i=1$ 时， $\mu(n)=(-1)^k$ ，也就是说每个质因子都仅仅只出现过一次时，即 $n=\prod_{i=1}^kp_i$ ， $\{p_i\}_{i=1}^k$ 中个元素唯一时， $\mu(n)$ 等于 $-1$ 的 $k$ 次幂，此处 $k$ 指的便是仅仅只出现过一次的质因子的总个数。



反演结论： $\displaystyle [gcd(i,j)=1] \iff\sum_{d\mid\gcd(i,j)}\mu(d)$ 



## 莫比乌斯反演

定义 $\textbf{1}$ 的逆是 $\mu$ 。如果 $\textbf g=\textbf f*\textbf 1$，就有 $\textbf f=\textbf f*\textbf 1*\mu=\textbf g*\mu$ 。

即如果 $\textbf g(n)=\sum_{d|n} \textbf f(d)$，那么 $\textbf f(n)=\sum_{d|n}\mu(\frac n d)\textbf g(d)$ 。 





## 内容来源声明

本文部分内容来自 [OI-wiki](https://oi-wiki.org) ，[洛谷日报](https://www.luogu.com.cn/blog/lx-2003/mobius-inversion) ，侵删。
---
title: 组合容斥计数及反演学习笔记
categories:
  - [OI,学习笔记]
date: 2020-01-19 11:05:46
tags: [学习笔记,数学,组合,容斥]
password: 19260817
abstract: 还没写完，这里面什么都没有
message: 请输入文章访问密码
wrong_pass_message: 您输入的密码有误，请重试
wrong_hash_message: 您输入的密码无法验证，您可以查看以此密码解密的文本
edit: true
---

<!--more-->
## 常用公式
### 二项式定理
$$
(a+b)^n=\sum_{i=0}^n{n\choose{i}}a^ib^{n-i}
$$

### 可重组合
$$
{n+m-1}\choose m
$$


### 二项式反演

$$
\begin{aligned}f_n&=\sum_{i=0}^n(-1)^i{n \choose i}g_i\\g_n&=\sum_{i=0}^n(-1)^i{n\choose i}f_i\end{aligned}
$$

或者把它写成更常见的形式

$$
\begin{aligned}f_n&=\sum_{i=0}^n{n\choose i}g_i\\g_n&=\sum_{i=0}^n(-1)^{n-i}{n\choose i}f_i\end{aligned}
$$

一些二项式反演变形公式，其实就是替换一下范围
$$
f_{k}=\sum_{i=k}^{n}(-1)^i\binom{i}{k}g_i\Rightarrow g_k= \sum_{i=k}^{n}(-1)^i\binom{i}{k}f_i\\
f_{k}=\sum_{i=k}^{n}\binom{i}{k}g_i\Rightarrow g_k=\sum_{i=k}^{n}(-1)^{i-k}\binom{i}{k}f_i
$$

把 $f_i$ 带入 $g_n$ 的式子中可以简单证明（蒯自yyb）。

$$
\begin{aligned}g_n&=\sum_{i=0}^n(-1)^{n-i}{n\choose i}f_i\\
&=\sum_{i=0}^n(-1)^{n-i}{n\choose i}\sum_{j=0}^i{i\choose j} g_j\\
&=\sum_{j=0}^n g_i\sum_{i=j}^n{n\choose i}{i\choose j}(-1)^{n-i}\\
&=\sum_{j=0}^n g_j\sum_{i=j}^n{n\choose j}{n-j\choose i-j}(-1)^{n-i}\\
&=\sum_{j=0}^n g_j[{n\choose j}\sum_{i=j}^n{n-j\choose i-j}(-1)^{n-i}]\\
&=\sum_{j=0}^n g_j[{n\choose j}\sum_{i=0}^{n-j}{n-j\choose i}(-1)^{n-j-i}]\\
&=\sum_{j=0}^n g_j[{n\choose j}(1-1)^{n-j}]\\
&=g_n\end{aligned}
$$


---
title: 简单数论
date: 2018-05-13 22:45:53
tags: [笔记,数学]
categories:
- OI   
---



数论是学习OI的必经之路。

<!--more-->

# Primarily Test

## Normal Sieve

* 对于每一个数，筛去所有倍数，因为它的倍数有它自己这个因子
* 时间复杂度： $n\sum\limits_{i=1}\frac{1}{i}$

## Sieve of Eratosthenes

* *埃拉托色尼筛选法*，伪线性筛算法（ *与线性筛算法呈常数关系*）
* 对于每一个质数，筛去所有它的倍数
* 一个显然的优化是 $i-i^2$ 这一段的数是不用筛的
* 时间复杂度： $n\sum\limits_{p\le n}\frac{1}{p}=O(n\log\log n)$

## Linear Sieve

* 对于每一个数，用小于等于它最小的质因子的所有质数筛
* 每一个数只会被它最小的质因子筛到一次
* 时间复杂度：  $O(n)$

# Multiplicative Function

* 欧拉函数一种重要的积性函数
* $φ(n)$ 表小于等于 $n$ 的数中与 $n$ 互质的数的个数
* 计算方法:
* $\varphi(n)=n\mathop\Pi\limits_{p|n}\frac{p-1}{p}$
* 本质：容斥原理，直接枚举质因数计算

# GCD

* k|a 且k|b , k is a common factor of a and b
* We call the largest k Greatest Common Divisor of a and b
* 若 $b=0$ ，$gcd(a,b)=a$
* $gcd(a,b)=gcd(a-b,b)$
* $\gcd(a,b)=\gcd(b\%a,a)$
* 辗转相除，时间复杂度O(log n)

52：36
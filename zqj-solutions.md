---
title: 中秋节欢乐赛题解
date: 2018-09-23 17:09:16
tags: [题解]
categories:
- [OI,题解]      
---

比赛地址分别为：

[中秋节欢乐赛DIV1](https://www.luogu.org/contest/show?tid=10826)

[中秋节欢乐赛DIV2](https://www.luogu.org/contest/show?tid=10827)

<!--more-->

**所有比赛标程、数据都放置在了[Github ](https://github.com/MicDZ/Code/tree/master/contest/ZQJ)**

## DIV1

### 作业计数

小凡一门学课的作业不做怒气总值增加$b$，那么$a$门作业不做怒气值即为$a\times b$，答案为对$m$取膜的结果。

数据范围为$1\leq a,b,m\leq 10^{18}$，此时你发现$a\times b$会直接炸int。所以需要一些玄学的优化。

~~Ps：很抱歉，这题没给部分分。~~

#### 方法一

使用自带高精度的python解决问题

```cpp
a=int(input())
b=int(input())
m=int(input())
print(a*b%m)
```

#### 方法二

使用高精度的乘法和魔法

在这里推荐各位到[yali_hzy](https://blog.csdn.net/yali_hzy/article/details/81430514)的博客看看。我就不写了。。

高精取膜的方法为：
$$
\begin{aligned}
a-b\times \lfloor\frac{a}{b}\rfloor
\end{aligned}
$$


#### 方法三

用快速乘的方法。

原理类似于卡速米。

```cpp
#define ll long long
ll mul(ll a,ll b,ll m) {
    ll ans=0;
    while(b) {
        if(b&1) b=(b+a)%m;
        a=(a+a)%m;
        b>>=1;
    }
    return ans;
}
```

复杂度为$\Theta(\log b)$

### 月饼采购

这可以算是一个非常经典的题目了，紫书、蓝书上都有。

和作业计数类似的，膜术可以这么算$a-b\times \lfloor\frac{a}{b}\rfloor$

那么就可以得到以下推导
$$
\begin{aligned}
ans&=\sum^{n}_{i=1}k-i\times\lfloor\frac{a}{b}\rfloor\\
&=n\times k-\sum^{n}_{i=1}i\times\lfloor\frac{k}{i}\rfloor
\end{aligned}
$$
然后可以从$\lfloor\frac{k}{i}\rfloor$出发，得到答案。

复杂度为$\Theta{\sqrt{k}}$。

### 分配月饼

$$
\begin{aligned}
C^m_n&=\frac{n!}{m!(n-m)!}\\
&=\frac{\prod^{n}_{i=n-m}}{\prod^{m}_{i=1}}
\end{aligned}
$$

得到了以上的结论后即可做到在$\Theta(2\times m)$，的复杂度内求解。

而原题的$n\leq2\times10^5$，因此可以通过。

值得一提的是，在做除法意义下的膜法时，需要用到逆元。

## DIV2

~~Ps：早知道就用OI赛制了，这样Imakf就不能AK了。~~

### 价格统计

直接依照题意模拟即可，把细节把握好。

### 月光计算

这是一道非常简单的数学题。

$n$行，$n-1$列的星星加上一行不少于1的星星肯定是要大于$(n-1)^2$颗星星的。


$$
\left\{
\begin{aligned}
y1&=n(n-1)+1=n^2-n+1\\
y2&=(n-1)^2=n^2-2n+1\\
\end{aligned}
\right.
$$
所以可以得出$y1>y2$。答案即为$n^2$，不过记得开long long

### 月饼管理

提示中的数据结构是前缀和。

您可以参考我的[这篇](https://www.micdz.cn/article/basic-ds/#%E5%89%8D%E7%BC%80%E5%92%8C%E5%B7%AE%E5%88%86)文章。

这里就不多累赘了。
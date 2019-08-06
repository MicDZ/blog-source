---
title: 拓展欧几里得
date: 2019-08-03 13:42:05
tags: [笔记,数学]
categories:
- OI   
---



数论是学习OI的自闭之路。QAQ

<!--more-->

问题如下：

求不定方程的整数解
$$
ax+by=\gcd(a,b)
$$
的解。

通过欧几里得原理：
$$
\gcd(a,b)=\gcd(b,a\%b)
$$
那么原式可化为：
$$
bx'+a\%by'=gcd(b,a\%b)
$$
那么只需求出$x$与$x'$，$y$与$y'$的关系即可：
$$
\begin{align}
bx'+a\%by'&=gcd(b,a\%b)\\=\gcd(a,b)&=ax+by\\

\end{align}
$$
将含$a$与含$b$的合并
$$
\begin{align}
&bx'+a\%by'=ax+by\\
&bx'+ay'-\lfloor\frac{a}{b}\rfloor b\times y'=ax+by\\
&b(x'-y-\lfloor\frac{a}{b}\rfloor\times y')+a(y'-x)=0
\end{align}
$$
已知该式恒成立，则：
$$
\begin{align}
x&=y'\\
y&=x'-\lfloor\frac{a}{b}\rfloor\times y'
\end{align}
$$
再利用辗转相除的函数递归计算$x$，$y$即可。

代码如下：

```cpp
int x,y;

int exgcd(int a,int b) {
	if(b==0) {x=1,y=0;return a;}
	int g=exgcd(b,a%b);
	int oldx=x,oldy=y;
	x=oldy;
	y=oldx-a/b*oldy; 
	return g;
}
```

这里需要注意的是，递归的边界为$x=1,y=0$时的一组特解。

那么将此式拓展为一般结论，即求解：

$$
ax+by=c
$$
首先讨论有无解，当$\gcd(a,b)\nmid c$一定无解，这里就不给出证明。我们另$k=\frac{c}{\gcd(a,b)}$：
$$
ax'k+by'k=\gcd(a,b)\times k=c
$$
就得到了$x=x'k$，$y=y'k$。

如果要求$x$非负且最小，另$t=\frac{b}{\gcd(a,b)}$，则$(x\%t+t)\%t$就是$x$的最小非负解。加$t$主要是为了处理负数的问题。

有关例题：


---
title: 【HNOI2008】玩具装箱 题解
categories:
  - [OI,题解]
date: 2019-10-23 23:26:06
tags: [题解,动态规划]
---
[HNOI2008](https://www.luogu.org/problem/P3195)
<!--more-->

Tham题目的难度跨度真大。

## 核心思路

首先有一个很显然的dp，设$\mathrm{dp}_i$为前$i$个的最小花费
$$
\mathrm{dp}_i=\min_{j=0}^{i-1}\{\mathrm{dp}_j+(i-j-1+\mathrm{sum}_i-\mathrm{sum}_j-L)^2\}
$$
斜率优化的套路就是把含$i$、$j$的分离开来，为了更加方便：
$$
a_i=\mathrm{sum}_i+i\ ,\ b_i=\mathrm{sum}_i+i+L+1\\
$$

方程转化为：
$$
\begin{aligned}
\mathrm{dp}_i&=\min_{j=0}^{i-1}[(a_i-b_j)^2+\mathrm{dp}_j]\\

\mathrm{dp}_i&=(a_i-b_j)^2+\mathrm{dp}_j\\
\end{aligned}
$$


$$
2\times a_ib_j+\mathrm{dp}_i-a_i^2=\mathrm{dp}_j+b_j^2
$$

再将这个式子看做一个一次函数，$a_i$是已知的
$$
k=2a_i,x=b_j,y=\mathrm{dp}_j+b_j^2
$$

$$
y=kx+\mathrm{dp_i}-a_i^2
$$

$$
\mathrm{dp}_i=y-kx+a_i^2
$$

任务是要找到$\mathrm{dp}_i$的最小值

数形结合可以理解为，上述直线过点$(b_j,\mathrm{dp}_j+b_j^2)$，直线在$y$轴截距增加$a_i^2$。

最优解即在这些点的下凸包上。

用单调队列维护一下就可以求出下凸包。边求边统计答案。

##　完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
using namespace std;

#define int ll

#define REP(i,e,s) for(register int i=e; i<=s; i++)
#define DREP(i,e,s) for(register int i=e; i>=s; i--)
#define ll long long
#define DE(...) fprintf(stderr,__VA_ARGS__)
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int MAXN=50000+10;

int c[MAXN],f[MAXN],q[MAXN],a[MAXN],b[MAXN];

double calc(int i,int j) {
	int yi=f[i]+b[i]*b[i],yj=f[j]+b[j]*b[j];
	return 1.0*(yi-yj)/(b[i]-b[j]);
}

signed main() {
	int n=read(),L=read()+1,l=1,r=1;
	REP(i,1,n) c[i]=read()+c[i-1];
	REP(i,0,n) a[i]=c[i]+i;
	REP(i,0,n) b[i]=a[i]+L;

	REP(i,1,n) {
		while(l<r&&calc(q[l],q[l+1])<2*a[i]) l++;
		f[i]=f[q[l]]+(a[i]-b[q[l]])*(a[i]-b[q[l]]);
		while(l<r&&calc(i,q[r-1])<calc(q[r-1],q[r])) r--;
		q[++r]=i;
	}

	printf("%lld\n",f[n]);
	return 0;
}
```


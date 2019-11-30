---
title: 【POI2017】flappybird 题解
categories:
  - OI
date: 2019-11-08 12:45:00
tags: [题解,贪心]
---

[POI](https://www.lydsy.com/JudgeOnline/problem.php?id=4723)

<!--more-->

# 核心思路

乍一看，这道题像是NOIP的flappybird的加强版。然而，认真分析一下好像任何dp都不能解决这道题。

那么考虑一下题目有什么特殊性质：

1. 不能多次点击
2. 每一次要么加一，要么减一

考虑直接模拟小鸟飞行的过程。

从第 $i$ 个障碍物到第 $i+1$ 个障碍物，小鸟飞行的区间：
$$
\begin{cases}
\max_i=\min\{\max_{i-1}+x_i-x_{i-1},\mathrm{high}_i\}\\
\min_i=\max\{\min_{i-1}-(x_i-x_{i-1}),\mathrm{low}_i)\}
\end{cases}
$$
推完之后你会发现，小鸟并不能出现在这个区间的每一个位置上，因为点一次与不点产生的差距为 $2$ ，即你无法改变最终到达位置的奇偶性。那么再判一下奇偶性即可。

# 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>

using namespace std;

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

const int MAXN=5e5+10,INF=0x3f3f3f;

int x[MAXN],low[MAXN],up[MAXN];

int main() {
	int n=read(),X=read();
	REP(i,1,n) x[i]=read(),low[i]=read(),up[i]=read();
	
	int a=0,b=0;

	REP(i,1,n) {
		a=min(a+(x[i]-x[i-1]),up[i]-1);
		b=max(b-(x[i]-x[i-1]),low[i]+1);
		if((a&1)!=(x[i]&1)) a--;
		if((b&1)!=(x[i]&1)) b++;

		if(a<b) {
			puts("NIE");
			return 0;
		}
	}
	
	printf("%d\n",(x[n]+b)/2);
	return 0;
}
```


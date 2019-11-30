---
title: 【NOIP2016】愤怒的小鸟 题解
categories:
  - OI
date: 2019-11-01 12:39:42
tags: [题解,状压]
---

[NOIP2016](https://www.luogu.org/problem/P2831)

<!--more-->

# 核心思路

直接搞肯定不行。考虑状压。 

设 $f(S)$ 表示，射落集合为 $S$ 的小猪所需要的最少鸟数。转移就很显然了：
$$
\begin{aligned}
\begin{cases}
f(0)=0\\
f(S\lor\mathrm{line}_{i,j})=\min \{f(S)+1\}\\
f(S\lor2^{i-1})=\min\{f(S)+1\}
\end{cases}
\end{aligned}
$$
其中 $\mathrm{line}_{i,j}$ 表示的为经过 $i,j$ 两点的抛物线可以射落的小猪的集合。

在枚举 $i,j$ 的时候考虑 $S$ ，找到 $S\lor2^{x-1}=0$ 的最小正整数，这样由 $S$ 拓展的所有点都要经过这个点。就将原来的 $\Theta(Tn^22^n)$ 变为了 $\Theta(Tn+Tn2^n)$ 。

$\Theta(Tn2^n)$ 实在和 $\Theta(Tn^22^n)$ 没什么区别啊。

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
#define eps 1e-8
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int MAXN=20+5;

double x[MAXN],y[MAXN];
int dp[(1<<20)+5],line[MAXN][MAXN];

double a,b;
void calc(int i,int j) {
	a=(y[j]-(y[i]*x[j])/x[i])/(x[j]*x[j]-x[i]*x[j]);
	b=y[i]/x[i]-x[i]*a;
}

int lowx[(1<<20)+5];

int main() {
	int t=read();
	while(t--) {
		memset(dp,127,sizeof(dp));
		memset(line,0,sizeof(line));
		dp[0]=0;
		int n=read(),m=read();
		REP(i,1,n)	scanf("%lf%lf",&x[i],&y[i]);
		REP(i,1,n) REP(j,1,n) {
			if(fabs(x[i]-x[j])<eps||x[i]*x[j]==0) continue;
			calc(i,j);
			if(a>-eps) continue;
			REP(k,1,n) if(fabs(a*x[k]*x[k]+b*x[k]-y[k])<=eps) line[i][j]|=(1<<(k-1));
		}
		
		REP(s,0,(1<<n)-1) {
			int i=1;
			while(i<=n&&s&(1<<(i-1))) i++;
			dp[s|(1<<(i-1))]=min(dp[s|(1<<(i-1))],dp[s]+1);
			REP(j,1,n) dp[s|line[i][j]]=min(dp[s|line[i][j]],dp[s]+1);
		}
		printf("%d\n",dp[(1<<n)-1]);
	}
	return 0;
}
```


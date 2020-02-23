---
title: 【NOIP2016】换教室 题解
categories:
  - [OI,题解]
date: 2019-10-22 20:17:52
tags: [题解,图论,期望,容斥]
---



[NOIP2016](https://www.luogu.org/problem/P1850)

<!--more-->

## 核心思路

设$dp_{i,j,0/1}$表示前$i$个教室，提交了$j$个申请，第$i$个教室是否成功的最小期望体力。

$\mathrm{dis}_{i,j}$表示从$i$到$j$的最短距离，Floyd初始化。

下面是完整的转移方程。
$$
\begin{aligned}
dp_{i,j,0}&=\min\begin{cases}dp_{i-1,j,0}+dis_{c_{i-1},c_i}\\dp_{i-1,j,1}+(1-k_{i-1})dis_{c_{i-1},c_i}+k_{i-1}dis_{d_{i-1,c_i}}\end{cases}
\\
dp_{i,j,1}&=\min\begin{cases} dp_{i-1,j-1,0}+dis_{c_{i-1},c_i}(1-k_i)+dis_{c_{i-1},d_i}k_i\\dp_{i-1,j-1,1}+dis_{d_{i-1},d_i}k_ik_{i-1}+dis_{d_{i-1},c_i}k_{i-1}(1-k_i)+dis_{c_{i-1},d_i}(1-k_{i-1})k_i+dis{c_{i-1},c_i}(1-k_i)(1-k_{i-1})\end{cases}
\end{aligned}
$$

## 完整代码

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

const int MAXN=2000+10,INF=0x3f3f3f;

int c[MAXN],d[MAXN],a[MAXN][MAXN];
double dp[MAXN][MAXN][2],k[MAXN];

signed main() {
	int n=read(),m=read(),v=read(),e=read();
	REP(i,1,n) c[i]=read();
	REP(i,1,n) d[i]=read();
	REP(i,1,n) scanf("%lf",&k[i]);

	REP(i,0,v) REP(j,0,v) a[i][j]=INF;
	REP(i,1,v) a[i][i]=a[i][0]=a[0][i]=0;

	REP(i,1,e) {
		int x=read(),y=read(),w=read();
		a[x][y]=a[y][x]=min(a[x][y],w);
	}

	REP(t,1,v) REP(i,1,v) REP(j,1,v) a[i][j]=min(a[i][j],a[i][t]+a[t][j]);
	REP(i,0,n) REP(j,0,m) dp[i][j][0]=dp[i][j][1]=INF;
	
	dp[1][0][0]=dp[1][1][1]=0;
		DE("in");
	REP(i,2,n) {
		dp[i][0][0]=dp[i-1][0][0]+a[c[i-1]][c[i]];
		REP(j,1,min(i,m)) {
			dp[i][j][0]=min(dp[i-1][j][0]+a[c[i-1]][c[i]],dp[i-1][j][1]+a[c[i-1]][c[i]]*(1-k[i-1])+a[d[i-1]][c[i]]*k[i-1]);
			dp[i][j][1]=min(dp[i-1][j-1][0]+a[c[i-1]][c[i]]*(1-k[i])+a[c[i-1]][d[i]]*k[i],
			dp[i-1][j-1][1]+a[d[i-1]][d[i]]*k[i]*k[i-1]+a[d[i-1]][c[i]]*k[i-1]*(1-k[i])+a[c[i-1]][d[i]]*(1-k[i-1])*k[i]+a[c[i-1]][c[i]]*(1-k[i])*(1-k[i-1]));
		}
	}

	
	double ans=INF;
	
	REP(i,0,m) DE("%.2lf\n",ans=min(ans,min(dp[n][i][1],dp[n][i][0])));
	printf("%.2lf\n",ans);
	return 0;
}
```


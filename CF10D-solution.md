---
title: 【CF10D】LCIS 题解
categories:
  - [OI,题解]   
date: 2019-10-23 23:26:06
tags: [题解,动态规划]
---

[Codeforces Beta Round #10](https://codeforces.com/contest/10)D题

<!--more-->

## 核心思路及完整代码

$f_{i,j}$表示$a$中前$i$个与$b$中前$j$个的LCIS
$$
f_{i,j}=
\begin{cases}
\begin{aligned}
&f_{i,j-1},\ \ &a_i\neq b_j\\
&\max_{k=0}^{j-1}\{dp_{i-1,k}+1\}, \ \ &b_k<a_i\ and \ \ a_i=b_j
\end{aligned}
\end{cases}
$$
时间复杂度$\Theta(n^3)$

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
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

const int MAXN=500+10;

int n,m,a[MAXN],b[MAXN],dp[MAXN][MAXN],ans[MAXN][MAXN];

void print(int j) {
	if(!j) return ;
	print(ans[n][j]);
	printf("%d ",b[j]-1);
}

int main() {
	n=read(),m;
	REP(i,1,n) a[i]=read()+1;
	m=read();
	REP(i,1,m) b[i]=read()+1;
	
	REP(i,1,n) REP(j,1,m) {
	ans[i][j]=ans[i-1][j];
		if(a[i]!=b[j]) dp[i][j]=dp[i-1][j];
		else {
			REP(k,0,j-1) 
				if(b[k]<a[i]) {
					if(dp[i-1][k]+1>dp[i][j]) {
						dp[i][j]=dp[i-1][k]+1;
						ans[i][j]=k;
					}
				}
		}
	}

	int pos=0,ans=0;
	REP(i,1,m) if(dp[n][i]>ans) ans=dp[n][i],pos=i;
	printf("%d\n",ans);
	print(pos);
	return 0;
}
```



每次$k$都从$0$开始检查到$j-1$浪费了时间，只需要记录下上次的结果和计算一下$b_{j-1}$与$a_i$关系即可。
$$
f_{i,j}=
\begin{cases}
\begin{aligned}
&f_{i,j-1},\ \ &a_i\neq b_j\\
&\max\{max',b_{j-1}\}+1, \ \ &b_{j-1}<a_i\ and \ \ a_i=b_j\\
&max', \ \ &b_{j-1}\geq a_i
\end{aligned}
\end{cases}
$$
时间复杂度$\Theta(n^2)$

我就没写了，大家去查xgzc、M_sea的啊。


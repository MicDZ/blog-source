---
title: 【CF5E】Bindian Signalizing 题解
categories:
  - OI
date: 2019-10-22 12:04:41
tags: [题解,单调栈]
---

[Codeforces Beta Round #5](https://codeforces.com/contest/5)E题

<!--more-->

# 核心思路

大佬们用的都是[单调栈](https://www.cnblogs.com/tham/p/8038828.html) 。~~考场没想（想不到）那么多~~直接打了一个RMQ。

官方题解貌似不是单调栈维护的，实现起来也非常简单。

首先显然的是，全局最高一定不会被两块石头跨过（如果有多个最高，任意一个均可）。那么可以直接从这里断开，将此之前的接到最后，变成一条链。处理起来就简单很多了。

但存在的问题是，一块石头很有可能绕过形成的链的最后一个点与全局最高产生贡献。解决的方案是，在形成的链末尾再补上一个全局最高。还需要减去因此而重复的点。

现在开始统计答案。设$\mathrm{l}_i$为$i$的左边第一个严格大于$i$的石头，$r_i$同理。$\mathrm{cnt}_x$表示所有高达$x$且位于$x$和$y$之间的山丘。

那么实现就很好做了。

# 完整代码

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

const int MAXN=2000000+10;

int h[MAXN],a[MAXN],c[MAXN],l[MAXN],r[MAXN];

signed main() {
//file("promise");
	int t=1;
	while(t--) {
		memset(c,0,sizeof(c));
		memset(l,0,sizeof(l));
		memset(r,0,sizeof(r));
		int n=read();
		REP(i,1,n) h[i]=read();
		int p=1;
		REP(i,1,n) if(h[i]>=h[p]) p=i;
		REP(i,0,n) a[i]=h[(i+p)%n==0?n:(i+p)%n];
	//	REP(i,0,n) printf("%d ",a[i]);
		
		//a[n]=h[p];	
		DREP(i,n-1,0) {
			r[i]=i+1;
			while(r[i]<n&&a[i]>a[r[i]]) r[i]=r[r[i]];
			if(r[i]<n&&a[i]==a[r[i]]) c[i]=c[r[i]]+1,r[i]=r[r[i]];
		}

		int ans=0;

		REP(i,0,n-1) {
			ans+=c[i];
			if(a[i]!=a[0]) {
				ans+=2;l[i]=i-1;
				while(l[i]>0&&a[i]>=a[l[i]]) l[i]=l[l[i]];
				if(l[i]==0&&r[i]==n) ans--;
			}
		}
		printf("%lld\n",ans);
	}
	return 0;
}
```


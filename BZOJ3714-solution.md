---
title: 【BZOJ3714】Kuglarz 题解
categories:
  - [OI,题解]
date: 2019-10-17 12:05:59
tags: [题解,最小生成树]
---

PA2014

<!--more-->

## 核心思路

知道了 $a$ 到 $b$ 、 $b$ 到 $c$ 的奇偶性，就知道了 $a$ 到 $c$ 的奇偶 性。要知道任意两点间的奇偶性就只需要保证图联通即可，问题转化为最小生成树。由于是一个完全图，kruskal的复杂度为 $O(n^2\log n^2)$ 的会TLE，只能上prim。

## 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>

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

const int MAXN=2000+10,INF=0x3f3f3f3f3f3f3f3f;

int a[MAXN][MAXN];

int dist[MAXN],vis[MAXN];

Signed main () {
	int n=read(),cnt=0;
	
	REP(i,1,n) REP(j,i,n) a[i-1][j]=a[j][i-1]=read();

	REP(i,0,n) dist[i]=a[1][i];	
	
	vis[1]=1;
	int ans=0;
	REP(u,1,n) {
		int minn=INF,pos;
		REP(i,0,n) if(!vis[i]&&dist[i]<minn) minn=dist[i],pos=i;
		ans+=minn;vis[pos]=1;
		REP(i,0,n) dist[i]=min(dist[i],a[pos][i]);
	}

	printf("%lld\n",ans);
	return 0;
}
```


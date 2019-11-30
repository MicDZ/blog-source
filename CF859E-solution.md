---
title: 【CF859E】Desk Disorder 题解
categories:
  - OI
date: 2019-10-20 23:27:21
tags: [数学]
---

[MemSQL Start UP 3.0 - Round 1](https://codeforces.com/contest/859)E题

<!--more-->

# 核心思路

首先，对于每一个人，都只有一种或两种选法。

那么将人与位置对应连两条边。所形成的的图可以分为以下三种情况。

1. 无环图（树），有$x-1$ 个椅子与$x$个点，有$x$种情况
2. 这个图中包含一个非自环的环，整个图都是有前后依赖的，确定一条边整个图的取法就确定了，那么有$2$种情况
3. 这个图中包含了一个自环。整个图都被自环锁定，只有一种情况

最后用乘法原理把情况统计起来。

# 完整代码

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

const int MAXN=200000+10,MOD=1e9+7;

int fa[MAXN],size[MAXN],selfloop[MAXN],loop[MAXN];

int find(int x) {
	if(fa[x]==x) return fa[x];
	return fa[x]=find(fa[x]);
}
int link(int x,int y) {
	fa[find(x)]=find(y);
}

int main() {
	int n=read();
	
	REP(i,1,(n<<1)) fa[i]=i,size[i]=1;
	
	ll ans=1;
	
	REP(i,1,n) {
		int u=read(),v=read();
		if(u==v) {selfloop[u]=1;continue;}
		u=find(u),v=find(v);
		if(u!=v) fa[u]=v,selfloop[v]|=selfloop[u],size[v]+=size[u];
		else loop[u]=1;
	}

	REP(i,1,(n<<1)) 
		if(find(i)==i&&!selfloop[i]) {
			if(loop[i]) ans=1ll*ans*2;
			else ans=1ll*ans*size[i];
			ans%=MOD;
		}
	printf("%lld\n",ans);
	return 0;
}
```


---
title: 【SDOI2015】寻宝游戏 题解
categories:
  - [OI,题解]
date: 2019-10-18 23:27:15
tags: [题解,LCA]
---



[SDOI2015](https://www.luogu.org/problemnew/show/P3320)

<!--more-->

## 核心思路

题目要求的就是关键点的极小联通子树。可以算半个结论题了。~~这个结论也不难想~~

再求出图的DFS序之后，所有关键点$\{a_1,a_2,...,a_n\}$的极小联通子树的边权和的两倍为
$$
\sum_{i=1}^{n-1}\mathrm{dist}(a_i,a_{i+1}+\mathrm{dist}(a_n,a_1))
$$
手玩一下很好理解。

那么每次修改$x$的贡献就为
$$
\mathrm{dist}(x,pre)+\mathrm{dist}(x,nxt)-\mathrm{dist}(pre,nxt)
$$
$\mathrm{dist}$就拿LCA直接维护就好了，找前驱和后继用STLset最方便

## 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<set>
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

const int MAXN=100000+10;

int head[MAXN],_next[MAXN<<1],to[MAXN<<1],weigh[MAXN<<1],cnt;

void addedge(int u,int v,int w) {
	cnt++;
	_next[cnt]=head[u];
	head[u]=cnt;
	to[cnt]=v;
	weigh[cnt]=w;
}

int dfn[MAXN],tot,fa[MAXN][21],dept[MAXN],dist[MAXN];

int lca(int u,int v) {
	if(dept[u]>dept[v]) swap(u,v);
	int len=dept[v]-dept[u];
	DREP(i,20,0) if((1<<i)&len) v=fa[v][i];
	if(u==v) return u;
	DREP(i,20,0) if(fa[u][i]!=fa[v][i]) u=fa[u][i],v=fa[v][i];
	return fa[u][0];
}

int dis(int u,int v) {
	return dist[u]+dist[v]-2*dist[lca(u,v)];
}
int have[MAXN],pos[MAXN],id[MAXN];

void dfs(int u,int f) {
	dfn[u]=++tot;id[tot]=u;
	for(int i=head[u]; i; i=_next[i]) {
		int v=to[i];
		if(dfn[v]) continue;
		fa[v][0]=u;
		dept[v]=dept[u]+1;
		dist[v]=dist[u]+weigh[i];
		dfs(v,u);
	}
}

set<int> s;

signed main() {
	int n=read(),m=read();
	REP(i,1,n-1) {
		int u=read(),v=read(),w=read();
		addedge(u,v,w);
		addedge(v,u,w);
	}

	dfs(1,0);
	

	REP(j,1,20) REP(i,1,n) fa[i][j]=fa[fa[i][j-1]][j-1];
	int ans=0;
	REP(i,1,m) {
		int x=dfn[read()],y,z;
		set<int> :: iterator it;

		if(!have[id[x]]) {
			s.insert(x);
			
			y=id[(it=s.lower_bound(x))==s.begin()?*--s.end():*--it];
        	z=id[(it=s.upper_bound(x))==s.end()?*s.begin():*it];
			
			x=id[x];
			ans+=(dis(x,y)+dis(x,z)-dis(y,z));
			have[x]=1;

		}
		else {
			y=id[(it=s.lower_bound(x))==s.begin()?*--s.end():*--it];
        	z=id[(it=s.upper_bound(x))==s.end()?*s.begin():*it];
			s.erase(x);
			x=id[x];

			ans-=(dis(x,y)+dis(x,z)-dis(y,z));
			have[x]=0;
		}
		printf("%lld\n",ans);
	}
	return 0;
}
```


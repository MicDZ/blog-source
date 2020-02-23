---
title: 最小生成树算法总结
date: 2019-08-11 15:45:30
tags: [图论,最短路,最小生成树]
categories:
- OI   
---

最小生成树是最简单的图论算法。

本文递交已结束。

  <!--more-->

## kruskal

kruskal算法是基于贪心的最小生成树算法。其算法实现流程如下：将边按照边权从小到大排序，从最小的边开始加入生成树，如果当前边的两端点未联通则可以加入生成树不会形成环，直至加到第$n-1$ 条边为止。算法的复杂度决定于排序与并查集大致为$m\log m$。

证明kruskal的正确性在课堂上没有严格说明（？），参考《算法竞赛进阶指南》证明方法如下：
    

**定理**：任意一棵最小生成树一定包含无向图中权值最小的边。

证明：假设有一棵在无向图$G$中的最小生成树不包含权值最小的边。设边$e(u,v,w)$为该无向图中权值最小的边。将$e$加入树种则会和树上从$u$ 到$v$的路径一起构成一个环，且环上其他边的权都比$w$大，那么$e$可以替代任何其他边形成一棵比当前最小更小的生成树，产生矛盾，则假设不成立，原命题成立。


有了上述的定理推导即可得出kruskal的正确性。
    

## Prim

Prim算法也基于上面的定理，不同的是其思路由向生成树中加边变为加点，实现流程类似于Dijkstra算法。每次选择距离已确定生成树最短的点加入生成树，用堆优化可以实现$\Theta(m\log n)$的复杂度。
    

考虑kruskal算法与Prim算法的区别，Prim在处理稠密图时效率高于kruskal算法。

```cpp
#include<bits/stdc++.h>
using namespace std;

const int MAXN=400000+10;
#define REP(i,e,s) for(register int i=e; i<=s; i++)
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
int head[MAXN],_next[MAXN],to[MAXN],weigh[MAXN],cnt,dis[MAXN];
bool vis[MAXN];
void addedge(int u,int v,int w) {
	cnt++;
	_next[cnt]=head[u];
	head[u]=cnt;
	to[cnt]=v;
	weigh[cnt]=w;
}

typedef pair<int,int> p;
priority_queue <p,vector<p>,greater<p> > q;


int main() {
		//freopen("test.in","r",stdin);
	int n=read(),m=read();
	REP(i,1,m) {
		int u=read(),v=read(),w=read();
		addedge(u,v,w);
		addedge(v,u,w);
	}

	memset(dis,127,sizeof(dis));
	dis[1]=0;
	q.push(make_pair(0,1));
	int tot=0,sum=0;  
	while(!q.empty()&&tot<n) {
		int d=q.top().first,u=q.top().second;
		//puts("in");
		q.pop();
		if(vis[u]) continue;
		tot++; sum+=d;
		vis[u]=1;
		for(int i=head[u]; i; i=_next[i]) if(weigh[i]<dis[to[i]]) {
			dis[to[i]]=weigh[i];
			q.push(make_pair(dis[to[i]],to[i]));
		}
	}

	printf("%d\n",sum);
	return 0;
} 
```
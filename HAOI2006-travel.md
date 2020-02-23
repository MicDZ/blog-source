---
title: '【HAOI2006】Travel 题解'
date: 2019-07-29 10:50:23
tags: [题解,图论]
categories:
- OI   
---

[HAOI]()

<!--more-->

## 题目大意

给定一张图，求从s到t经过边权最大与最小之比最小值。

## 核心思路

因为要保证$s$、$t$联通，所以按照普通的bfs思路是行不通的，会导致更新错乱的问题，因为可能到这个点的所经过最大边的最小值的路径与所经过最小边的最大值的路径是不同的，但是似乎有大佬写出了此题的bfs解法。

那么考虑最小生成树的算法。考虑如下问题，如果固定图中的最小边，那么存在此边的使$s$、$t$联通的路径的最大边最小值一定是固定的。

那么在这固定的含有固定的最小边的使$s$、$t$联通的路径的最大边一定是含有此固定边的使$s$、$t$联通的图中中最长边最短的。

那么对于每一条边，向其中加入比它长的边，直至联通，再用图中最长边与最短边更新答案即可。

## 完整代码

```cpp
#include<iostream>
#include<cstring>
#include<cstdio>
#include<algorithm>
#include<queue>

using namespace std;
#define REP(i,e,s) for(register int i=e; i<=s; i++)
int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int MAXN=500+10,MAXM=50000+10;

struct edge {
	int u,v,w;
}a[MAXM];

bool cmp(edge a,edge b) {
	return a.w<b.w;
}
int fa[MAXN];

int find(int x) {
	if(fa[x]==x) return x;
	return fa[x]=find(fa[x]);
}
int link(int x,int y) {
	fa[find(x)]=find(y);
}
int main() {
	int n=read(),m=read();
	REP(i,1,n) fa[i]=i;
	REP(i,1,m) {
		a[i].u=read();
		a[i].v=read();
		a[i].w=read();
		link(a[i].u,a[i].v);
	}
	
	int s=read(),t=read();
	
	if(find(s)!=find(t)) {puts("IMPOSSIBLE");return 0;}

	
	sort(a+1,a+1+m,cmp);
	
	
	int maxx=0,minn=0;
	REP(i,1,m) {
		REP(j,1,n) fa[j]=j;
		int now=0;
		for(now=i; now<=m; now++) {
		//	now=j;
			if(find(a[now].u)==find(a[now].v)) continue;
			link(a[now].u,a[now].v);
			if(find(s)==find(t)) break;
		}
		if(find(s)!=find(t)) break;
		if(maxx*a[i].w>=a[now].w*minn) maxx=a[now].w,minn=a[i].w;
	}
	int g=__gcd(maxx,minn);
	if(minn/g!=1)
	printf("%d/%d\n",maxx/g,minn/g);
	else printf("%d\n",maxx/g);
	return 0;
}
```
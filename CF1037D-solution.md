---
title: '【CF1037D】Valid BFS? 题解'
categories:
  - OI
date: 2019-10-15 23:16:42
tags: [题解,图论,模拟]
---

[Manthan, Codefest 18 (rated, Div. 1 + Div. 2)](https://codeforces.com/contest/1037)D题

<!--more-->

# 核心思路

直接照题意模拟bfs的过程，顺序加入每个点与当前点的出边对应。用一个set维护最为直观。

nzr表示让我们不要学习这种$\Theta(n\log n)$的做法。

# 完整代码

我的代码太丑了，大家看nzr神仙的代码吧

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=200010;
int n,m;
struct edge
{
	int to,next;
}e[N<<1];
int head[N],cnt;
void adde(int a,int b)
{
	e[++cnt].to=b;e[cnt].next=head[a]; head[a]=cnt;
}
queue<int>q;
int dep[N];
int a[N];
void out()
{
	puts("No");
	exit(0);
}
int now=1;
void bfs()
{
	dep[1]=1;
	q.push(1);
	while(!q.empty())
	{
		int x=q.front(); q.pop(); int ct=0;
		for(int i=head[x];i;i=e[i].next)
		{
			int v=e[i].to;
			if(dep[v])continue;
			dep[v]=dep[x]+1;ct++; 
		}
		for(int i=now+1;i<=now+ct;i++)
			if(!dep[a[i]])out();
			else
				q.push(a[i]);
		now=now+ct;
	}
}
int main()
{
	scanf("%d",&n);
	int u,v;
	for(int i=1;i<n;i++)
		scanf("%d%d",&u,&v),adde(u,v),adde(v,u);
	for(int i=1;i<=n;i++)
		scanf("%d",&a[i]);
	if(a[1]!=1)out();
	bfs();
	puts("Yes");
	return 0;
}
```


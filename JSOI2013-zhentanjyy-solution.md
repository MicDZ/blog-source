---
title: 【JSOI2013】侦探jyy 题解
tags:
  - 题解
  - 贪心
categories:
  - - OI
    - 题解
date: 2020-02-03 09:35:43
---

[JSOI2013](https://www.lydsy.com/JudgeOnline/problem.php?id=4478)

<!--more-->

## 核心思路

我们枚举每一个点，假设这个点不发生，那么它的前驱都不能发生。搜索完它的全部前驱打上标记作为这些点不能发生。然后再从剩下不一定发生的点中贪心地将每一个入度为 $0$ 的设定为一定发生，统计它们的全部后继，如果能够覆盖题目所给的 $d$ 个点，那么最开始枚举的这个点就可以不发生，否则一定发生。

## 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>
#include<vector>
#include<queue>

using namespace std;

#define REP(i,e,s) for(register int i=(e); i<=(s); i++)
#define DREP(i,e,s) for(register int i=(e); i>=(s); i--)
#define DE(...) fprintf(stderr,__VA_ARGS__)
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
#define ll long long

int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int MAXN=1000+10,MAXM=100000+10;

int head[MAXN],_next[MAXM],to[MAXM],cnt;

void addedge(int u,int v) {
	cnt++;
	_next[cnt]=head[u];
	head[u]=cnt;
	to[cnt]=v;
}
vector<int> g[MAXN];

int book[MAXN],flag[MAXN],mark[MAXN];

queue<int> q;

void bfs(int s) {
	q.push(s);
	while(!q.empty()) {
		int u=q.front();q.pop();
		REP(i,0,(int)g[u].size()-1) {
			int v=g[u][i];
			if(flag[v]) continue;
			flag[v]=1;
			q.push(v);
		}
	}
}

int in[MAXN];

void bfs2(int s) {
	q.push(s);mark[s]=1;
	while(!q.empty()) {
		int u=q.front();q.pop();
		for(int i=head[u]; i; i=_next[i]) {
			int v=to[i];
			if(mark[v]) continue;
			mark[v]=1;
			q.push(v);
		}
	}
} 

int ans[MAXN];

int main() {
	int n=read(),m=read(),d=read();
	REP(i,1,m) {
		int u=read(),v=read();
		addedge(u,v);in[v]++;
		g[v].push_back(u);
	}
	REP(i,1,d) book[read()]=1; 

	REP(i,1,n) {
		memset(flag,0,sizeof(flag));
		memset(mark,0,sizeof(flag));
		bfs(i);
		REP(j,1,n) 
			if(!flag[j]&&in[j]==0&&j!=i) bfs2(j);
		REP(j,1,n) {
			if(!mark[j]&&book[j]) {
				ans[i]=1;
				break;
			}
		}
	}
	REP(i,1,n) if(ans[i]) printf("%d ",i);
	return 0;
}

```
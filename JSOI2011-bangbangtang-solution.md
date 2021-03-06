---
title: 【JSOI2011】棒棒糖 题解
tags: [题解,可持久化,线段树]
categories:
  - - OI
    - 题解
date: 2020-02-01 16:47:00
---

[JSOI2011](https://www.lydsy.com/JudgeOnline/problem.php?id=5178)

<!--more-->

## 核心思路

直接上一棵主席树，不用离散化美滋滋。

## 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>

using namespace std;

#define REP(i,e,s) for(register int i=(e); i<=(s); i++)
#define DREP(i,e,s) for(register int i=(e); i>=(s); i--)
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

const int MAXN=200000+10,MAXM=MAXN*18;;

int rt[MAXM],a[MAXN];

struct President_Tree {
	int cnt,ls[MAXM],rs[MAXM],sum[MAXM];
	President_Tree() {
		cnt=0;
	}
	void build(int &p,int l,int r) {
		p=++cnt;
		if(l==r) return ;
		int mid=(l+r)>>1;
		build(ls[p],l,mid);
		build(rs[p],mid+1,r);
	}
	void change(int &p,int l,int r,int u,int x) {
		p=++cnt,ls[p]=ls[u],rs[p]=rs[u],sum[p]=sum[u]+1;	
		if(l==r) return ;
		int mid=(l+r)>>1;
		if(x<=mid) change(ls[p],l,mid,ls[u],x);
		else change(rs[p],mid+1,r,rs[u],x);
	}
	int ask(int x,int y,int l,int r,int k) {
		if(l==r) return l;
		int mid=(l+r)>>1;
		if(sum[ls[y]]-sum[ls[x]]>=k) return ask(ls[x],ls[y],l,mid,k);
		else if(sum[rs[y]]-sum[rs[x]]>=k) return ask(rs[x],rs[y],mid+1,r,k);
		return -1;
	}
} s;


int main() {
	int n=read(),m=read();
	REP(i,1,n) a[i]=read();
	int sz=n;
	s.build(rt[0],1,sz);
	REP(i,1,n) s.change(rt[i],1,sz,rt[i-1],a[i]);

	while(m--) {
		int l=read(),r=read();
		int t=s.ask(rt[l-1],rt[r],1,sz,(r-l+1)/2+1);
		if(t!=-1) printf("%d\n",t);
		else puts("0");
	}
	return 0;
}
```
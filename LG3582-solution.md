---
title: 【POI2015】KIN 题解
categories:
  - [OI,题解]
date: 2019-10-20 23:12:02
tags: [线段树,最大子段和]
---

[洛谷P3582](https://www.luogu.org/problem/P3582)

<!--more-->

## 核心思路

此题与求解最大子段和唯一的区别在于，每一部电影如果重复则不能计入贡献。

处理起来比较巧妙。

我们从左往右扫过每一部电影。假设扫到了第$i$部，当前求解的$\mathrm{ans}_i$就是$1-i$部电影的子序列电影产生贡献的最大值。

考虑如何高效地处理这个问题。对于第一次出现的电影，直接在对应位置$+w_i$，为了去除重复的贡献，在第二次出现这部电影时，我们将上一次出现的位置更改为$-w_i$，在第二次出现的位置设为$w_i$。这样当同时选到第二个与第一个同样的电影时的贡献为$0$，而单独选择第二个电影的贡献可以直接包含（即不选到第一个相同的电影），单独包含第一个电影的贡献已经计算在了$\mathrm{ans}_{i-1}$中。选到第三个相同电影时，为了避免多次减去，需要将第一个相同电影位置处设为0，第二次位置处设为$-w_i$，当前位置设为$w_i$，如此往复。

最终答案就为
$$
\max_{i=1}^n\{ans_i\}
$$
是不是非常ZZ。

## 完整代码

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

const int MAXN=1000000+10,INF=0x3f3f3f3f;

int a[MAXN];

struct SegmentTree {
	int l,r,prel,prer,res,sum;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define prel(x) tree[x].prel
	#define prer(x) tree[x].prer
	#define res(x) tree[x].res
	#define sum(x) tree[x].sum
} tree[MAXN<<2];

void pushup(int p) {
	sum(p)=sum(p*2)+sum(p*2+1);
	prel(p)=max(prel(p*2),sum(p*2)+prel(p*2+1));
	prer(p)=max(prer(p*2+1),sum(p*2+1)+prer(p*2));
	res(p)=max(prer(p*2)+prel(p*2+1),max(res(p*2),res(p*2+1)));
}

void build(int p,int l,int r) {
	l(p)=l,r(p)=r;
	if(l==r) {
		return ;
	}
	int mid=(l+r)>>1;
	build(p*2,l,mid);
	build(p*2+1,mid+1,r);
}

void change(int p,int x,int d) {
	
	if(l(p)==r(p)) {
		prel(p)=prer(p)=res(p)=sum(p)=d;
		return ;
	}
	int mid=(l(p)+r(p))>>1;
	if(x<=mid) change(p*2,x,d);
	else change(p*2+1,x,d);
	pushup(p);
}


int f[MAXN],pre[MAXN],last[MAXN];

signed main() {
	int n=read(),m=read();
	REP(i,1,n) f[i]=read();
	REP(i,1,m) a[i]=read();
	build(1,1,n);
	int ans=0;
	REP(i,1,n) {
		pre[i]=last[f[i]],last[f[i]]=i;

		if(pre[i]) change(1,pre[i],-a[f[i]]);
	
		if(pre[pre[i]]) change(1,pre[pre[i]],0);
		change(1,i,a[f[i]]);
		ans=max(ans,res(1));
	}

	printf("%lld\n",ans);
	return 0;
}
```


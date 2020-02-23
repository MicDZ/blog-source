---
title: 【JOI 2018 Final】美术展览 题解
categories:
  - [OI,题解]
date: 2019-11-01 23:25:49
tags: [题解]
---



[JOI 2018 Final](https://loj.ac/problem/2348)

<!--more-->

这场挂分好惨啊😭

## 核心思路

首先，你所选取的美术品肯定是一个按照尺寸排序的连续区间。因为最终贡献只与最大尺寸与最小尺寸有关，中间的肯定是要全部加上。

观察这个式子
$$
\begin{aligned}
&S-(A_{max}-A_{min})\\
=&S-A_{max}+A_{min}
\end{aligned}
$$
可以直接维护一个后缀max，再 $\Theta(n)$ 地扫过统计答案。

官方题解强行解释部分分的操作真的很。。大家可以去看看

## 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>

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

const int MAXN=5000000+10,INF=0x3f3f3f3f3f3f;

struct work {
	int s,v;
} a[MAXN];

bool cmp(work a,work b) {
	return a.s<b.s;
}

int jian[MAXN],far[MAXN],qianz[MAXN];

signed main() {
	int n=read();
	REP(i,1,n) {
		a[i].s=read();
		a[i].v=read();
	}
	sort(a+1,a+1+n,cmp);
	REP(i,1,n) qianz[i]=qianz[i-1]+a[i].v;

	if(0&&n<=5000) {
		int ans=0;
		REP(i,1,n) REP(j,i,n) {
			ans=max(ans,qianz[j]-qianz[i-1]+a[i].s-a[j].s);
		}	
		printf("%lld\n",ans);
		return 0;
	}
	
	int sum=0;
	DREP(i,n,1) {
		jian[i]=-a[i].s-sum;
		sum+=a[i].v;
	}
	far[n]=n;	
	DREP(i,n-1,1) {
		if(jian[i]>jian[far[i+1]]) far[i]=i;
		else far[i]=far[i+1];
	}
	int ans=0;
	REP(i,1,n) {
		int l=i,r=far[i];
		ans=max(ans,qianz[r]-qianz[l-1]+a[l].s-a[r].s);
	}

	printf("%lld\n",ans);
	return 0;
}
```


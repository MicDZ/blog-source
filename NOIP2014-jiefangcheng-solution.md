---
title: 【NOIP2014】解方程 题解
categories:
  - [OI,题解]
date: 2019-10-31 12:39:49
tags: [题解,数学,黑科技]
---

[NOIP2014](https://www.luogu.org/problem/P2312)

<!--more-->

## 核心思路

说实话刚拿到这道题时，除了30pts，没有任何思路。

设原多项式方程的对应函数为：
$$
f(x)=a_0+a_1x+\cdots+a_nx^n
$$
题目所求就是 $f(x)=0$ 在 $x\in [1,m]$ 的整数解。

考虑 $f(x) \bmod p$ ，当 $f(x)=0$ 时显然等于$0$。

那么直接 $\Theta(nm)$ 暴力枚举 $[1,m]$ 并计算答案 $f(x) \bmod p$ 是否为$0$。

选择一个好的模数很重要。

读入的时候用快读，边读边模。

{% pdf /解方程.pdf %}

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

const int MAXN=100000+10,MOD=19260817;

int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=(x*10+ch-'0')%MOD;ch=getchar();}
	return x*f;
}

int a[MAXN],n,m;

int f(int x) {
	int ans=0,prod=1;
	REP(i,0,n) {
		ans=(ans+prod*a[i])%MOD;
		prod=(prod*x)%MOD;
	}
	return ans;
}

int book[MAXN];

int ans[MAXN],cnt;

signed main() {
	n=read(),m=read();
	
	REP(i,0,n) a[i]=read();

	REP(i,1,m) if(f(i)==0) ans[++cnt]=i;	
	printf("%lld\n",cnt);
	REP(i,1,cnt) printf("%lld\n",ans[i]);
	return 0;
}
```




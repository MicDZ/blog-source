---
title: 【JXOI2018】游戏 题解
categories:
  - [OI,题解]
date: 2019-11-03 22:55:49
tags: [题解,数学,组合]
---

[JXOI2018](https://www.luogu.org/problem/P4562)

<!--more-->

~~数学专题都好神仙啊~~

这道题神奇的有两个题面。

## 核心思路

我们把 $[l, r]$ 内不是其它任何数的倍数的数称为关键数，那么 $t(p)$ 显然等于该排列中最后一个关键点的位置。

设 $num$ 为 $[l,r]$ 中关键点的个数，那么枚举最后一个关键点的位置，答案就很显然了
$$
\sum_{i=1}^ni\times(i-1)!\times num \times{n-s\choose n-i}\times(n-i)!
$$
 稍微解释一下， $i$ 后面位置是不能再放关键点的，因此有 $n-s\choose n-i$ 种选法。 $i$ 位置必须为关键点，因此有 $num$ 种选法， $i$ 前与 $i$ 后随便选取，那么有 $(i-1)!\times(n-i)!$ 种选法。

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

const int MOD=1e9+7,MAXN=1e7+10;

int vis[MAXN],num,fac[MAXN],inv[MAXN];

int qpow(int a,int b) {
	int ans=1;
	while(b) {
		if(b&1) ans=ans*a%MOD;
		a=a*a%MOD;
		b>>=1;
	}
	return ans;
}

int c(int n,int m) {
	if(n<m) return 0;
	return fac[n]*inv[m]%MOD*inv[n-m]%MOD;
}

signed main() {
	int l=read(),r=read(),n=r-l+1;
	REP(i,l,r) {
		if(vis[i]) continue;
		num++;
		for(int j=i+i; j<=r; j+=i) vis[j]=1;
	}
	
	fac[0]=fac[1]=1;
	REP(i,2,n) fac[i]=fac[i-1]*i%MOD;

	inv[n]=qpow(fac[n],MOD-2);
	DREP(i,n-1,0) inv[i]=inv[i+1]*(i+1)%MOD;

	int ans=0;
	
	REP(i,1,n) ans=(ans+i*fac[i-1]%MOD*num%MOD*c(n-num,n-i)%MOD*fac[n-i]%MOD)%MOD;
	printf("%lld\n",ans);

	return 0;
}
//常数过大，开O2过
```


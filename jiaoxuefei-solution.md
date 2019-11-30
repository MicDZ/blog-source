---
title: 【T98549】交学费 题解
categories:
  - OI
date: 2019-10-01 23:52:59
tags: [二分答案,题解]
---

国庆比赛T2

<!--more-->

# 核心思路

答案显然具有单调性，要求最大的学费则直接二分答案即可。

# 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<algorithm>

using namespace std;
#define REP(i,e,s) for(register int i=e; i<=s; i++)
#define DREP(i,e,s) for(register int i=e; i>=s; i--)
#define DE(...) fprintf(stderr,__VA_ARGS__);
#define DEBUG(a) DE("DEBUG: %d\n",a)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)
#define ll long long

int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int INF=0x3f3f3f,MAXN=100000+10;

int n,m,k,p,a[MAXN],b[MAXN];

bool check(int x) {
    int cnt=0;

    REP(i,1,n) if(x<=a[i]) cnt++;
    REP(i,1,m) if(x<=b[i]+k) cnt++;

    return cnt>=p;
}


int main() {
    n=read(),m=read(),k=read(),p=read();
    
    int l=0,r=0;

    REP(i,1,n) a[i]=read(),r=max(a[i],r);
    REP(i,1,m) b[i]=read(),r=max(b[i]-k,r);

    while(l<r) {
        int mid=(l+r+1)>>1;
        if(check(mid)) l=mid;
        else r=mid-1;
    }

    printf("%d\n",r);
	return 0;
}

```


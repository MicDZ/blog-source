---
title: 【CF1203E】Boxers 题解
categories:
  - [OI,题解]
date: 2019-08-14 15:16:44
tags: [题解,贪心,排序]
---

Codeforces Round #579 (Div. 3)  E题

<!--more-->

## 题目大意

给定你$n$ 个数，你可以对每个数加一或减一或不变，求使最后序列不同数最多的个数。

## 核心思路

将$n$个数排序，然后从小到大对每个数优先减一，再不济就不变，再不济就加一。这是因为，如果你优先加一很有可能就会错过最小的数减一这个答案。

## 完整代码

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
 
const ll INF=0x3f3f3f,MAXN=150000+10;
 
int a[MAXN],have[MAXN];
 
int main() {
    int n=read();
    REP(i,1,n) a[i]=read();
    sort(a+1,a+1+n);
    int ans=0;
    REP(i,1,n) {
        if(have[a[i]-1]||a[i]-1<=0) {
            if(have[a[i]]) {
                if(have[a[i]+1]) continue;
                else have[a[i]+1]=1,ans++;
            }
            else have[a[i]]=1,ans++;
        }
        else have[a[i]-1]=1,ans++;
    }
 
    printf("%d\n",ans);
	return 0;
}
```


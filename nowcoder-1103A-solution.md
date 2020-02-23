---
title: 【nowcoder-1103A】复读数组 题解
categories:
  - [OI,题解]
date: 2019-11-06 23:07:21
tags: [题解,数学]
---



[nowcoder-1103](https://ac.nowcoder.com/acm/contest/1103)

CSP-S提高组赛前集训营4 A题

<!--more-->

刚拿到题就觉得比较神仙，又因为之前做过一道长得很像但几乎没什么关系的神仙题更加深了这种感觉。

看左右两位神仙半小时就交了一发还写了拍，我才刚读完题，感觉这场凉凉，结果还真凉了。

## 题目大意

给定长为 $n$ 的数列 $a_1,a_2,\cdots,a_n$ ，将这个数列循环 $k$ 次。求
$$
\sum_{1\leq i\leq j\leq n\times k}\left| \bigcup_{k=i}^j\{a_k\}\right|
$$
的值 $\bmod 10^9+7$ 。

## 核心思路

首先想想可不可以写一个 $O(nk)$ 的暴力，设 $f_i$ 为以 $i$ 为右端点区间的贡献。

这个式子一开始分类讨论要死。写了几十行还一直WA，调自闭后，冷静分析一下，好像是一个傻逼转移。

设 $pre_i$ 为最后一个与 $a_i$ 相同点的编号。转移如下：
$$
f_i=f_{i-1}+i-pre_i
$$
这样你就获得了70pts的好成绩。

这时离下考差不多只有1.5h了，我决定继续刚这题。

直接推式子，……，经过一番努力，你发现式子并不好推，但是把一些因数提出来之后貌似有等差数列的性质。

于是愉快地开始打表，打出每 $k$ 个的 $f_i$  之和，发现从第三项开始即为等差数列。

然后用高斯求和公式就愉快地解决了。

代码还是有些小细节。

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
 
const ll MOD=1e9+7,MAXN=6e5+10;
int a[MAXN],b[MAXN],pos[MAXN],pre[MAXN],f[MAXN];
 
int qpow(int a) {
    int ans=1,b=MOD-2;
    while(b) {
        if(b&1) ans=ans*a%MOD;
        a=a*a%MOD;
        b>>=1;
    }
    return ans;
}
 
int sum[4];
 
signed main() {
    int n=read(),k=read();
    REP(i,1,n) b[i]=a[i]=read();
    sort(b+1,b+1+n);
    int num=unique(b+1,b+1+n)-b-1;
    REP(i,1,n) a[i]=lower_bound(b+1,b+1+num,a[i])-b;
     
    REP(i,n+1,3*n) a[i]=a[i-n];
    REP(i,1,3*n) pre[i]=pos[a[i]],pos[a[i]]=i;
    REP(i,1,3*n) f[i]=f[i-1]+i-pre[i];
     
     
    REP(i,1,3) {
        sum[i]=0;
        REP(j,1+(i-1)*n,i*n) sum[i]=(sum[i]+f[j])%MOD;
    }//直接找出前3n个的每一节答案
     
    int ans=(sum[1]+(2*sum[2]%MOD+(sum[3]-sum[2]+MOD)%MOD*(k+MOD-2)%MOD)%MOD*(k+MOD-1)%MOD*qpow(2))%MOD;
    //从第二节开始为等差数列
    printf("%lld\n",ans%MOD);
    return 0;
}
//考场代码略丑，轻D
```


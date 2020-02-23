---
title: 【CF1066F】Yet another 2D Walking 题解
date: 2018-11-02 11:43:32
tags: [题解,图论,动态规划]
categories:
- [OI,题解]   
---

此题可以用动态规划求解。

<!--more-->

## 题目描述

题目链接[here](http://codeforces.com/problemset/problem/1066/F)

小M在平面直角坐标系上的$(0,0)$点。他每次可以向上下左右走一格。
如果两个点满足$\max(X_i,Y_i)\leq \max(X_j,Y_j)$，那么从$i$到$j$就有一条有向路径。
定义两点的距离为曼哈顿距离。
最小化路径长度。

![](https://i.loli.net/2018/11/08/5be39d473e10f.png)

## 核心思路

我们将所有的点按照$\max(x,y)$分层，就如上图所示。可以证明只有到达某一层并且将该层完全走完再走下一层是最优的。

定义$dp_{i,0}$表示到达第$i$条边的左端点的最小路径，$dp_{i,1}$表示到达第$i$条边的右端点的最小路径。

转移方程显然。

$$
\begin{aligned}
dp_{i,1}&=\min(dp_{i-1,1}+dist(right_{i-1},left_i),dp_{i-1,0}+dist(left_{i-1},left_i))+dis_{i-1};\\
dp_{i,0}&=\min(dp_{i-1,1}+dist(right_{i-1},right_i),dp_{i-1,0}+dist(left_{i-1},right_i))+dis_{i-1};\\
\end{aligned}
$$

在方程中$dis$表示某一层走完的长度。

## 完整代码

```cpp
#include<bits/stdc++.h>
using namespace std;

#define ll long long
#define  REP(i,e,s) for(register int i=e; i<=s; i++)
#define DREP(i,e,s) for(register int i=e; i>=s; i--)

#define MAXN 200000+10

ll read() {
    ll x=0,f=1,ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

struct edge{
    ll x,y,id;
}a[MAXN];

bool cmp(edge a,edge b) {
    ll as=max(a.x,a.y),bs=max(b.x,b.y);
    if(as==bs) {if(a.x==b.x) return a.y>=b.y;else return a.x<b.x;} 
    return as<bs;
}

ll dist(ll u,ll v) {
    return abs(a[u].x-a[v].x)+abs(a[u].y-a[v].y);
}

ll _right[MAXN],_left[MAXN],dis[MAXN];
ll dp[MAXN][2];

int main() {

    ll n=read();
    
    REP(i,1,n) {
        a[i].x=read();
        a[i].y=read();
        a[i].id=max(a[i].x,a[i].y);
    }

    sort(a+1,a+1+n,cmp);
    
    ll now=-1,cnt=0;

    ll ans=0;
    

    REP(i,1,n) {
        if(a[i].id!=a[i-1].id) {
            _right[cnt]=i-1;
            dis[cnt]=dist(_right[cnt],_left[cnt]);
            _left[++cnt]=i;
        } 
    }
    
    _right[cnt]=n;
    dis[cnt]=dist(_right[cnt],_left[cnt]);

    REP(i,1,cnt) {
        dp[i][1]=min(dp[i-1][1]+dist(_right[i-1],_left[i]),dp[i-1][0]+dist(_left[i-1],_left[i]))+dis[i-1];
        dp[i][0]=min(dp[i-1][1]+dist(_right[i-1],_right[i]),dp[i-1][0]+dist(_left[i-1],_right[i]))+dis[i-1];
    }

    cout<<min(dp[cnt][1],dp[cnt][0])+dis[cnt]<<endl;

    return 0;
}
//考场代码略丑
```

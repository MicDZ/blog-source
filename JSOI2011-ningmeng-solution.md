---
title: 【JSOI2011】柠檬 题解
date: 2019-10-24 12:41:43
tags: [题解,动态规划,斜率优化]
categories:
- OI
---
[JSOI2011](https://www.luogu.org/problem/P5504)

<!--more-->
# 核心思路

M_sea写的是真的好啊

设$\mathrm{dp}_i$为前$i$个贝壳可以得到的最多柠檬，转移方程显然：
$$
\begin{aligned}
\mathrm{dp}_i&=\max_{j=1}^i\{\mathrm{dp}_{j-1}+s_i(c_i-c_j+1)\}\\
&=\mathrm{dp}_{j-1}+s_ic_i^2+s_ic_j^2-2s_ic_ic_j+2s_ic_i-2s_ic_j+s_i
\end{aligned}
$$

按照斜率优化的套路，将含$i$，$j$分开，转化为一次函数，可以得到：
$$
m_i=s_ic_i^2+2s_ic_i+s_i,\ \ y=\mathrm{dp}_{i-1}+s_ic_i^2-2s_ic_i,\ \ k=2c_i,\ \ x=s_ic_i
$$

$$
\mathrm{dp}_i=y-kx+m_i
$$

答案就在这些点的上凸包上，用一个单调栈维护即可。

求大佬轻D。

# 完整代码


```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<vector>
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
 
const int MAXN=100000+10;
 
int s[MAXN],p[MAXN],c[MAXN],dp[MAXN];
 
int x(int i) {return s[i]*c[i];}
int y(int i) {return dp[i-1]+s[i]*c[i]*c[i]-2*s[i]*c[i];}
 
double calc1(int i,int j) {
    return 1.0*(y(i)-y(j))/(x(i)-x(j));
}
 
int calc2(int i,int j) {
    return dp[j-1]+s[i]*(c[i]-c[j]+1)*(c[i]-c[j]+1);
}
 
vector<int> stack[MAXN];
 
signed main() {
    int n=read();
    REP(i,1,n) s[i]=read(),c[i]=c[p[s[i]]]+1,p[s[i]]=i;
    REP(i,1,n) {
        while (stack[s[i]].size()>=2&&calc1(stack[s[i]][stack[s[i]].size()-2],i)>=calc1(stack[s[i]][stack[s[i]].size()-2],stack[s[i]][stack[s[i]].size()-1])) stack[s[i]].pop_back();
        stack[s[i]].push_back(i);
        while (stack[s[i]].size()>=2&&calc2(i,stack[s[i]][stack[s[i]].size()-1])<=calc2(i,stack[s[i]][stack[s[i]].size()-2])) stack[s[i]].pop_back();
        dp[i]=calc2(i,stack[s[i]].back());  
    }//上凸包
    printf("%lld\n",dp[n]);
    return 0;
}
```
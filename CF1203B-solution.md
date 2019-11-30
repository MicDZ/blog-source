---
title: 【CF1203B】Equal Rectangles 题解
categories:
  - OI
date: 2019-08-14 15:16:19
tags: [题解,数学]
---



Codeforces Round #579 (Div. 3)  B题

<!--more-->

# 题目大意

给定你$4*n$个小木条，让你检测是否能组成$n$个面积相等的长方形。

# 核心思路

考虑到组成长方形的问题，必须要有$2*n$组相同的小木条才能构成长方形。首先将序列排序，从小到大抽出奇数位的与偶数位的查看是否相等，然后存入一个$b$数组中。

对于$b$数组中的任意两个数字都可以构成一个以他们为宽和高的长方形。

考虑使长方形面积相等的问题。

由上述的推到可得出

$$
\forall b_i(i< n),b_i\leq b_{i+1}
$$

那么考虑$b_n$的搭配方式，如果$b_n$选择了$b_1$，那么组成的长方形面积则为$b_n\times b_1$，没有问题；如果选择$b_2$，那么组成的长方形为$b_n\times b_2$，此时的$b_1$ 无论与谁进行组合都不可能与$b_n\times b_2$相等。

所以首尾搭配是唯一的解决办法。如果首位依次向内不能保证乘积相等的话就没有对应的组合方法就输出 `NO`即可。

代码如下：

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
 
const int INF=0x3f3f3f,MAXN=10000+10;
 
int a[MAXN],b[MAXN];
 
int main() {
	int t=read();
	while(t--) {
		int n=read();
        REP(i,1,4*n) a[i]=read();
 
        sort(a+1,a+1+4*n);
        bool flag=0;
        int cnt=1;
        for(int i=1; i<=4*n-1; i+=2) {
            if(a[i]==a[i+1]) b[cnt++]=a[i];
            else flag=1;
        }
        int times=b[1]*b[2*n];
        REP(l,2,n) {
            int r=2*n-l+1;
            if(b[l]*b[r]!=times) flag=1;
        }
 
        if(flag) puts("NO");
        else puts("YES");
    }
	return 0;
}
 
```






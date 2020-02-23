---
title: 【CF1203A】Circle of Students 题解
categories:
  - [OI,题解]
date: 2019-08-14 15:16:02
tags: [题解,模拟]
---



Codeforces Round #579 (Div. 3)  A题

<!--more-->

## 题目大意

$n$个小朋友围坐一圈，要求从$1$号小朋友开始报数，依次以顺时针或逆时针报数，如能报到$n$则输出`YES`否则输出`NO`。

## 核心思路

首先想到的是线性的做法，从左到右扫一遍，要求前一个为后一个加一或减一，但是实现较复杂。后来想到可以直接模拟，找到编号为$1$ 的小朋友向左或向右找，找到一边有答案就记为`YES`，否则就是`NO`。

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
 
const int INF=0x3f3f3f,MAXN=200+10;
 
int p[MAXN];
 
int main() {
	int t=read();
	while(t--) {
		int n=read();
		REP(i,1,n) p[i]=read();
		bool flag1=0,flag2=0;
		
		int pos;
		REP(i,1,n) if(p[i]==1) pos=i;
		int cnt=1;
		REP(i,1,n-1) {
			int now=(i+pos)%n;
			if(now==0) now=n;
			cnt++;
			if(p[now]!=cnt) flag1=1;
			//cout<<now<<" ";
		}
	//puts("");
		cnt=n+1;
 
		REP(i,1,n-1) {
			int now=(pos+i+n)%n;
			if(now==0) now=n;
			cnt--;
			if(p[now]!=cnt) flag2=1;
		}
 
		if(!flag1||!flag2) puts("YES");
		else puts("NO"); 
	}
	return 0;
}
```




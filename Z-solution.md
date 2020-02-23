---
title: '【CJOJ】 甜点'
date: 2019-07-23 22:11:45
tags: [题解,动态规划,背包]
categories:
- [OI,题解]   
---

CJ NOIp模拟赛动态规划。

<!--more-->

## 题目描述

小 z 准备举办一个比赛。他需要提供一些甜点给参赛者来补充能量。每种甜品有一定的能量 $t_i$和大小 $u_i$，且每种甜点最多有$v_i$个。小 z 准备用箱子来包装甜点。箱子可以容纳一定体积的甜点且需要一定的费用。小 z有一种魔法，可以将一个甜点分成多份装在箱子里，最后再合在一起（但合成之后必须是完
整的一个）。小 z 想知道准备能量至少为 P 的甜点的最小大小和最少需要多少费用来购买箱子，如果最少费用超过小 z 所拥有的钱数 k 则输出` FAIL`。

## 核心思想

经过分析，对于糖的能量与箱子的容量都为完全背包。只需要对糖和箱子做两次完全背包即可。这里有一个问题是，我们并不知道糖的最大体积，我们可以假定其为2000（由具体题目数据范围而定），然后在$dp[1-2000]$中找到第一个大于所需能量的最小糖果体积。再将此数值记录下来用于下次的箱子的完全背包中。

## 完整代码

```cpp
//考场代码略丑
#include<bits/stdc++.h>
using namespace std;

#define ll long long

#define DREP(i,e,s) for(register ll i=e; i>=s; i--)
#define REP(i,e,s) for(register ll i=e; i<=s; i++)
#define file(a) freopen(a".in","r",stdin);freopen(a".out","w",stdout)


ll read() {
	ll x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0') {if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9') {x=x*10+ch-'0';ch=getchar();}
	return x*f;
}
const ll MAXN=6000+10,INF=0x3f3f3f3f3f3f3f3f;
ll w[MAXN][2],v[MAXN][2],l[MAXN][2],dp[MAXN*1000];
int main() {
	file("z");
	ll n=read(),m=n,t=20000,p=read(),q=p,power_need=read(),money_have=read();
	REP(i,1,n) {
		v[i][0]=read();
		w[i][0]=read();
		l[i][0]=read();
	}
	REP(i,1,p) {
		v[i][1]=read();
		w[i][1]=read();
		l[i][1]=read();
	}
	REP(i,1,n) {
		int maxx=log(l[i][0])/log(2);
		REP(j,1,maxx-1) {
			m++;
			w[m][0]=(1<<j)*w[i][0];
			v[m][0]=(1<<j)*v[i][0];
			//cout<<w[m]<<" "<<v[m]<<endl;
			l[i][0]-=(1<<j);
		}
		l[i][0]-=1;
		if(l[i][0]>0) {
			m++;
			w[m][0]=l[i][0]*w[i][0];
			v[m][0]=l[i][0]*v[i][0];		
			//cout<<w[m]<<" "<<v[m]<<endl;
		}
	}//用到了log的优化
	REP(i,1,m) DREP(j,t,w[i][0]) dp[j]=max(dp[j],dp[j-w[i][0]]+v[i][0]);
	ll ans=-INF;
	REP(i,1,t) if(dp[i]>=power_need) {printf("%d\n",i);ans=i;break;}
	int qerwer=ans;
	t=20000;

	n=m=p;
		REP(i,1,n) {
		int maxx=log(l[i][1])/log(2);
		REP(j,1,maxx-1) {
			m++;
			w[m][1]=(1<<j)*w[i][1];
			v[m][1]=(1<<j)*v[i][1];
			//cout<<v[m]<<" "<<w[m]<<endl;
			l[i][1]-=(1<<j);
		}
		l[i][1]-=1;
		if(l[i][1]>0) {
			m++;
			w[m][1]=l[i][1]*w[i][1];
			v[m][1]=l[i][1]*v[i][1];		
			//cout<<v[m]<<" "<<w[m]<<endl;
		}
	}
	memset(dp,0,sizeof(dp));

	REP(i,1,m) DREP(j,t,w[i][1]) dp[j]=max(dp[j],dp[j-w[i][1]]+v[i][1]);

	
	REP(i,1,t) {
		//printf("%d ",dp[i]);
		if(dp[i]>=qerwer) {ans=i;break;}
	}


	if(ans>money_have) puts("FAIL");
	else printf("%d\n",ans);
}
```

此题的自测数据[onedrive](https://1drv.ms/f/s!AmkbL8SRgVRrge86PjqOwD4nuFyLYA)。可能需要科学上网。
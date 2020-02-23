---
title: 【tyvj1061】Mobile Service 题解
categories:
  - [OI,题解]
date: 2019-12-10 12:48:18
tags: [动态规划,题解]
---

[tyvj1061](http://www.joyoi.cn/problem/tyvj-1061)

<!--more-->

## 核心思路

首先可以直接思考到的是设 $f_{i,a,b,c}$ 表示，进行到第 $i$ 个任务时，三个服务员分别在 $a,b,c$ 的最小花费。

这个的复杂度为 $O(NL^3)$ ，显然过不了。

然后就是比较套路的步骤了。发现记录的三个位置并不是都会用到，因为在完成了第 $i$ 个任务后，必定有一个服务员在 $P_i$，这样就可以优化到 $O(NL^2)$ 了。

然后发现卡空间，滚动数组优化下就可以了。

## 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>

using namespace std;

#define REP(i,e,s) for(register int i=(e); i<=(s); i++)
#define DREP(i,e,s) for(register int i=(e); i>=(s); i--)
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

const int MAXN=1000+1,MAXL=200+1,INF=0x3f3f3f3f;

int c[MAXL][MAXL],p[MAXN],f[2][MAXL][MAXL];

int main() {
	int l=read(),n=read();
	REP(i,1,l) REP(j,1,l) c[i][j]=read();
	REP(i,1,n) p[i]=read();
	
	int o=0;
	memset(f[o],127,sizeof(f[o]));
	p[0]=3;
	f[0][1][2]=0;

	REP(i,0,n) {
		o^=1;
		memset(f[o],127,sizeof(f[o]));
		REP(x,1,l) REP(y,1,l) {
			f[o][x][y]=min(f[o][x][y],f[o^1][x][y]+c[p[i]][p[i+1]]);
			f[o][p[i]][y]=min(f[o][p[i]][y],f[o^1][x][y]+c[x][p[i+1]]);
			f[o][x][p[i]]=min(f[o][x][p[i]],f[o^1][x][y]+c[y][p[i+1]]);
		}
	}

	int ans=INF;

	REP(i,1,l) REP(j,1,l) {
		if(i==j) continue;
		ans=min(ans,f[o][i][j]);
	}

	DE("%.2f MB\n",(sizeof(c)+sizeof(p)+sizeof(f))*1.0/1048576);
	printf("%d\n",ans);
	return 0; 
}

```
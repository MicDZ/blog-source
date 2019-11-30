---
title: 【CF525D】Arthur and Walls 题解
categories:
  - OI
date: 2019-09-23 12:26:06
tags: [题解,数学]
---

Codeforces Round #297 (Div. 2) D题

<!--more-->

虽然是在贪心题的集训里面的，但是这题好像和贪心没有什么关系。

# 题目大意

给出一个$n\times m$的矩阵，里面有星号和点两种符号，要求把最少的星号变成点，使得点的联通块构成一个矩形。求最少需要变几个星号。

### 

# 核心思路

直接寻找矩形、合并是非常困难的。相交重叠的部分不好统计，并且每合并两个矩形还有可能影响到此前已经合并好的矩形。~~玄学合并也许可以卡过去~~

正解非常之不好想，思路相当巧妙。对于每一个星号，当它的周边有三个点，时，这个星号就必须要改为点。这样修改下去，直到没有一个星号的周边有三个点。

考虑如何修改，因为每修改一个星号为点，都只会对这个点上下左右四个方向的8个点产生影响，bfs的时候把上下左右四个方向丢进队列即可。

# 完整代码

```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<queue>
using namespace std;

#define REP(i,e,s) for(register int i=e; i<=s; i++)
#define DREP(i,e,s) for(register int i=e; i>=s; i--)
#define ll long long
#define DE(...) fprintf(stderr,__VA_ARGS__)
#define DEBUG(a) DE("DEBUG: %d\n",a)
//#define int ll

int read() {
	int x=0,f=1,ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

const int MAXN=2000+10;

char a[MAXN][MAXN];

queue<pair<int,int> > q;
int n,m;
int tx[]={0,1,-1,1,-1};
int ty[]={0,-1,1,1,-1};

void bfs() {
	while(!q.empty()) {
		int x=q.front().first,y=q.front().second;
		q.pop();
		REP(i,1,4) {
			int xx=x+tx[i],yy=y+ty[i];
			if(xx>n||yy>m||xx<1||yy<1) continue;
			if(a[x][yy]+a[xx][y]+a[xx][yy]+a[x][y]==3*'.'+'*') {
				if(a[x][yy]=='*') q.push(make_pair(x,yy)),a[x][yy]='.';
				if(a[xx][y]=='*') q.push(make_pair(xx,y)),a[xx][y]='.';
				if(a[xx][yy]=='*') q.push(make_pair(xx,yy)),a[xx][yy]='.';
			}
		}
	}
}

int main() {
	n=read(),m=read();
	REP(i,1,n) {
		REP(j,1,m) a[i][j]=getchar();
		getchar();
	}
	
	REP(i,1,n) REP(j,1,m) 
		if(a[i][j]=='.') q.push(make_pair(i,j));
	
	bfs();

	REP(i,1,n) {REP(j,1,m) printf("%c",a[i][j]); puts("");}
}
```


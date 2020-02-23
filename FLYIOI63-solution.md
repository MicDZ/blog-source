---
title: '【FLYIOI63】逃离 题解'
date: 2018-11-03 20:03:23
tags: [题解,bfs,图论,搜索]
categories:
- [OI,题解]   
---

题目链接[here](http://lxg.yali.edu.cn:8080/problem/63)

这是一道非常有思维难度和代码强度的宽搜好题。

<!--more-->
## 题目大意

在一个地图中有两个人物和一些障碍物。

小z具有技能急速:每次移动可以走两步(允许这两步的方向不同,也允许不走第二步);

小m具有技能跳跃:若移动的方向为障碍且障碍后为空地,可借助障碍直接跳跃至空地。

计算两人相遇最少要花费多少秒。

![](https://i.loli.net/2018/11/08/5be39f813bd58.png)

如上图所示，黄色表示可以到达区域，黑色表示障碍物。

## 核心思路

通过数据范围可以发现此题的$n\leq 1000$，就可以进行广搜，并且留下了可观的常数范围。

### 广搜小z

#### 分解小z技能

小z的技能相当于在一秒内运用两次常规操作。因此我们只需要对小z做两次push即可。

#### 小z的搜索代码
```cpp
void bfs1(int xs,int ys) {
	int tx[5]={0,1,0,-1,0};
	int ty[5]={0,0,1,0,-1};
	queue<edge2> q;
	q.push((edge2){0,xs,ys});
	have[xs][ys]=1;
	z[xs][ys]=0;
	while(!q.empty()) {
		int x=q.front().xq,y=q.front().yq,s=q.front().s;
		q.pop();
		if(a[x][y]) continue;
		for(int i=1; i<=4; i++) {
			int xx=x+tx[i],yy=y+ty[i];

			if(xx<1||xx>n||yy<1||yy>n||a[xx][yy]) continue;
			if(!have[xx][yy]) {
				have[xx][yy]=1;
				z[xx][yy]=s+1;//答案存在z数组中
				q.push((edge2){s+1,xx,yy});//跳第一次到达的点
			}
			for(int j=1; j<=4; j++) {
				int xr=xx+tx[j],yr=yy+ty[j];
				if(xr<1||xr>n||yr<1||yr>n||a[xr][yr]||have[xr][yr]) continue;//如果中间的这个点是障碍物则无法运用技能
				have[xr][yr]=1;
				z[xr][yr]=s+1;//答案存在z数组中
				q.push((edge2){s+1,xr,yr});//运用技能调达的点
			}
		}
	}
}

```

### 广搜小m

#### 分解小m技能

小m的技能相当于如果常规操作跳到一个障碍物上，那么可以在跳跃方向上继续向前跳一格，但是要判断前面的这一格是否为障碍物。

#### 小z的搜索代码
```cpp
void bfs1(int xs,int ys) {
	int tx[5]={0,1,0,-1,0};
	int ty[5]={0,0,1,0,-1};
	
	queue<edge> q;
	q.push((edge){xs,ys});
	while(!q.empty()) {
		int x=q.front().xq,y=q.front().yq;

		q.pop();
		for(int i=1; i<=4; i++) {
			int xx=x+tx[i],yy=y+ty[i];
			if(have[xx][yy]||xx<1||xx>n||yy<1||yy>n) continue;
			
			if(a[xx][yy]) {
				if(i==1) xx++;
				if(i==2) yy++;
				if(i==3) xx--;
				if(i==4) yy--;
			}//如果xx,yy是障碍物则跳过障碍物，针对不同的来路要跳到指定的地方
			
			if(have[xx][yy]||xx<1||xx>n||yy<1||yy>n) continue;
			
			if(!a[xx][yy]) {
				m[xx][yy]=min(m[xx][yy],m[x][y]+1);//答案存储在m数组中
				q.push((edge){xx,yy});
				have[xx][yy]=1;
			}
		}
		
	}
}

```

### 统计答案

$\Theta(nm)$扫整个图一遍，找到$\max(z_{i,j},m_{i,j})$最小的点。
```cpp
	for(int i=1; i<=n; i++)
		for(int j=1; j<=n; j++) {
			ans=min(ans,max(m[i][j],z[i][j]));
		}
```

## 完整代码
```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAXN 1000+10
#define INF 0x3f3f3f3f
int read() {
	int x=0,f=1;
	char ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

int n,xa,xb,ya,yb;

int a[MAXN][MAXN];

int z[MAXN][MAXN],m[MAXN][MAXN];


void debug1() {
	cout<<endl;
	for(int i=1; i<=n; i++) {
		for(int j=1; j<=n; j++) {
			printf("%3d",m[i][j]==INF?0:m[i][j]);
		}
		cout<<endl;
	}
}


void debug2() {
	cout<<endl;
	for(int i=1; i<=n; i++) {
		for(int j=1; j<=n; j++) {
			printf("%3d",(z[i][j]==INF?0:z[i][j]));
		}
		cout<<endl;
	}
}

struct edge{
	int xq,yq;
};

int have[MAXN][MAXN];

void bfs1(int xs,int ys) {
	int tx[5]={0,1,0,-1,0};
	int ty[5]={0,0,1,0,-1};
	
	queue<edge> q;
	q.push((edge){xs,ys});
	while(!q.empty()) {
		int x=q.front().xq,y=q.front().yq;

		q.pop();
		for(int i=1; i<=4; i++) {
			int xx=x+tx[i],yy=y+ty[i];
			if(have[xx][yy]||xx<1||xx>n||yy<1||yy>n) continue;
			
			if(a[xx][yy]) {
				if(i==1) xx++;
				if(i==2) yy++;
				if(i==3) xx--;
				if(i==4) yy--;
			}
			
			if(have[xx][yy]||xx<1||xx>n||yy<1||yy>n) continue;
			
			if(!a[xx][yy]) {
				m[xx][yy]=min(m[xx][yy],m[x][y]+1);
				q.push((edge){xx,yy});
				have[xx][yy]=1;
			}
		}
		
	}
}

struct edge2{
	int s,xq,yq;
};

void bfs2(int xs,int ys) {
	int tx[5]={0,1,0,-1,0};
	int ty[5]={0,0,1,0,-1};
	queue<edge2> q;
	q.push((edge2){0,xs,ys});
	have[xs][ys]=1;
	z[xs][ys]=0;
	while(!q.empty()) {
		int x=q.front().xq,y=q.front().yq,s=q.front().s;
		q.pop();
		if(a[x][y]) continue;
		for(int i=1; i<=4; i++) {
			int xx=x+tx[i],yy=y+ty[i];

			if(xx<1||xx>n||yy<1||yy>n||a[xx][yy]) continue;
			if(!have[xx][yy]) {
				have[xx][yy]=1;
				z[xx][yy]=s+1;
				q.push((edge2){s+1,xx,yy});
			}
			for(int j=1; j<=4; j++) {
				int xr=xx+tx[j],yr=yy+ty[j];
				if(xr<1||xr>n||yr<1||yr>n||a[xr][yr]||have[xr][yr]) continue;
				have[xr][yr]=1;
				z[xr][yr]=s+1;
				q.push((edge2){s+1,xr,yr});
			}
		}
	}
}

int main() {
	n=read(),xa=read(),ya=read(),xb=read(),yb=read();
	for(int i=1; i<=n; i++) {
		for(int j=1; j<=n; j++) a[i][j]=getchar()-'0';
		getchar();
	}
	for(int i=1; i<=n; i++)
		for(int j=1; j<=n; j++) 
			m[i][j]=INF,z[i][j]=INF;
	m[xb][yb]=0;
	bfs1(xb,yb);
	
	for(int i=1; i<=n; i++)
		for(int j=1; j<=n; j++) 
			have[i][j]=0;
	z[xa][ya]=0;
	bfs2(xa,ya);
	//debug1();
	//debug2();
	int ans=INF;
	for(int i=1; i<=n; i++)
		for(int j=1; j<=n; j++) {
			ans=min(ans,max(m[i][j],z[i][j]));
		}
	cout<<ans<<endl;
}
```

# 小结

此题码量较大，细节较多。调试较困难，一遍AC较难。

但是是一道练习广搜的好题。
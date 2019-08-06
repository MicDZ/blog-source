---
title: SPFA深入解析
date: 2018-05-28 22:19:30
id: 'SPFA'
tags: [随笔,笔记,图论]
categories:
- OI   
---



SPFA是一种高效的单源最短路径算法。

<!--more-->

# 算法简述

1. 读入每一条边的权值到**邻接表**
3. 将最短路径数组初始化为INF
4. 将起点加入队列
5. 原点到原点路径设为0
6. 从队列取出首元素
7. 广度优先遍历出边
8. 进行松弛操作，如可进行则将出边对应点加入队列
8. 用一个数组表示i是否已入队

这是一个单源最短路径算法（废话）

# 正确性证明
其实只需证明SPFA的遍历过程是收敛的，可以看看段凡丁的介绍。




# 算法复杂度分析

经过长篇的分析与介绍，段凡丁在原文中给出了

$$
\begin{aligned}
T=&\Theta(\frac{m}{n}\cdot e)\\
=&\Theta(k\cdot e)\\
\end{aligned}
$$
所以得出结论：
$$
\begin{aligned}
T=&\Theta(e)
\end{aligned}
$$



你不禁大叫：“假的吧！”。这里的$m,n$指的不是点数和边数，$m$其实是外层while的循环次数。

没错，按照正常的推理逻辑的确可以得出这个结论。

但是$\frac{m}{n}$完全可以很大，也就是$n$要远小于$m$时也就是一个稠密图时SPFA的复杂度竟然可以无限地趋近于$\Theta(nm)$。这样就远远不如dijkstra算法了（实际上还有各种各样的常数）。

Ps：论文里的说法太不负责任了。



# 实现

下面以是[热浪](https://www.luogu.org/problemnew/show/P1339)为例题的代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAXN 10000+10
#define MAXM 65000+10
#define INF 0x3f3f3f

struct edge{
	long long to,next,v;
}e[MAXM*2];

long long cnt,last[MAXN];;
long long n,m,s,t;
void add(long long u,long long v,long long w){
	cnt++;
	e[cnt].to=v;
	e[cnt].v=w;
	e[cnt].next=last[u];
	last[u]=cnt;
}

void spfa(){
	long long inq[MAXN],d[MAXN];
	queue<long long> q;
	memset(inq,0,sizeof(inq));
	for(long long i=1;i<=n;i++)
		d[i]=INF;

	d[s]=0;inq[s]=1;
	q.push(s);
	while(!q.empty()){
		long long u=q.front();
		q.pop();
		inq[u]=0;
		for(long long i=last[u];i>=0;i=e[i].next){
			long long v=e[i].to,w=e[i].v;
			if(d[v]>d[u]+w){
				d[v]=d[u]+w;
				if(!inq[v]){
					inq[v]=1;
					q.push(v);
				}
			}
		}
	}
	cout<<d[t]<<endl;
}
long long read(){
	long long x=0,f=1;
	char ch=getchar();
	while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
	while(ch<='9'&&ch>='0'){x=x*10+ch-'0';ch=getchar();}
	return x*f;
}

int main(){
	memset(last,-1,sizeof(last));
	n=read(),m=read(),s=read(),t=read();
	for(long long i=1; i<=m; i++){
		long long u=read(),v=read(),w=read();
		add(u,v,w);
		add(v,u,w);
	}
	spfa();
}
```



_References:_

1. Duan, Fanding (1994), ["关于最短路径的SPFA快速算法"](http://wenku.baidu.com/view/3b8c5d778e9951e79a892705.html), _西南交通大学学报_, **29** (2): 207–212
2. [SPFA算法wiki](https://en.wikipedia.org/wiki/Shortest_Path_Faster_Algorithm)



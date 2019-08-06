---
title: 基础数据结构
date: 2018-05-13 22:26:53
tags: [笔记]
categories:
- OI   
---

数据结构在OI中可以很大程度降低空间和时间复杂度。

<!--more-->

# 前缀和差分

## 1. 基础

1. ds=data structures 数据结构
2. 如无特殊说明，序列长度和操作次数同阶
3. 默认的数据范围为10^5

### 2. 定义

1. 数组$a_1...a_n$ ，设$b_i = a_1+a_2+…+a_i$，则b是a的前缀和
2. $a_i= b_i-b_i-1$ ; 就称a是b的差分

### 3. 经典应用

**Exam1 **

给出序列，n次询问区间和。

1. 直接模拟O(n²)
2. 求出原序列的前缀和，区间和就是两个前缀和的差。$b_i = a_1 + a_2 + … + a_i ; b_i-n = a_1 + a_2 + … + a_i-x$。所以$b_i - b_i-1 =  a_i-x+1+…+a_i$。$sum( l ,r) = pre( r ) - pre(l - 1)$
3. 求解前缀和递推思路：$b_i=b_i-1+a_i$   时间复杂度:O(n) 

**Exam2**

给出序列，n次区间加

1. 直接模拟O(n²)
2. 求出原序列的差分，区间加就是加减端点
3. $[l  , r ]$加上$x -> dif [l]+ = x$; $ dif [r + 1]- = x$
4. 最后对差分数组求前缀和，得到结果


**Exam3**

给出一棵树，每次询问一条路径上边权的和

1. 直接模拟O(n²)
2. 对于每个点，求出根距（到根的距离）d[ i ]等于父亲的深度+与父亲的边权（树上前缀和）。
3. $dis( u , v )=dis( u , LCA( u , v ) ) - dis( v ,  LCA( u , v ) ) = dis( u , v ) = d( u ) + d( v ) - 2d( LCA(u , v) )$

![EsT7A.png](https://s1.ax2x.com/2018/03/11/EsT7A.png) 

**Exam4**

P2420 让我们异或吧。树，询问路径边权异或和

1. $dis( x , y ) = d[ x ]⊕d[ y ]$
2. 甚至连LCA都不用求


**Exam5**

直线上n个区间，求被覆盖次数最多的点的被覆盖次数

1. 将区间端点离散化
2. 变为区间加问题
3. 求出最大值


**Exam6**

给出序列，求出最长的区间，满足区间和为7的倍



# 树状数组

## 1. 基础

* Binary Indexed  Tree(B.I.T), Fenwick Tree

 ![树状数组](https://s1.ax2x.com/2018/03/11/EsFGu.png)

## 2. 优势

更新一个点，至多修改$\Theta(\log n)$个段，求一个前缀和，之多只需要查询$\Theta(\log n)$个段

背代码：

```cpp
	inline int lowbit(int x) {return x&-x;}
	void add(int x,int k){// a[x] += k
		for(int i=x;i<=n;i+=lowbit(i))c[i]+=k;
	}
	int sum(int x){ // a[1] + a[2] + ... + a[x]
		int ret=0;
		for(int i=x;i>0;i-=lowbit(i))ret+=c[i];
		return ret;
}
```



## 3. 应用

1. Exam1:单点修改，前缀和查询
2. Exam1:单点修改，区间查询（要求可逆性）
3. Exam3:区间修改，单点查询（利用差分）
4. Exam4:区间修改，区间查询（较复杂，可查询相关资料）

 时间复杂度$\Theta(\log n)$



# 线段树

![](https://s1.ax2x.com/2018/03/11/Euc9R.png)



* 建立如图的二叉树
* 每个节点存储一段的信息（最大值）
* 修改至多更新$\Theta(\log n)$个节点
* 查询至多询问$\Theta(\log n)$个节点

hzy大佬的代码:[【模板】线段树1](https://www.luogu.org/problemnew/show/P3372)

```cpp
#include<cstdio>
using namespace std;
long long n,m,i,t,x,y,z,a[400010],sum,c[400010];
void js(long long h,long long l,long long r)
{
	long long x,mid=(l+r)/2;
	if(l==r)
	{
		scanf("%lld",&x);
		a[h]=x;
	}
	else
	{
		js(h*2,l,mid);
		js(h*2+1,mid+1,r);
		a[h]=a[h*2]+a[h*2+1];
	}
}
void work(long long h,long long l,long long r,long long L,long long R,long long k)
{
	long long mid=(l+r)/2;
	if(L<=l && r<=R)
		a[h]+=k*(r-l+1),c[h]+=k;
	else
	{
		if(c[h]>0)
		{
			c[h*2]+=c[h];
			c[h*2+1]+=c[h];
			a[h*2]+=c[h]*(mid-l+1);
			a[h*2+1]+=c[h]*(r-mid);
			c[h]=0;
		}
		if(L<=mid)
			work(h*2,l,mid,L,R,k);
		if(mid<R)
			work(h*2+1,mid+1,r,L,R,k);
		a[h]=a[h*2]+a[h*2+1];
	}
}
void find(long long h,long long l,long long r,long long L,long long R)
{
	long long mid=(l+r)/2;
	if(L<=l && r<=R)
		sum+=a[h];
	else
	{
		if(c[h]>0)
		{
			c[h*2]+=c[h];
			c[h*2+1]+=c[h];
			a[h*2]+=c[h]*(mid-l+1);
			a[h*2+1]+=c[h]*(r-mid);
			c[h]=0;
		}
		if(L<=mid)
			find(h*2,l,mid,L,R);
		if(mid<R)
			find(h*2+1,mid+1,r,L,R);
	}
}
int main()
{
	scanf("%lld%lld",&n,&m);
	js(1,1,n);
	for(i=1;i<=m;i++)
	{
		scanf("%lld",&t);
		if(t==1)
		{
			scanf("%lld%lld%lld",&x,&y,&z);
			work(1,1,n,x,y,z);
		}
		if(t==2)
		{
			scanf("%lld%lld",&x,&y);
			sum=0;
			find(1,1,n,x,y);
			printf("%lld\n",sum);
		}
	}
}
```




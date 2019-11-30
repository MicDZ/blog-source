---
title: 线段树进阶
date: 2018-11-08 16:47:03
tags: [笔记,随笔,数据结构]
categories:
- OI   
---

在[上篇](https://www.micdz.cn/article/segment-tree/)文章中，已经进行了详细的对线段树单点修改和区间查询的描述。

在这篇文章中将会更深入的了解线段树的区间修改。不过，在NOIp赛事中，几乎很少出现。

> 这是一篇原始文章，不保证内容的正确性

<!--more-->

# 核心思路

在上篇文章中，已经证明了线段树的区间修改的时间复杂度是$\Theta(\log n)$的，可以以此为思路研究区间修改的方法。

试想，如果我们在一次区间修改的操作中一次性将其子树的所有节点全部更新，而后面的查询操作根本不会用到这些节点，那么这些修改就完全浪费了。

我们在修改操作的时候对$p$加入一个标记，标识“该节点曾经修改，但其子节点尚未被跟新”，每一次查询操作的时候再将该标记向下传递。

我们称这种标记为“延迟标记”或“懒惰标记”。这就运用到了线段树优秀的性质。

接下来，我们以POJ3468为例，了解区间修改的线段树。

# 具体实现

建树、查询、修改的框架保持不变，用`spread`函数实现向下传递。

## 建树

用与上一篇一样，但是我们可以用几个宏定义的方法减少代码量。

```cpp
struct SegmentTree {
	int l,r;
	ll sum,add;
	#define l(x) tree[x].l
	#define r(x) tree[x].r
	#define sum(x) tree[x].sum
	#define add(x) tree[x].add
} tree[MAXN<<2];
```

利用上述的宏定义可以很方便地访问线段树。

建树过程

```cpp
void build(int p,int l,int r) {
	l(p)=l,r(p)=r;
	if(l==r) {
		sum(p)=a[l];
		return ;	
	}
	int mid=(l+r)>>1;
	build(p*2,l,mid);
	build(p*2+1,mid+1,r);
	sum(p)=sum(p*2)+sum(p*2+1);
}
```
## 传递标记

根据题目的维护内容改写。

```cpp
void speard(int p) {
	if(add(p)) {
		sum(p*2)+=add(p)*(r(p*2)-l(p*2)+1);//更新左字节点和
		sum(p*2+1)+=add(p)*(r(p*2+1)-l(p*2+1));//更新右子节点和
		add(p*2)+=add(p);//更新标记
		add(p*2+1)+=add(p);
		add(p)=0;//切记不能忘记
	}
}
```

## 区间修改

思路算是比较简单，但是有许多细节容易忘记导致莫名RE或TLE。

```cpp
void change(int p,int l,int r,int d) {
	if(l<=l(p)&&r>=r(p)) {//完全覆盖
		sum(p)+=d*(r(p)-l(p)+1);//这里是区间长度
		add(p)+=d;//只更新当前节点信息
		return ;
	}
	spread(p);//向下传递
	int mid=(l(p)+r(p))>>1;
	if(l<=mid) change(p*2,l,r,d);//左区间覆盖
	if(r>mid) change(p*2+1,l,r,d);//右区间覆盖
	sum(p)=sum(p*2)+sum(p*2+1);
}
```

## 区间查询

与上一篇所描述的方法几乎相同，只不过每一次查询都需要花一点点时间向下传递标志。

```cpp
ll ask(int p,int l,int r) {
	if(l<=l(p)&&r>=r(p)) return sum(p);
	spread(p);
	ll val=0;
	if(l<=mid) val+=ask(p*2,l,r);
	if(r>mid) val+=ask(p*2+1,l,r);
	return val;
}
```

# 小结

在NOIp赛事中，使用线段树、数状数组、平衡树等高级的数据结构一般是在T3或毒瘤的T2，因此掌握好这些数据结构有助于我们在考场上快速想到正解。



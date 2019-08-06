---
title: 线段树
date: 2018-10-11 23:28:35
tags: [笔记,随笔,数据结构]
categories:
- OI   
---

线段树是一种基于分治思想的二叉树结构。用于在区间上进行信息统计。于树状数组相比，线段树更为通用。

<!--more-->

# 实现目标

1. 区间修改
2. 区间查询
3. 可以基于所有具有可互逆运算法则的运算。

# 核心思路

## 线段树的建立

![](https://i.loli.net/2018/10/12/5bc0c17fabd0b.jpg)

在[基础数据结构](https://www.micdz.cn/article/basic-ds/)一文中，我有提到线段树是如上的结构。更为准确地描述，应该是一个二叉树。上图也可以很好地表现这一点。

那么就可以使用存储二叉树的方式来存储线段树。

```cpp
struct SegmentTree{
    int l,r;
    int dat;
} t[MAXN<<2];
```

也可以使用链式前向星或是邻接表。但是因为已经是二叉树的结构就没有必要了。

这里我们以查找最大值为例。其实任何有逆运算及结合律的操作都可以用线段树维护，这是因为线段树在合并时需要从左子树及右子树统计答案需要满足结合律，如果没有逆运算则无法更新答案。

每一个节点代表的区间是从根节点的整个长度出发依次分半直至不能再分为止。因此线段树的每一个节点都表示一个区间的最大值，特别的每一个叶子节点就表示一个数（其实一个数也可以是一个区间）。

```cpp
void build(int p,int l,int r) {
    t[p].l=l,t[p].r=r;
    if(l==r) {
        t[p].dat=a[l];
        return ;
    }//叶子节点的情况
    int mid=(l+r)/2;
    build(p*2,l,mid);//左子树
    build(p*2+1,mid+1,r);//右子树
    t[p].dat=max(t[p*2].dat,t[p*2+1].dat);//更新答案
}
```



## 单点修改

与建树十分相似。但是我们只需要找到$x$的位置即可。

```cpp
void change(int p,int x,int v) {
    if(t[p].l==t[p].r) {
        t[p].dat=v;
        return ;
    }//找到叶节点并修改
	int mid=(t[p].l+t[p].r)/2;
    if(x<=mid) change(p*2,x,v);
    else change(p*2+1,x,v);//在左区间还是右区间
    t[p].dat=max(t[p*2].dat,t[p*2+1].dat);//更新答案
}
```

调用入口为，将第$x$个节点更改为$v$。

```cpp
change(1,x,v);
```

## 区间查询

区间查询分为以下三种情况：

1. 所查找的子区间将当前节点区间完全包含，直接得出答案
2. 所查找的子区间与当前节点的左子节点有重叠
3. 所查找的子区间与当前节点的右子节点有重叠

对应三种情况我们可以用三个`if`语句解决。

```cpp
int ask(int p,int l,int r) {
    int mid=(t[p].l+t[p].r)/2;
    int val=-INF;
    if(l<=t[p].l&&r>=t[p].r) return t[p].dat;
	if(l<=mid) val=max(val,ask(p*2,l,r));
    if(r>mid) val=max(val,ask(p*2+1,l,r));
    //切记此处不能写为else if
}
```

调用入口为，返回值为答案。

```cpp
ask(1,l,r);
```

考虑查询操作的时间复杂度：

查询操作会将区间$[l.r]$在线段树上分为$\log n$个节点。但是因为存在左子区间或右子区间有重叠的情况有可能会向下查找两次。但好在这种情况最多出现一次。

因此查询操作的复杂度为$\Theta(2\log n)=\Theta(\log n)$。



有关线段树的更多操作，请参考[线段树进阶](https://www.micdz.cn/article/segment-tree-pro/)
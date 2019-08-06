---
title: 最近公共祖先(LCA)
date: 2018-07-27 11:13:57
tags: [笔记,随笔,图论]
categories:
- OI   
---



LCA最近公共祖先解决的是：对于有根树T的两个结点u、v，最近公共祖先LCA(T,u,v)表示一个结点x，满足x是u、v的祖先且x的深度尽可能大。

<!--more-->

通俗地讲就是下面的这个例子：

![图片来自默思·朸安](https://www.micdz.cn/img/2018-7-27-2.png)

在上图中10和12的LCA就是3。

# 核心思路

## 模拟法

根据题意进行模拟。一步一步回向根节点递进，分别对到达的顶点标号，碰到的第一个两个都到达过的顶点即为两点的LCA了。

这样的做法非常自然，但是绝对不可能符合这一道题所要求的时间复杂度。

## 倍增法

仍旧是按照题意进行模拟，不同的是，这次不再一次一次向上找，这次以2的倍数从上向下找。

具体的操作步骤很简单，首先将两节点调到保证$deep[X]>=deep[Y]$，如果这个时候两点是同一点则可以直接出答案，如果不行则将$X$从上向下跳。为什么要从上往下跳呢？

![](https://www.micdz.cn/img/2018-7-27-1.png)

通过上面的这个例子可以看出，如果从下向上的话，有可能跳到的那一个点不是最近公共祖先，而从上向下可以在找到后继续向下。



# 代码分解

$p[i][j]$ 表示节点$i$的$2^j$级祖先。

## dfs求深度

```cpp
void dfs(int u,int fa) {//u表示当前节点，fa表示它的父亲节点
    d[u]=d[fa]+1;//深度比它的父亲要大1
    p[u][0]=fa;
    for(int i=1; (1<<i)<=d[u]; i++)
        p[u][i]=p[p[u][i-1]][i-1];//转移
	for(int i=head[u];i;i=next[i]) {
        int v=to[i];
        if(v!=fa)//v不是父亲节点
            dfs(v,u);//从根节点出发进行dfs
    }
}
```

上述的状态转移的含义是：
$u$的$2^i$个祖先和$u$的$2^{i-1}$个祖先的$2^{i-1}$是同一个祖先

数学逻辑来讲就是$2^i=2\times2\times\ldots\times2\times2$(共有i个2)

而$2^{i-1}=2\times2\times\ldots \times2\times2$(共有i-1个2)

根据乘法分配律，得出$2^{i-1}+2^{i-1}=2^i$并且要满足$2^i\leq d[u]$，其$d[u]$是该节点的深度。

所以上面的含义也就成立，可以得出每一个节点的任意的祖先（这里没有边权的概念）。



## ST求LCA

```cpp
int lca(int a,int b) {
    if(d[a]>d[b]) swap(a,b);
    for(int i=20; i>=0; i--)
        if(d[a]<=d[b]-(1<<i))
            b=p[b][i];
    if(a==b) return a;//如果直接找到答案就可以输出了，此时一定是最近的解
    
    for(int i=20; i>=0; i--) {
        if(p[a][i]==p[b][i]) continue;//得到第一个答案，但不确定是不是最近所以continue，继续
        else {
            a=p[a][i];//将a往下跳
            b=p[b][i];//将b往下跳
        }
    }
    return p[a][0];//经过20轮操作后即可得到最终答案，也等价于p[b][0]

}
```

这一步的操作也比较简单。首先必须确保$a$的深度要小于或等于$b$的深度。然后将深度大的$b$由上向下跳，可以说是从$2^{20}$开始逐步向下（这样可以有效降低代码难度而对程序运行几乎没有影响）。直到$b$刚刚跳到了$a$的上面。此时$a$与$b$的LCA就一定在$a$和$b$之间了。接下来的操作与刚才的非常相似，同样是进行20次“徘徊”，详细的内容见上方注释。



# 完整代码

```cpp
#include<bits/stdc++.h>
#define MAXN 500000+10

using namespace std;

int head[MAXN<<1],to[MAXN<<1],next[MAXN<<1],cnt=0;

int n,m,s;

void addedge(int u,int v) {
    cnt++;
    to[cnt]=v;
    next[cnt]=head[u];
    head[u]=cnt;
}

int read() {//快读略
}

int d[MAXN],p[MAXN][21];

void dfs(int u,int fa) {
    d[u]=d[fa]+1;
    p[u][0]=fa;
    for(int i=1; (1<<i)<=d[u]; i++)
        p[u][i]=p[p[u][i-1]][i-1];
	for(int i=head[u];i;i=next[i]) {
        int v=to[i];
        if(v!=fa)
            dfs(v,u);
    }
}

int lca(int a,int b) {
    if(d[a]>d[b]) swap(a,b);
    for(int i=20; i>=0; i--)
        if(d[a]<=d[b]-(1<<i))
            b=p[b][i];
    if(a==b) return a;
    
    for(int i=20; i>=0; i--) {
        if(p[a][i]==p[b][i]) continue;
        else {
            a=p[a][i];
            b=p[b][i];
        }
    }
    return p[a][0];

}
int main() {

	memset(head,-1,sizeof(head));
    
    n=read(); m=read(); s=read();
    for(int i=1; i<=n-1; i++) {
        int x,y;
        x=read(),y=read();
        addedge(x,y);
        addedge(y,x);//树是无向图
    }
    dfs(s,0);
    for(int i=1; i<=m; i++) {
        int a=read(),b=read();
        printf("%d\n",lca(a,b));
    }
    return 0;
}

```


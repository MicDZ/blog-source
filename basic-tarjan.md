---
title: 浅析tarjan算法
date: 2018-11-02 16:18:30
tags: [随笔,图论,强连通分量]
categories:
- OI   
---

几乎网上所有有关tarjan算法的介绍都会有下面这一段话：

Tarjan老爷子一生发明了许多算法下到 NOIP 上到 CTSC 难度的都有。

(Tarjan 算法，并查集，Splay 树，Tarjan 离线求 lca)

我们这里要介绍的是图论中的 Tarjan 算法，用来处理各种连通性相关的问题。

<!--more-->

# 有向图连通性

## 一些定义

1. 给定有向图$G=(V,E)$，若存在$r\in V$，满足从$r$出发能到达$V$中的所有点，则称$G$为一个“流图”，其中$r$称为流图的源点。

2. 在流图的深度优先遍历中，按照每个节点第一次被访问的顺序，以此给予$N$个节点$1-n$的整数标记，该标记就称为$dfn[x]$，中文名为“时间戳”。

3. 给定一张有向图，若对于图中任意两个节点$x$,$y$，既存在$x$到$y$的路径，又存在从$y$到$x$的路径，则称该有向图强连通。

4. 强连通分量在有向图G中，如果两个顶点$x$,$y$间$（x>y）$有一条从$x$到$y$的有向路径，同时还有一条从$x$到$y$的有向路径。

如下图：
![](https://i.loli.net/2018/11/01/5bdac3208567e.png)

此图是一个非常经典的例图

在图中，$\{1,2,3\}$在同一个强连通分量，$\{4\}$和$\{5\}$单独两个强连通分量。

6. 在强连通图中各种类型的边，定义边$(x,y)$满足以下条件。

>树枝边，指搜索树中的边，即$x$是$y$的父节点
>
>前向边，$x$是$y$的祖先节点
>
>后向边，$y$是$x$的祖先节点
>
>横叉边，除了以上条件的所有边，这种边一定满足$dfn[y]<dfn[x]$

## Tarjan求强连通分量

### 核心思路

#### 初始化

在刚刚遍历到某一个节点时初始化$low$和$dfn$。

而为什么此时的$low$可以等于$dfn$，请自行证明。

对于上面的那一张图，转化成一个流图即为

![](https://i.loli.net/2018/11/09/5be4eb8e452a0.png)

#### 利用栈存储

每次到达一个节点时，就将该节点压入栈中，并且记录下该节点已入栈。

当某一节点的出边的$low==dfn$时，就将栈顶推出直至某一节点与栈顶相同为止，此时推出的所有节点均为同一强连通分量的节点，并且没有其他节点与此强连通。

### 完整代码

```cpp
void tarjan(int x) {
    dfn[x]=low[x]=++num;
    _stack[++top]=x;
    ins[x]=1;
    for(int i=head[x]; i; i=_next[i]) {
        int v=to[i];
        if(!dfn[v]) {
            tarjan(v);
            low[x]=min(low[x],low[v]);
        }
        else if(ins[v]) low[x]=min(low[x],low[v]);
    }
    if(dfn[x]==low[x]) {
        cntt++;
        int y;
        do{
            y=_stack[top--];
            ins[y]=0;
            c[y]=cntt;
            scc[cntt].push_back(y);
        }while(x!=y);
    }
}
```

在上述的代码中`scc`存储的的是新的有向无环图。`c`数组存储的是强连通分量。

我们可以通过下面的这段代码建出新的有向无环图。


```cpp
void addedge_c(int u,int v) {
	cnt++;
	to_c[cnt]=v;
	_next_c[cnt]=head_c[u];
	head_c[u]=cnt;
}
//在main()中
for(int x=1; x<=n; x++)
	for(int i=head[x]; i; i=_next[i]) {
		int y=to[i];
		if(c[x]==c[y]) continue;
		add_c(c[x],c[y]);
	}
```

### 对应例题

>Network of Schools[POJ1236](http://poj.org/problem?id=1236)
>
>Popular Cows[POJ2186](http://poj.org/problem?id=2186)
>
>消息扩散[LG2002](https://www.luogu.org/problemnew/show/P2002)

# 无向图连通性

## 一些定义

给定无向连通图$G=(V,E)$：

1. 若对于$x\in V$，从图中删除$x$与其相邻的所有的边，使$G$分裂为两个或两个以上的子图，则称$x$为$G$的割点。

2. 若对于$e\in E$，从图中删除$e$，使$G$分裂为两个或两个以上的子图，则称$e$为$G$的割边或桥。

3. 与有向图相似的$dfn$与$low$。

## Tarjan求割点与桥

### 核心思路

#### 割边判定法则

如果边$(x,y)$是一个桥，当且仅当满足：

$$
\begin{aligned}
dfn_x<low_y
\end{aligned}
$$

有了上述的判定法则，就可以很轻松地在有向图的基础上该写出割边的代码。

特别的，我们可以引用一个$bridge$来存储割边的左右节点。

##### 完整代码

```cpp
void tarjan(int x,int in_edge) {
	dfn[x]=low[x]=++num;
	for(int i=head[x]; i; i=_next[i]) {
		int y=to[i];
		if(!dfn[y]) {
			tarjan(y,i);
			low[x]=min(low[x],low[y]);//与有向图相同的操作
			
			if(low[y]>dfn[x]) bridge[i]=bridge[i^1]=1;//判定为割边
			else if(i!=(in_bridge^1)) low[x]=min(low[x],dfn[y]);
		}
	}
}
```
##### 对应例题

>暂无

#### 割点判定法则

当$x$不是搜索树的根节点，如果$x$是割点当且仅当搜索树上存在$x$的一个子节点$y$。满足：

$$
\begin{aligned}
dfn_x\leq low_y
\end{aligned}
$$

##### 完整代码
```cpp
void tarjan(int x) {
    dfn[x]=low[x]=++num;
    int flag=0;
    for(int i=head[x]; i; i=_next[i]) {
        int y=to[i];
        if(!dfn[y]) {
            tarjan(y);
            low[x]=min(low[x],low[y]);
            if(low[y]>=dfn[x]) {
                flag++;
                if(x!=root||flag>1) cut[x]=1;
            }
        }
        else low[x]=min(low[x],dfn[y]);
    }
}
```
##### 对应例题

>【模板】割点（割顶）[LG3388](https://www.luogu.org/problemnew/show/P3388)

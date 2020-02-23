---
title: '【HDU1520】Anniversary party s题解'
date: 2018-10-10 23:12:23
tags: [题解,动态规划,树形dp,dfs]
categories:
- [OI,题解]   
---

题目链接[here](http://acm.hdu.edu.cn/showproblem.php?pid=1520)，是目前公认的树形dp的入门模板题。

思路还是比较简单的。

<!--more-->

## 题目大意

由若干个人在举行派对，给定你若干对从属关系，要求具有从属关系的两人不能同时参加派对，每个人都有一个权值让你求出权值的最大是多少。

## 核心思路

### 状态设计

因为对于每一个节点都会有两种状态分别为选择和不选择，记$f[i][1]$表示以$i$为根结点的子树当选择了$i$的最大权值和，而$f[i][0]$表示以$i$为根结点的子树当没有选择$i$的最大权值和。

对于任意叶子结点$k$，$f[k][1]$即为该节点的权值，而$f[k][0]$即为0。

### 状态转移方程

得到了上述的关系，我们可以发现，假设在一对从属关系中，“属点”是$u$，“从点”是$v$（即$u$是$v$的领导），将会有一条边从$u$指向$v$，并且当$u$被选择时，$v$就不能被选择，而$u$不被选择时$v$可以不被选择，也可以不选择。

因此方程如下：
$$
\begin{aligned}
f[u][1]&=\sum\max(f[v][0],f[v][1])\\
f[u][0]&=\sum f[v][0]\\
\end{aligned}
$$
所以只需要dfs从根节点向叶子结点遍历，在回溯时修改即可。

### dfs遍历
```cpp
void dfs(int u) {
    int fa=0,tu=a[u];
    for(int i=head[u]; i; i=_next[i]) {
        int v=to[i];
        dfs(v);

       	//切记是在回溯的时候修改
    }
    dp[u][0]=fa,dp[u][1]=tu;
}
```

我这里使用的是链式前向星存图，当然也可以使用邻接表。邻接矩阵有可能会超时。

dfs遍历的过程应该很好理解，在回溯的时候才可以更新答案。入口是`dfs(root)`。

### 寻找根节点

根据上面所说的需要寻找根节点，根据根节点的定义，入度为0的节点即为根节点。

在存图的时候加入一个数组记录下每个节点的入度即可。

值得注意的是有些题目会有多棵树，而有些题会明确指出只有一棵树，因此使用下面的代码最为保险。

```cpp
for(int i=1; i<=n; i++) {
	if(!root[i]) {
		dfs(i);
        //统计答案
	}
}
```

## 完整代码

```cpp
#include<bits/stdc++.h>
using namespace std;
#define MAXN 6000+15
#define MAXM 6000+15
int read() {
    int x=0,f=1;
    char ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch<='9'&&ch>='0'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

int head[MAXN],to[MAXM],a[MAXM],_next[MAXN],root[MAXN],cnt;

void addedge(int u,int v) {
    cnt++;
    to[cnt]=v;
    _next[cnt]=head[u];
    head[u]=cnt;
}

int dp[MAXN][2];

void init() {
    memset(root,0,sizeof(root));
    memset(head,0,sizeof(head));
    memset(dp,0,sizeof(dp));
    cnt=0;
}

void dfs(int u) {
    int fa=0,tu=a[u];
    for(int i=head[u]; i; i=_next[i]) {
        int v=to[i];
        dfs(v);

        fa=max(fa,fa+max(dp[v][0],dp[v][1]));
        tu=max(tu,tu+dp[v][0]);
    }
    dp[u][0]=fa,dp[u][1]=tu;
}

int main() {
    int n;
    while(scanf("%d",&n)!=EOF) {
        init();
        for(int i=1; i<=n; i++) a[i]=read();    
        int u,v;
        while(scanf("%d%d",&u,&v)==2&&u+v) {
            addedge(v,u);
            root[u]=1;
        }
        int ans=0;
        for(int i=1; i<=n; i++) {
            if(!root[i]) {
                dfs(i);
                ans+=max(dp[i][0],dp[i][1]);
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```


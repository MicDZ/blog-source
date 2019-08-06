---
title: 树状数组基础
date: 2018-09-10 23:00:42
tags: [随笔,笔记,数据结构]
categories:
- OI   
---

树状数组是一个非常高效的支持区间修改单点查询的数据结构。

<!--more-->

# 引入

线段树和树状数组，是两个十分相似的数据结构。他们能使对一个区间的数修改以及查询的速度提升许多。两个结构本质相同，各有优缺点，今天我们来从单点修改，单点查询，区间修改，区间查询进行了解。

# 内容及存储方式

## 树的形式

![](https://i.loli.net/2018/09/10/5b968fd653fed.jpg)



$A$数组是原数组，$C$数组是树状数组。

$C$树状数组中的每一个元素都是其所有叶节点的总和。特别的，叶子结点就是对应的原数组。
## 数的规律

```cpp
c1=a1;
c2=a1+a2;
c3=a3;
c4=a1+a2+a3+a4;
```

根据树状数组的定义，可以得到如上。没有必要纠结为什么是这样，只需要知道这样有什么用，因为这个定义也是人为定义出来的。

那么，这里的规律究竟是怎么样的？

将上述表格中的下标转化为二进制后即可发现规律。

```cpp
c0001=a0001
c0010=a0001+a0010
c0011=a0011
c0100=a0001+a0010+a0011+a0100
```

从左向右数，$C$数组下标有$n$个0就有$2^n$个元素。



# 核心代码

## 单点更新

```cpp
void change(int x,int v) {
    while(x<=n) {
        t[x]+=v;
        x+=x&(-x);
    }
}
```

利用此操作，将第$x$个元素增加v。

## 前缀查询

```cpp
int ask(int x) {
    int ans=0;
    while(x) {
        ans+=t[x];
        x-=x&(-x);
    }
    return ans;
}
```

得到的结果是前$x$个元素的和。

## 区间修改

### 差分存储

我们将原数组的差分数组插入到树状数组中。

例如$[2,6,9,1,3,4]$的差分数组为$[2,4,3,-8,2,1]$，可以看出，差分数组$b_i=a_i-a_{i-1}$。

这样的好处是查找某个元素可以直接树状数组求前缀和。

### 差分修改

如果要将从$[l,r]$的所有元素加上$x$，只需要将$b_l+x$和$b_{r+1}-x$。在求前缀和时，只会对$[l,r]$有影响。而对后面和前面的数没有影响。

为了节省空间，可以如下进行插入：

```cpp
for(int i=1; i<=n; i++) {
    a=read();
    change(i,a-b);
    b=a;
}
```



# 完整代码

[ LG3368 【模板】树状数组 2](https://www.luogu.org/problemnew/show/P3368)

```cpp
#include<iostream>
#include<cstdio>

using namespace std;

#define MAXN 500000+10
#define ll long long

int read() {
    int x=0,f=1;
    char ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch<='9'&&ch>='0'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

int n,m;

int a,b,t[MAXN];

void change(int x,int v) {
    while(x<=n) {
        t[x]+=v;
        x+=x&(-x);
    }
}

int ask(int x) {
    int ans=0;
    while(x) {
        ans+=t[x];
        x-=x&(-x);
    }
    return ans;
}

int main() {
    n=read(),m=read();
    for(int i=1; i<=n; i++) {
        a=read();
        change(i,a-b);
        b=a;
    }
    for(int i=1; i<=m; i++) {
        int pd=read();
        if(pd==1) {
            int x=read(),y=read(),k=read();
            change(x,k);
            change(y+1,-k);
        }
        else {
            int x=read();
            printf("%d\n",ask(x));
        }
    }
}
```






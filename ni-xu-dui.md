---
title: 树状数组与逆序对
date: 2018-11-09 16:10:53
tags: [笔记,随笔]
categories:
- OI   
---

在NOIp来之前，赶快奶一口会出逆序对毒瘤数学题，RP++。

<!--more-->

在前几年的NOIp的考试中，已经有很久没有出现过与逆序对有关的题目了。但是正因如此，逆序对在这次NOIp出现的几率就大大提高了。

## 核心思路

计算逆序对可以通过归并排序或桶排+数据结构优化的思路求解。

又因为归并排序的应用范围不广，因此我更倾向于学习桶排+数据结构优化的方法求逆序对。

下面给出一个样例：

```
1 8 2 1 4 14 2
```

如果我们以桶排的思路依次存下每一个元素出现的数量，那么我们只需要求出在它之前的比他大的元素数量，最后求和即可。

因此我们可以对原数列离散化成：

```
1 4 2 1 3 5 2
```

对于这个数列，例如我们在计算以第4个`1`为右端点的逆序对的时候，我们只需要统计从第1个到第3个数字中大于`1`的数的个数。

这里可应用桶排的思想，从左向右扫，将每一个数加入对应的桶中，再用树状数组对小于当前的元素的桶维护前缀和即可。这样动态单点修改，区间查询就成为了树状数组的强项，可以用树状数组优化到$\log$级。


## 完整代码

同样也可以用线段树，这里只有树状数组的代码。

解释见注释。

```cpp
#include<bits/stdc++.h>
using namespace std;

#define MAXN 500000+10
#define lowbit(x) (x&(-x))
#define REP(i,e,s) for(register int i=e; i<=s; i++)
#define ll long long
int read() {
    int x=0,f=1,ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

int c[MAXN],temp[MAXN],n;

struct edge {
    int id,num;
} a[MAXN];

bool cmp(edge q,edge w) {
    if(q.num==w.num)
        return q.id<w.id;
    return q.num<w.num;
}//使用stable_sort可以达到同样效果。

void add(int x,int v) {
    while(x<=n) {
        c[x]+=v;
        x+=lowbit(x);
    }
}

int sum(int x) {
    int ans=0;
    while(x) {
        ans+=c[x];
        x-=lowbit(x);
    }
    return ans;
}

int main() {
    n=read();
    REP(i,1,n) {
        a[i].num=read();
        a[i].id=i;
    }
    
    sort(a+1,a+1+n,cmp);
    //stable_sort(a+1,a+1+n,cmp);
    
	REP(i,1,n)
        temp[a[i].id]=i;//temp存离散化后的值
        
    ll ans=0;
    REP(i,1,n) {
        add(temp[i],1);//将temp[i]加1
        ans+=i-sum(temp[i]);//i为在这之前的数字总个数，sum(temp[i])表示就目前为止小于等于temp[i]的个数。
    }
    printf("%lld\n",ans);
    return 0;
}
```

# 小结

这是一种思路简单，代码实现简易的逆序对求法。唯一的不足就是~~常数~~可能会比较大。

复杂度为$\Theta(3\times n\log n)$。
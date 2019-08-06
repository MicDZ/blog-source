---
title: 【LG2893】 修路
date: 2018-08-20 22:22:56
tags: [题解,动态规划,贪心]
categories:
- OI   
---

在最优解的排行榜中看到了一种神奇的方法。

**声明**：此题解的思路来源于洛谷代码公开计划。

<!--more-->

# 核心思路

这是一个DP的裸题，然而。。总有一些神人用神奇的方法解决了问题。

首先建一个大根堆和一个小根堆，将每一个的路面高度push进去，这里可以用STL的优先队列。

用大根堆模拟递增的情况，小根堆模拟递减的情况。

接下来利用一种类似于贪心的方法解决这道题。

因为这里有一个结论：修改后的道路高度在原来的道路的高度中。

那么面对每一次插入的高度，如果它的高度要比小根堆中已插入的最小的高度要大的话，那么就可以把它休整到和最小的同样高度。同理，反之是相反的操作。

这样尽管不能在每次操作的时候确定当前路段的花费，但是总和确是确定的。

给张图理解下：

![](https://i.loli.net/2018/08/19/5b7930629f335.png)


# 完整代码
```cpp
#include<bits/stdc++.h>

using namespace std;

priority_queue<int, vector<int>, greater<int> > q;
priority_queue<int> p;
int ans1,ans2;

int main(){
    int n=read();
    for(int i=1; i<=n; i++) {
        cin>>x;
        p.push(x);
        q.push(x);
        if(q.top()<x) {
            ans1+=abs(q.top()-x);//这里要加一个abs
			q.pop();
			q.push(x);
        }  
		if(p.top()>x) {
            ans2+=p.top()-x;
			p.pop();
			p.push(x);
        }
    }
    printf("%d\n",min(ans1,ans2));
    return 0;
}
```

欢迎批评指正！
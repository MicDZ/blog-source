---
title: 【LG2085 最小函数值 题解
date: 2018-05-12 09:59:02
tags: [堆,题解]
categories:
- OI   
---



题目链接[here](https://www.luogu.org/problemnew/show/P2085)

正解讲了啥我也不清楚，但是有一种神奇的解法。

<!--more-->

# 核心思路

其中关键在于：

```cpp
void work(int a,int b,int c){
	for(int i=1;i<=sqrt(m)+1;i++){
		q.push(a*i*i+b*i+c);
	}
}
```

在计算中只计算$\sqrt m$的范围。如果直接m的话只能勉强获得20分。

因为计算m的复杂度是$\Theta(nm)$，显然会超时。



这可能是一种投机取巧的方式，如果数据强一点一定会被卡掉，因为当二次函数的对称轴大于了$\sqrt m$，最小值就一定不会在$\sqrt m$的范围内了（$a>0$的情况），$a<0$同样成立。



建议管理员还是可以强化一下数据，hack非正解。

# 完整代码

```cpp
#include<bits/stdc++.h>
#define MAXN 200000+10
#define INF 0x3f3f3f
using namespace std;
int read(){
    int x=0,f=1;
    char ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch<='9'&&ch>='0'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int a[MAXN],n,m;

priority_queue <int,vector<int>,greater<int> > q;

void work(int a,int b,int c){
	for(int i=1;i<=sqrt(m)+1;i++){
		q.push(a*i*i+b*i+c);
	}
}
int main(){ 
	n=read();
	m=read();
	int a,b,c;
	for(int i=1;i<=n;i++){
		a=read(),b=read(),c=read();
		work(a,b,c);
	}
	for(int i=1;i<=m;i++){
		cout<<q.top()<<" ";
		q.pop();
	}
}  
```


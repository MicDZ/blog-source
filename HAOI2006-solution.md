---
title: '【HAOI2006】聪明的猴子 题解'
date: 2018-05-05 16:52:38
tags: [题解,kruskal]
categories:
- OI   
---

题目链接[here](https://www.lydsy.com/JudgeOnline/problem.php?id=2429)

题目不难，普及/提高-

<!--more-->

# 核心思路

> 找到最小生成树的。
> 
> 记录下最小生成树的最大边。
> 
> 比较每一只猴子的跳跃最远距离与最大边的关系，可以跳过则ans++。

## 易错点

看上去很简单，可以直接将[最小生成树](https://douglas-zhou.cn/2018/04/28/%E6%9C%80%E5%B0%8F%E7%94%9F%E6%88%90%E6%A0%91/)直接拿过来套模板，我就是这样做的，然而：

2AC，3WA，5RE。

我开始找问题，经历了大概两天的时间，我总结出了以下这些坑点，作为一个刚开始做图论题的蒟蒻容易错的一些坑点。

​	1. 套模板时忘记把n改为cnt，cnt指的是在图中的n个点与其它点构成的$\frac{n\times (n-1)}{2}$条边。

​	2. 忘记将struct存下的边开大到$\frac{n\times (n-1)}{2}$，在题目中也就是大约1000000。

​	3. f[]数组没有完全初始化。

# 代码

由于套模板，这里的代码非常丑陋。

简单的读入

```cpp
int m=read();
for(int i=1;i<=m;i++)hd[i]=read();
int n=read();
for(int i=1;i<=n;i++)x[i]=read(),y[i]=read();
```

处理每一条边

```cpp
for(int i=1;i<=n;i++)
for(int j=i+1;j<=n;j++){
	cnt++;
	a[cnt].u=i;
	a[cnt].v=j;
	a[cnt].s=dist(x[i],x[j],y[i],y[j]);
}
```

非常丑陋的核心代码

```cpp
sort(a+1,a+1+cnt,cmp);
for(int i=1;i<=MAXN;i++)f[i]=i;
int ans=0,s=0;
for(int i=1;i<=cnt;i++){
	if(find(a[i].u)!=find(a[i].v)){
		link(a[i].u,a[i].v);
		s++;
		if(a[i].s>maxx)maxx=a[i].s;
		if(s==n-1){//找到答案了
			int shu=0;
			//cout<<maxx;
			for(int i=1;i<=m;i++)if(hd[i]>=maxx)shu++;//计数
			cout<<shu<<endl;                
			return 0;
		}
	}
}
```

完整代码[here](https://douglas-zhou.cn/code/%E8%81%AA%E6%98%8E%E7%9A%84%E7%8C%B4%E5%AD%902)
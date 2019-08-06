---
title: 最小生成树
date: 2018-04-28 23:53:41
tags: [笔记,题解,kruskal,贪心]
categories:
- OI   
---





最小生成树的主要思路是贪心，用到了并查集的知识。

<!--more-->

![](/img/4.jpg)

# 定义



> 什么是最小生成树？
>
> 给定一个图$G<V,E>$，$G'<v,e>$的$v\in V,e\in E$，且是一棵树时就可称$G'$是$G$的生成树。
>
> 最小生成树的定义就更清晰了。
>
> 即在$G$所有的生成树中，边权之和最小的一棵

[题目链接](https://www.luogu.org/problemnew/show/P3366)







考虑用Kruskal算法，尽管离线但是效率感人。

看上去很麻烦？其实不然，只是简单的贪心罢了。



# 核心思路

```
将所有的点按照边权从小到大排列。
找到最小的一条，如果首尾不在同一子树中则相连。
如果在则舍弃。
当连完n-1条边则可输出答案。
```

# 代码

代码并不难理解：

并查集部分

```cpp
int find(int x){
	if(f[x]==x)return f[x];
	return f[x]=find(f[x]);
}
int link(int x,int y){
	f[find(x)]=find(y);
}
```

用`point`存边，用cmp作排序规则

```cpp
struct point{
	int u,v,s;
}a[MAXN];

bool cmp(point a,point b){
	return a.s<b.s;
}
```

读入的方式很简单

```cpp
int n=read(),m=read();
for(int i=1;i<=m;i++)
	a[i].u=read(),a[i].v=read(),a[i].s=read();
```

就代表着a[i].u到a[i].v有一条边权为a[i].s的边。



核心代码

```cpp
sort(a+1,a+1+m,cmp);//将边排序
for(int i=1;i<=m;i++)f[i]=i;//并查集初始化
ans=0,s=0;//ans存答案，s存步骤
for(int i=1;i<=m;i++){
	if(find(a[i].u)!=find(a[i].v)){//如果不在同一子树
		link(a[i].u,a[i].v),ans+=a[i].s;//连接在一起，将ans更新
		s++;//将s更新
		if(s==n-1){//有了n-1条边
			cout<<ans;//直接输出答案
			return 0;//结束
		}
	}
}
```



完整代码[here](https://douglas-zhou.cn/code/%E6%9C%80%E5%B0%8F%E7%94%9F%E6%88%90%E6%A0%91)

最小生成树不难理解，接下来还会有次小生成树等待学习。


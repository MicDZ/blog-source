---
title: 简单的背包问题
date: 2018-04-18 22:48:46
tags: [动态规划,笔记,随笔]
categories:
- OI   
---

背包问题是非常典型的动态规划问题。

包括01背包，完全背包，多重背包……

<!--more-->

# 01背包问题

有$n$件物品，最大载荷为$m$的背包，$f_{i,j}$表示有前$i$个物品，总量为$j$的情况的最优解

## 状态转移方程
$$
f_{i+1,j} =
\left\{
\begin{aligned}
\max (f_{i,j},f_{i,j-w_i+1}+v_i+1)\quad  w_{i+1}\leq j\\
{f_{i,j} \quad w_{i+1}> j}\\
\end{aligned}
\right.
$$

## 代码

对应的代码就很简单了：

```cpp
for(int i=0;i<=m;i++)
	f[0][i]=0;
for(int i=1;i<=n;i++)
	for(int j=0;j<=m;j++){
		if(j>=w[i])
			f[i][j]=max(f[i-1][j],f[i-1][j-w[i]]+v[i]);
		else f[i][j]=f[i-1][j];
    }
```

利用动态数组优化后的代码为：

```cpp
for(int i=0;i<=m;i++)
	f[i]=0;
for(int i=1;i<=n;i++)
	for(int j=m;j>=m;j--){
		if(j>=w[i])
			f[j]=max(f[j],f[j-w[i]]+v[i]);
		else f[j]=f[j];
    }
```

## 复杂度

时间复杂度$\Theta(nm)$，空间线性。



# 完全背包问题

与01背包不同的是，每一个物品都有无数件

## 代码 

```cpp
for(int i=0;i<=m;i++)
	f[i]=0;
for(int i=1;i<=n;i++)
	for(int j=1;j<=m;j++){
		if(j>=w[i])
			f[j]=max(f[j],f[j-w[i]]+v[i]);
		else f[j]=f[j];
	}
```

内层循环正序即可实现。

## 复杂度

时间复杂度$\Theta(nm)$，空间线性。


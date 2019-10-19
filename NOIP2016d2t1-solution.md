---
title: 【NOIP2016】组合数问题 题解
date: 2018-07-26 14:32:19
tags: [题解,随笔,数学]
categories:
- OI   
---



NOIP2016中除了玩具谜题这道大水题之外，稍难一点的就是这道题了。

<!--more-->

[题目链接](https://www.luogu.org/problemnew/show/P2822)

# 核心思路

> 用杨辉三角存储下组合数的值，利用二维前缀和累加。代码实现非常简单，思维难度仅限于杨辉三角的部分。

不要被题面所迷惑了，都是骗人的，使用公式最多30分（代码完成度非常高且非常鲁棒）。

## 杨辉三角

```
       1
      1 1
     1 2 1
    1 3 3 1
   1 4 6 4 1
  ……       ……
```

上述就是最基础的杨辉三角。

你可以很轻松地发现第$n$行的第$k$个数字为[组合数](https://zh.wikipedia.org/wiki/%E7%BB%84%E5%90%88%E6%95%B0) $C^{k-1}_{n-1}$

具体的论证过程参照[here](https://zh.wikipedia.org/wiki/%E6%9D%A8%E8%BE%89%E4%B8%89%E8%A7%92%E5%BD%A2)

这样我们就可以$\Theta(2000\times \sqrt{2000})$把所有的组合数的值求出来了



## 二维前缀和

可以参照我的[这篇文章](https://www.micdz.cn/article/basic-ds/)。

这里的转移方程是

```cpp
s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1];
```

s(i,j)表示题意中的n，m所对应的对数。



# 完整代码

```cpp
#include<bits/stdc++.h>
#define MAXN 2000+10

using namespace std;

int read() {//快读略
}
int a[MAXN][MAXN],s[MAXN][MAXN];
int main() {
    int t=read(),k=read();
    for(int i=0;i<=2000;i++) {
        a[i][i]=1;
        a[i][0]=1;
    }
    for(int i=1; i<=2000; i++)
        for(int j=1; j<i; j++)
            a[i][j]=(a[i-1][j]+a[i-1][j-1])%k;//在计算的时候直接%k可以避免后面无效的重复计算
    for(int i=1; i<=2000; i++)
        for(int j=1; j<=2000; j++) {
            s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1];//前缀和口诀：上加左 减左上 加自己
            if(a[i][j]==0&&j<=i) s[i][j]++;//如果a[i][j]满足要求则++
        }
    for(int i=1; i<=t ;i++) {
        int n=read(),m=read();
        printf("%d\n",s[n][m]);
    }
    return 0;
}
```


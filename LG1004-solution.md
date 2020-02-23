---
title: '【LG1004】方格取数 题解'
date: 2018-08-01 08:27:59
tags: [题解,动态规划]
categories:
-    
---

这是一道四维DP的模版题，思维难度也不大，题目链接[here](https://www.luogu.org/problemnew/show/P1004)

<!--more-->

## 核心思路

定义$dp[i][j][k][l]$为第一次走到$(i,j)$第二次走到$(k,l)$的最优方案，$a[i][j]$为放在$(i,j)$处的数字。

由于每一步只能从$i-1$、$j-1$、$k-1$、$l-1$转移

状态转移方程就很好推出了：

$dp[i][j][k][l]=\max(dp[i-1][j][k-1][l],max(dp[i][j-1][k-1][l],dp[i][j-1][k][l-1],dp[i-1][j][k][l-1])++a[i][j]+a[k][l]$

要尤其注意$(i,j)$和$(k,l)$相同的情况



## 完整代码

```cpp
#include<bits/stdc++.h>
using namespace std;

#define MAXN 10+5

int read() {
    int x=0,f=1;
    char ch=getchar();
    while(ch>'9'||ch<'0'){if(ch=='-')f=-1;ch=getchar();}
    while(ch<='9'&&ch>='0'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}
int dp[MAXN][MAXN][MAXN][MAXN],a[MAXN][MAXN];

int main() {
    int n=read();
    while(1) {
        int x,y,s;
        x=read(),y=read(),s=read();
        if(x==0&&y==0&&s==0) break;
        a[x][y]=s;
    }
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)
            for(int k=1; k<=n; k++)
                for(int l=1; l<=n; l++) {
                    dp[i][j][k][l]=max(dp[i-1][j][k-1][l],max(dp[i][j-1][k-1][l],max(dp[i][j-1][k][l-1],dp[i-1][j][k][l-1])))+a[i][j]+a[k][l];

                    if(i==k&&j==l) dp[i][j][k][l]-=a[i][j];
    }

    printf("%d",dp[n][n][n][n]);
}
```

